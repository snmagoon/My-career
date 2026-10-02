# EVA Smart Guardrails

**Client:** the firm's Innovation & Digital Office / EVA platform (CONFIRM naming) · **Via:** 10Pearls Pakistan
**Period:** 2026 (docs dated 02/2026 – 10/2026) · **My role:** CONFIRM (author of design, handover, and work-item docs)
**Domain:** AI safety, governance, Power Platform

## Overview
An enforcement layer between users and generative AI: it screens prompts before they reach the model, can screen responses before users see them, and logs every decision for audit. The core is a FastAPI service (`EVA.Helpers.GuardrailsAPI`) that runs a chain of validators and returns structured per-category results. It can be called directly, through a Power Platform custom connector (Power Apps, Power Automate, Copilot Studio), or enforced org-wide through APIM policies on Azure OpenAI traffic.

## Architecture
- **API:** FastAPI on Azure App Service; Azure AD bearer-token auth (JWKS validation); `POST /validate`, `GET /categories`, `GET /health`.
- **Chain of responsibility:** each category is a handler; critical handlers (PII, secrets, prompt injection) halt the chain on failure and redact the prompt in the response (fail-safe).
- **Handlers:** toxic language, PII detection (Presidio + deny lists), prompt injection, secrets detection, competitor check, regex match, max length, QA relevance, RAG evaluation, document similarity, plus a separate URL/claim validation pipeline with a weighted risk score (green/yellow/red).
- **Detection techniques:** multi-model LLM ensembles (GPT-4o-mini, o3-mini, GPT-4.1-mini) combined with GuardrailsAI, spaCy, and TF-IDF.
- **Integration layers:** Power Automate pre-input and post-response flows (with AI Builder pre-classification), Copilot Studio topics, SharePoint audit list, Power BI reporting, APIM inbound and outbound policies, DLP governance and CoE Starter Kit templates.
- **Observability:** Application Insights structured logging.

## My contributions
- Wrote the **technical handover** (API contract, execution model, handler catalogue, known gaps such as the response-shape mismatch with older flow docs).
- Designed the **M365 custom connector** architecture and the four architecture diagrams (component flow, enforcement layers, APIM enforcement, Copilot Studio flow).
- Authored the **Smart Guardrails build guide** and **presentation notes** for stakeholder demos.
- Broke the programme into an **Azure DevOps epic with six features**, user stories and detailed tasks: connector, Power Automate flows, Copilot Studio enforcement, APIM policies, governance/DLP, testing.
- Exported and cleaned the OpenAPI spec for Power Platform compatibility.

## Tech stack
Python, FastAPI, Azure OpenAI, GuardrailsAI, Presidio, spaCy, Azure App Service, APIM, Azure AD, Application Insights, Power Platform (custom connector, Power Automate, Copilot Studio, AI Builder), SharePoint, Power BI, Mermaid, Bicep.

## Resume bullets
- Delivered an enterprise AI-guardrails service (FastAPI, Azure OpenAI, Presidio, GuardrailsAI) that screens prompts and responses for PII, secrets, prompt injection, and toxicity, with fail-safe handling for critical categories.
- Designed organisation-wide enforcement across Power Platform, Copilot Studio, and Azure API Management policies, with a SharePoint audit trail and Power BI reporting.
- Produced technical handover, connector design, build guide, and a six-feature Azure DevOps delivery plan for rollout as an organisational default.
