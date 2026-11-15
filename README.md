# Lab-1-Project-A-Web-Application-Security-PortSwigger-

# PortSwigger Web Security Academy  Objective and Methodology Report

**Project:** Web Application Security Testing  
**Platform:** PortSwigger Web Security Academy  
**Report status:** Initial project planning                                         
**Prepared:** 31 October 2026  
**Author:** Clinton Nkpa

---

## 1. Purpose and objective

The objective of this project is to develop and demonstrate a practical understanding of common web application security weaknesses through structured, authorised testing in PortSwigger Web Security Academy.

The labs work focuses on how vulnerabilities arise in real HTTP request/response flows, how they can be identified and validated, what an attacker could achieve if they were present, and what changes would prevent or reduce the risk. The aim is not simply to complete a collection of Academy exercises, but to build a repeatable testing process and produce evidence-backed findings that can be explained clearly.

The initial emphasis will be understanding application behaviour before attempting to manipulate it. Requests, responses, parameters, cookies, tokens, redirects and access decisions will be examined in context. Where a weakness is identified, testing will remain within the deliberately vulnerable Academy environment.

The project is intended to provide practical evidence relevant to a junior penetration tester, web application security testing and application security analyst roles.

## 2. Scope

The initial scope is PortSwigger Web Security Academy and its deliberately provided training labs. Testing will be limited to Academy exercises and any local test components explicitly introduced for this project.

Planned subject areas:

1. HTTP fundamentals and request/response analysis.
2. Authentication and session management.
3. Access control, including insecure direct object reference (IDOR) patterns.
4. Injection vulnerabilities, including SQL injection where covered by selected labs.
5. Path traversal and file-handling weaknesses.
6. Evidence capture, impact assessment, remediation recommendations and retesting.

Order subject to change as practical workflow reveals hidden dependencies or likely knowledge gaps. Exact labs completed, difficulties and outcomes will be recorded accordingly.

**Out of scope:** Though it goes without saying, public websites, systems belonging to other people or organisations, and any explicitly authorised  target has not been provided beyond the scope of what Portswigger Academy Web Security provides. Academy techniques will  therefore not be transferred to live targets without clear lawful permissions.

## 3. Tools and resources

| Tool/resource | Intended use | Status |
|---|---|---|
| PortSwigger Web Security Academy | Structured learning material and authorised practice labs | Selected |
| Burp Suite Community Edition | Intercepting, inspecting, modifying and replaying HTTP traffic | Planned |
| Firefox or Chromium | Browser used to access labs and route traffic through Burp | Planned |
| Markdown notes and evidence folders | Recording steps, observations, evidence references and conclusions | To be established |
| OWASP Web Security Testing Guide (WSTG) | Supplementary methodology and terminology where useful | Optional |
| OWASP ZAP | Alternative proxy/scanner for comparison where appropriate | Optional; not required initially |

The initial workflow is deliberately based on a browser and an intercepting proxy. Additional tools will be introduced only where they answer a specific testing question. Automated scanning will not replace manual validation.

## 4. Method of execution

Each exercise follows a consistent sequence  for ease of reproduction of results and so observations are separated from assumptions.

### 4.1 Establish the test context
- Record the Academy topic, lab title, stated objective and difficulty.
- Confirm that the target is an Academy-provided lab instance.
- Note relevant application functionality, test roles/accounts and supplied data.
- Record browser and Burp versions where practical.

### 4.2 Establish a baseline
- Browse any relevant functionality without modifying requests.
- Identify the normal sequence of requests and responses.
- Record relevant methods, paths, parameters, cookies, headers, tokens, status codes and response behaviour.
- Establish baseline parameters of a normal authorised action before testing for a weakness.

### 4.3 Form a test hypothesis
State what control may be missing or incorrectly implemented and what observable result would support that hypothesis. For example, an access-control test might ask whether changing an object identifier allows one test user to access an object belonging to another test user.

The hypothesis should be specific enough to be disproved. A response that differs from the baseline is an observation; it is not automatically proof of a vulnerability.

### 4.4 Conduct a controlled test
- Use Burp Proxy to inspect relevant traffic.
- Use Repeater or equivalent manual request replay to change one meaningful variable at a time.
- Compare the modified request and response with the baseline.
- Keep tests within the lab's intended scope and avoid unnecessary request volume.
- Record exact request changes and resulting behaviour, sanitise session material before storing evidence.

### 4.5 Validate and assess impact
Where a suspected weakness appears, repeat the test where possible and consider alternative explanations. Establish what the behaviour demonstrates and what it does not. Describe impact in the specific lab context rather than assuming every similar production system would be affected identically.

### 4.6 Document remediation and retest
For each validated finding:
- Explain the underlying control failure.
- Recommend a mitigation that addresses the cause, not only the observed payload.
- Identify relevant server-side enforcement, validation, session or logging improvements.
- Retest the same behaviour after mitigation, if the lab provides a way to do so.
- Record whether the retest confirms the mitigation, partially supports it or remains inconclusive, partially rejects it, or completely rejects it.

### 4.7 Produce a finding record
Each finding highlights associated labs and titles, preconditions and test-role context (without credentials), baseline behaviour, hypothesis, steps performed, sanitised evidence, observed result, impact and limitations, alternative interpretations, remediation, and retest result/status.

## 5. Expected constraints and methodological limitations

These are anticipated constraints to test against, before any findings are observed.

| Expected constraint | Possible effect | Planned response |
|---|---|---|
| Academy labs are intentionally scoped and simplified | Results may not represent production complexity or controls | State the lab context and avoid generalising beyond evidence |
| Lab instances may reset or expire | Test state and evidence may be lost | Save sanitised notes and screenshots promptly; record reset conditions |
| Burp Community has fewer features than Professional | Some workflow conveniences and automated capabilities may be unavailable | Prioritise manual Proxy and Repeater testing; note material limitations |
| Browser/proxy configuration affects traffic capture | Missing requests may be mistaken for application behaviour | Verify proxy settings, interception state and browser routing |
| Academy instructions may constrain the intended solution | A lab may reward a particular exploit chain rather than cover every variant | Separate the lab success condition from broader security conclusions |
| Shared lab infrastructure or network conditions affect timing | Delays, resets or intermittent responses may reduce repeatability | Repeat tests and record timestamps/environmental conditions |
| Automated tooling can produce false positives or miss context | Scanner output may not establish exploitability or business impact | Manually validate tool-generated results |

## 6. Future observed constraints — expected versus reality

This section will be updated with observed constraints and compared with the expectations above.

| Area | Expected constraint | Observed constraint / evidence | Effect on method/result | Adjustment made | Status |
|---|---|---|---|---|---|
| Browser and proxy | Configuration may prevent complete capture | To be recorded during testing | To be assessed | To be recorded | observed |
| Burp Community | Feature limitations may affect workflow | To be recorded during testing | To be assessed | To be recorded | Not observed |
| Academy environment | Reset/expiry or simplified behaviour may affect repeatability | To be recorded during testing | To be assessed | To be recorded | observed |
| Test methodology | Lab's expected path may not represent all variants | To be recorded during testing | To be assessed | To be recorded | observed |
| Evidence capture | Sanitisation or missing context may limit interpretation | To be recorded during testing | To be assessed | To be recorded | Not observed |

A difference between expectation and reality is not automatically a failure. A constraint may be absent, more significant than expected, or caused by an unaccounted or external factor rather than the application itself.

## 7. Alternative interpretations of evidence

This section will be completed alongside individual findings in order to prevent a premature conclusion based on one response or an incomplete capture.

Potential alternative explanations include:

- **Authentication versus authorisation:** a request may be authenticated successfully while access to a particular object is correctly denied. A successful response alone does not establish that the correct user or role was authorised.
- **Application versus intermediary behaviour:** a status code, redirect or blocked request may originate from the application, proxy, browser control or another intermediary.
- **Session state:** an unexpected response may result from an expired, missing or mismatched cookie/token rather than the vulnerability being tested.
- **Caching and stale state:** a response may reflect cached content or an earlier application state rather than a fresh authorisation decision.
- **Input handling versus business logic:** a rejected payload may indicate validation, parsing behaviour or a business-rule restriction; further evidence is needed to identify the control responsible.
- **Lab-specific behaviour:** an outcome may be deliberately engineered for the exercise and may not map directly to a production implementation.

For each finding, the report will identify the interpretation best supported by the evidence, note credible alternatives, and state what additional test could distinguish between them.

## 8. Evidence handling and reporting standard

Evidence will be stored in a structured, sanitised form. Screenshots and request/response captures should show the relevant behaviour without exposing passwords, live tokens, personal data or unrelated session information.

repository structure:

```text
PortSwigger/
├── README.md
├── PortSwigger_Objective_Report.md
├── PortSwigger_Objective_Report.docx
├── evidence/
│   └── .gitkeep
└── findings/
    └── .gitkeep
```

Use descriptive filenames that identify the lab and evidence sequence. Keep raw secrets and reusable session material out of Git. Review all evidence and notes for credentials, cookies, bearer tokens, personal data and other sensitive values before committing.

## 9. Final conclusions

**Initial conclusion — project not yet executed**

No vulnerability findings are being claimed at this stage. The project is at the planning and setup stage. The methodology is designed to progress from normal application behaviour to a testable hypothesis, controlled request manipulation, evidence-based validation and a defensible remediation recommendation.

The value of the project will come from the quality of the reasoning and evidence, not simply the number of Academy labs marked as solved. A successful lab completion will be recorded as an exercise outcome; it will only be described as a validated security finding where the evidence supports that conclusion.

A final conclusion will be written after testing has produced enough evidence to assess the areas covered, recurring weaknesses, constraints encountered and effectiveness of the testing method.

## 10. Future labs and work that will supplement the conclusions

These exercises are intended to extend and cross-check the skills developed here. They are planned follow-on work, not part of the current completed scope.

| Follow-on lab/project | How it supplements this project |
|---|---|
| OWASP Juice Shop | Provides a deliberately vulnerable application in a more integrated context and allows comparison of findings and workflow |
| OWASP crAPI | Extends work into API-specific weaknesses, including object-level authorisation, authentication and excessive data exposure |
| AWS API Security and Detection lab | Provides a defensive counterpart: API authentication/authorisation, request validation, WAF, logging, monitoring and controlled detection tests |
| AWS cloud attack-path assessment | Adds cloud identity, network and service-permission context to application security |
| MITRE ATT&CK adversary emulation and detection engineering | Connects controlled attack behaviour with telemetry, detection logic, triage and response |

The AWS API Security and Detection project will remain in its own report and repository. Cross-references will be added only when both workstreams contain enough practical evidence to compare a specific offensive behaviour with a corresponding defensive control or detection.

## 11. Initial project status

| Item | Status |
|---|---|
| Project objective and scope | Defined |
| Initial toolset | Selected / planned |
| Testing methodology | Defined |
| Expected constraints | Documented as hypotheses |
| Observed constraints | Not yet assessed |
| Practical Academy testing | Not yet started |
| Validated findings | None claimed |
| Final conclusions | Pending execution |

