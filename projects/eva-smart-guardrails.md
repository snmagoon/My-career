# EVA Smart Guardrails

| | |
|---|---|
| **Company** | 10Pearls Pakistan |
| **Client** | BDO (EVA AI platform) |
| **Period** | 2026 |
| **Role** | Staff Software Consultant |
| **Domain** | AI safety, governance, Power Platform integration |

## Overview
An enforcement layer between users and generative AI. It screens prompts before they reach the model, can screen responses before users see them, and records every decision for audit. The core is a FastAPI service that runs a chain of validators and returns structured per-category results. It can be called directly, through a Power Platform custom connector (Power Apps, Power Automate, Copilot Studio), or enforced organisation-wide through API Management policies on Azure OpenAI traffic.

## Architecture
- **API:** FastAPI on Azure App Service with Azure AD bearer-token validation. Endpoints: `POST /validate`, `GET /categories`, `GET /health`.
- **Chain of responsibility:** each category is a handler. The critical handlers (PII, secrets, prompt injection) halt the chain on failure and redact the prompt in the response, so the system fails safe.
- **Detection techniques:** multi-model LLM ensembles (GPT-4o-mini, o3-mini, GPT-4.1-mini) combined with GuardrailsAI, Presidio, spaCy, and TF-IDF similarity.
- **Enforcement layers:** Power Automate pre-input and post-response flows with AI Builder pre-classification, Copilot Studio topics, APIM inbound and outbound policies, DLP governance, and CoE Starter Kit templates.
- **Audit and observability:** SharePoint audit list, Power BI reporting, Application Insights structured logging.

### Guardrail categories
| Category | Critical | Technique |
|---|---|---|
| PII detection | Yes | Presidio, deny lists, GuardrailsAI |
| Secrets detection | Yes | LLM ensemble, GuardrailsAI |
| Prompt injection | Yes | LLM ensemble, GuardrailsAI |
| Toxic language | No | LLM ensemble, GuardrailsAI |
| Competitor check | No | String matching, GuardrailsAI |
| Regex match / max length | No | Caller-supplied patterns, length bounds |
| QA relevance, RAG evaluation | No | LLM-as-judge |
| Document similarity | No | spaCy, LLM, GuardrailsAI |
| URL and claim validation | No | Weighted risk score (green / yellow / red) |

## My contributions
- Wrote the **technical handover**: API contract, execution model, handler catalogue, and known gaps, including a response-shape mismatch with older flow documentation.
- Designed the **Microsoft 365 custom connector** architecture and produced four architecture diagrams (component flow, enforcement layers, APIM enforcement, Copilot Studio flow).
- Authored the **Smart Guardrails build guide** and **stakeholder presentation notes**.
- Planned delivery as an **Azure DevOps epic with six features**: connector, Power Automate flows, Copilot Studio enforcement, APIM policies, governance and DLP, testing.
- Exported and cleaned the OpenAPI specification for Power Platform compatibility.

## Tech stack
Python · FastAPI · Azure OpenAI · GuardrailsAI · Presidio · spaCy · Azure App Service · API Management · Azure AD · Application Insights · Power Automate · Copilot Studio · AI Builder · SharePoint · Power BI · Bicep

## Resume bullets
- Delivered an enterprise AI-guardrails service (FastAPI, Azure OpenAI, Presidio, GuardrailsAI) that screens prompts and responses for PII, secrets, prompt injection, and toxicity, with fail-safe handling of critical categories.
- Designed organisation-wide enforcement across Power Platform, Copilot Studio, and API Management policies, with a SharePoint audit trail and Power BI reporting.
- Produced the technical handover, connector design, build guide, and a six-feature Azure DevOps delivery plan for rollout as an organisational default.
