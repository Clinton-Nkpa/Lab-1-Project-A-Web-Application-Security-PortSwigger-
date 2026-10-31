# Findings

## Purpose

This directory will contain the documented findings produced during the PortSwigger Web Security Academy project.

Each finding will be recorded separately so that the test process, supporting evidence, impact assessment and remediation can be reviewed without relying on the overall project report.

**Current status:** Draft structure. Practical testing has not yet started, and no vulnerabilities have been confirmed.

## Finding records

A separate Markdown file should be created for each validated finding. Use a descriptive filename that identifies the vulnerability and, where useful, the relevant Academy lab.

Suggested format:

```text
Findings/
├── README.md
├── finding-001-descriptive-title.md
└── finding-002-descriptive-title.md
```

The numbering is intended to keep records organised. It does not indicate severity or priority.

## Finding record template

Copy this structure when documenting a finding:

### Finding [number]: [title]

| Field | Details |
|---|---|
| Academy topic | To be completed |
| Lab title | To be completed |
| Date tested | To be completed |
| Finding status | Not yet assessed |
| Severity | To be assessed from evidence and impact |

#### 1. Summary

Describe the behaviour observed and the security issue being investigated. Do not describe a suspected issue as confirmed until the evidence supports that conclusion.

#### 2. Test context

Record the relevant application functionality, test roles, accounts and preconditions. Do not include passwords, live tokens or reusable session material.

#### 3. Baseline behaviour

Describe the expected behaviour before the test. Include relevant request and response details.

#### 4. Hypothesis

State the control believed to be missing or incorrectly implemented and what result would support the hypothesis.

#### 5. Method and steps

Record the actions taken in order. Include the relevant tool and configuration details where they affect reproducibility.

#### 6. Evidence

Reference the corresponding files in the `Evidence/` directory. Identify which request, response, screenshot or other artefact supports each observation.

#### 7. Observed result

Describe what happened during the test. Separate direct observations from interpretation.

#### 8. Impact and limitations

Explain what the result demonstrates in the Academy lab. Record what cannot be concluded from the test and any limitations affecting the assessment.

#### 9. Alternative interpretations

Consider other explanations for the result, such as session state, caching, intermediary behaviour or application logic. State what further test could distinguish between them.

#### 10. Remediation

Recommend a mitigation that addresses the underlying control failure. Explain where the control should be enforced where that can be established.

#### 11. Retest

Record the retest method and outcome. Use one of the following statuses:

- **Confirmed:** the retest supports the original conclusion or verifies the proposed mitigation, as applicable.
- **Partially confirmed:** some, but not all, expected behaviour was demonstrated.
- **Rejected:** the evidence does not support the original hypothesis.
- **Inconclusive:** the available evidence is insufficient to reach a reliable conclusion.
- **Not retested:** no retest has been performed.

#### 12. Conclusion

Summarise the finding and its evidential limits.

## Recording standards

- Keep observations separate from assumptions.
- Include enough detail for another person to understand and reproduce the test within the authorised lab.
- Reference evidence files rather than duplicating large captures in the finding record.
- Sanitise all evidence before adding it to the repository.
- Do not include credentials, cookies, bearer tokens, personal data or other secrets.
- Do not claim a vulnerability, severity or remediation outcome that has not been supported by testing.

This document is a draft template. It will be refined as practical testing identifies what information is useful to capture consistently.
