# V5. Secure LLM Integration

## Control Objective
Establish controls that enable safe interactions and operations between application components and LLMs. 

| # | Requirement | L1 | L2 | L3 |
| - | ---------- | -- | -- | -- |
| 5.1.1 | Ensure that prompts to LLMs are issued from a trusted server-side component. | ✓ | ✓ | ✓ |
| 5.1.2 | Ensure that prompts to LLMs are constructed server-side, rather than accepting the complete prompt directly from the client. | ✓ | ✓ | ✓ |
| 5.1.3 | Ensure that user prompts are properly validated to exclude special characters, keywords, separators, etc. - NEW: https://arxiv.org/pdf/2306.05499, https://arxiv.org/pdf/2311.16119, https://arxiv.org/pdf/2402.06363 | ✓ | ✓ | ✓ |
| 5.1.4 | Ensure that user prompts are protected against semantic-based prompt injections, such as completion attacks, ignore attacks and important task attacks. - NEW: Ignore attacks - https://arxiv.org/pdf/2211.09527, https://arxiv.org/pdf/2306.05499, https://arxiv.org/pdf/2311.16119, https://arxiv.org/pdf/2406.13352 | ✓ | ✓ | ✓ |
| 5.1.5 | Ensure that user-supplied URLs undergo safety checks to prevent indirect prompt injection. - NEW: https://arxiv.org/pdf/2302.12173, https://arxiv.org/pdf/2402.18649 | ✓ | ✓ | ✓ |
| 5.1.6 | Ensure that proper protections are in place to prevent meta-prompt leakage, e.g. canary tokens, guardrails, input and output filtering, etc. - EXPANDED from 5.7 in original: https://github.com/NVIDIA/NeMo-Guardrails |   | ✓ | ✓ |
| 5.1.7 | Ensure that appropriate LLM guards are used to scan prompts and compilations to help detect potential prompt injection attacks. |      | ✓ | ✓ |
| 5.1.8 | Ensure that prompts are scanned for suspicious, harmful or banned sequences of words. - NEW: https://arxiv.org/pdf/2306.05499, https://arxiv.org/pdf/2405.20234v1, https://arxiv.org/pdf/2402.18649 |    | ✓ | ✓ |

## V5.2 Context Security

This section deals primarily with defenses against context injection attacks. In context injection attacks, an entire context is fed to the LLM, rather than a single prompt. Reference for all requirements: https://arxiv.org/pdf/2405.20234v1

| # | Requirement | L1 | L2 | L3 |
| - | ---------- | -- | -- | -- |
| 5.2.1 | Ensure that the application protects user context against word anonymization (replacing harmful/banned words with other words). - NEW | ✓ | ✓ | ✓ |
| 5.2.2 | Ensure that the application protects user context from acceptance elicitation attacks, i.e. the LLM should not accept users' claims about previously received messages from the LLM. - NEW | ✓ | ✓ | ✓ |
| 5.2.3 | Ensure that chat histories are stored server-side and their integrity is regularly verified. - NEW |    |    | ✓ |
| 5.2.4 | Check that inputs from various sources, e.g. user, system, administrator, are segregated from each other and do not influence one another. - NEW |    |    | ✓ |

## V5.3 LLM Access Control

This section deals with proper access controls to and from the LLM module in an application.

| # | Requirement | L1 | L2 | L3 |
| - | ---------- | -- | -- | -- |
| 5.3.1 | Ensure that the output of LLM completions is considered to be untrusted by any subsequent system. For example, if using the LLM response within a SQL query, the query should not be constructed by concatenating parts of the LLM response but should follow section V5.3.4 of the OWASP ASVS and use parmeterized queries. | ✓ | ✓ | ✓ |
| 5.3.2 | Ensure that all prompts are considered to be untrusted and subjected to any deployed security controls. Reflecting stored data, data from third-party APIs, or the response from previous prompt compilations may lead to indirect prompt injections and must be subjected to the same controls as prompts containing direct user input. |      | ✓ | ✓ |
| 5.3.3 | Ensure that the principle of least privilege is respected with respect to the LLM's internal APIs, functions and network access. - NEW: https://portswigger.net/web-security/llm-attacks#exploiting-llm-apis-functions-and-plugins  |      | ✓ | ✓ |
| 5.3.4 | Ensure any functionality that allows anonymous users to preview features is properly restricted to allow only the necessary features. |      | ✓ | ✓ |
| 5.3.5 | Ensure that credentials for LLM providers are securely handled according to section V2.10 “Service Authentication” of the OWASP ASVS. |      | ✓ | ✓ |
| 5.3.6 | Consider the use of redundant LLM accounts and providers to avoid single points of failure and ensure application availability. |      |      | ✓ |

## V5.4 LLM Output Security

This section deals with protecting and monitoring the output of the LLM module.

| # | Requirement | L1 | L2 | L3 |
| - | ---------- | -- | -- | -- |
| 5.4.1 | Ensure that the output format and properties of the data returned from the LLM match the expected structure and properties. Specifically, when a response is expected in JSON, the result should not only be in valid JSON format, but also undergo schema validation to ensure it contains all the expected JSON fields and does not include any unnecessary or extraneous properties. | ✓ | ✓ | ✓ |
| 5.4.2 | Ensure that the application properly suppresses any exceptions and error messages when interacting with the LLM. LLM errors may inadvertently leak the prompt and should not be visible to the client. | ✓ | ✓ | ✓ |
| 5.4.3 | Ensure that LLM output is scanned for suspicious, harmful or banned content. - NEW: https://arxiv.org/pdf/2405.20234v1, https://arxiv.org/pdf/2402.18649 |    | ✓ | ✓ |
| 5.4.4 | Ensure that the output language of the LLM response matches the expected language. |      | ✓ | ✓ |
| 5.4.5 | Perform length checks on LLM completions to verify that the response length is within an expected range. For example, a response that is significantly longer than the normal output length might indicate the completion is including additional, unexpected data. |      |      | ✓ |
| 5.4.6 | Check the entropy of LLM responses to detect encoded data which aims to circumvent additional checks, such as bypassing canary tokens. |      |      | ✓ |

## V5.5 Cost Control

This section deals with ensuring that LLM costs stay within appropriate limits.

| # | Requirement | L1 | L2 | L3 |
| - | ---------- | -- | -- | -- |
| 5.5.1 | Ensure that cost alerts are active within LLM provider configurations to be alerted when costs exceed expectations. | ✓ | ✓ | ✓ |
| 5.5.2 | Ensure that systems that result in LLM calls have appropriate API rate limiting to avoid excessive calls to LLMs, which may result in unexpected and excessive LLM costs. |      | ✓ | ✓ |
| 5.5.3 | Ensure that the LLM is not able to perform computationally heavy tasks. - NEW: https://arxiv.org/pdf/2311.11415 |      | ✓ | ✓ |