# BDO Personas

**Client:** BDO (professional services) · **Via:** 10Pearls Pakistan
**Period:** 2024 – present (CONFIRM start) · **My role:** CONFIRM (architecture and revamp analysis evident from docs)
**Domain:** Enterprise GenAI assistant · **Status:** Production; revamp in planning

## Overview
An internal AI chatbot for BDO staff using generative AI and RAG to help write client reports, analyse data, and find answers in the firm's archive. Users choose a "persona" tailored to a task, and can chat with uploaded files or with knowledge-base-grounded sources. Authentication is SSO through Entra ID.

## Architecture
- **Frontend:** React SPA with MSAL (Entra ID), Ant Design, and custom hooks.
- **API:** ASP.NET Core on .NET 8: persona chat, file chat, knowledge base retrieval, notifications, admin, and user profile/consent.
- **Background:** Azure Functions timers for conversation-retention cleanup and proactive deletion notifications.
- **Data:** Cosmos DB (threads, messages, personas, profiles), Redis distributed cache, Blob Storage.
- **AI services:** Azure API Management in front of OpenAI-style endpoints, Azure AI Document Intelligence, Azure AI Search, a BDO Search API.
- **Other:** Microsoft Graph, SendGrid, PDF export function, Key Vault references, Application Insights.
- **Delivery:** Bicep IaC and Azure DevOps YAML pipelines.

## Key flows
1. Standard persona chat, with streaming responses.
2. Chat with files: upload, chunk, summarise, retrieve.
3. Knowledge-base-grounded chat with cited sources.
4. Notification lifecycle and retention cleanup (impending deletion notices, scheduled purge).

## My contributions
- Authored the **Architecture Revamp Knowledge Pack**: system context, container diagrams, runtime flows, 10 use cases, security model, and a prioritised risk and modernisation plan.
- Proposed target architecture: modular monolith / vertical slices with transport, use-case, domain, and infrastructure-adapter layers; strangler-pattern migration with v2 contracts.
- Identified priorities: secret hygiene and Key Vault-only retrieval, tighter CORS, resilience policies (timeouts, retries, circuit breakers), streaming idempotency, schema versioning, Cosmos partitioning review, and more contract testing.
- Related work to confirm: React + Azure Functions AI assistant UI/backend, APIM-vs-direct-OpenAI benchmarking, Azure AD/MSAL auth.

## Tech stack
React, MSAL, Ant Design, C#/.NET 8, Azure Functions, Cosmos DB, Redis, Blob Storage, Azure AI Search, Document Intelligence, APIM, Key Vault, App Insights, Microsoft Graph, SendGrid, Bicep, Azure DevOps.

## Resume bullets
- Documented and analysed the architecture of an enterprise GenAI assistant (React, .NET 8, Cosmos DB, Redis, Azure AI Search, APIM) and produced a prioritised modernisation roadmap covering security hardening, resilience, and a strangler-pattern migration.
- Defined target architecture (modular monolith with vertical slices) and five revamp workstreams to simplify chat orchestration and standardise streaming contracts.
