# Evidence

## Purpose

This directory will hold the supporting material collected during authorised testing in PortSwigger Web Security Academy.

Evidence should allow the observations and conclusions in each finding record to be reviewed. It should provide enough context to understand what was tested, what changed and what the application returned.

**Current status:** Draft structure. No practical test evidence has been collected yet.

## Evidence types

Depending on the lab and the test performed, evidence may include:

- Screenshots showing relevant application behaviour.
- Sanitised HTTP requests and responses.
- Burp Repeater captures or exported items where suitable.
- Notes recording test conditions, timestamps and observed results.
- Before and after comparisons used to support a retest.

Only collect material that is relevant to the test. Avoid storing complete browser sessions or unnecessary application data.

## Suggested directory structure

Create a separate directory for each lab or finding where this makes the evidence easier to identify.

```text
Evidence/
├── README.md
├── lab-topic/
│   └── lab-name/
│       ├── 001-baseline-request.txt
│       ├── 002-test-request.txt
│       ├── 003-test-response.txt
│       └── 004-observed-behaviour.png
└── finding-001/
    └── retest/
        └── 001-retest-result.png
```

The structure is a suggested starting point. It can be adjusted once the volume and type of evidence become clearer.

## File naming

Use descriptive filenames with a sequence number where several artefacts belong to the same test.

For example:

- `001-baseline-request.txt`
- `002-modified-request.txt`
- `003-response-after-test.txt`
- `004-access-result.png`

Avoid vague names such as `image1.png` or `capture-final.png`.

## Evidence record

For each group of related evidence, record the following in the relevant finding or a short accompanying note:

| Field | Details |
|---|---|
| Evidence reference | Filename or relative path |
| Related lab | Academy lab title |
| Related finding | Finding identifier, if applicable |
| Date and time | When the evidence was collected |
| Tool | Browser, Burp component or other tool used |
| Test stage | Baseline, controlled test, validation or retest |
| Description | What the artefact demonstrates |
| Limitations | Missing context or factors that may affect interpretation |

## Handling and sanitisation

Before committing any evidence to GitHub:

1. Remove passwords, API keys, cookies, bearer tokens and other reusable session material.
2. Remove personal data and unrelated account information.
3. Check screenshots for sensitive values visible in browser tabs, address bars, page content or developer tools.
4. Check exported requests and responses for authentication headers, session identifiers and hidden secrets.
5. Retain only the request and response content needed to support the observation.
6. Confirm that the material comes from an authorised Academy lab or another explicitly authorised environment.

Re-review it before every commit.

## Evidence quality

Evidence should be:

- **Relevant:** directly connected to the test hypothesis or finding.
- **Understandable:** accompanied by enough context to explain what it shows.
- **Reproducible:** supported by a record of the test conditions and steps.
- **Consistent:** referenced by the corresponding finding record.
- **Sanitised:** free from credentials, personal data and reusable secrets.

A screenshot or response by itself may not establish the cause of an observed behaviour. Where necessary, retain the baseline and comparison evidence and explain alternative interpretations in the finding record.

This document is a draft. The evidence structure and recording requirements will be updated as the practical labs reveal what is needed.
