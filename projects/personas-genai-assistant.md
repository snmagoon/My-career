# Personas: Enterprise GenAI Assistant

| | |
|---|---|
| **Company** | 10Pearls Pakistan |
| **Client** | Confidential enterprise client |
| **Period** | 2024 – present |
| **Role** | Staff Software Consultant (developer on the initial build and the revamp, as part of the team) |
| **Domain** | Enterprise generative AI assistant |
| **Status** | In production; revamp delivered |

## Overview
An internal AI assistant for a global professional services firm's staff that combines generative AI and Retrieval-Augmented Generation (RAG) to help write client reports, analyse data, and find answers in the firm's document archive. Users pick a *persona* tailored to a task, and can chat with uploaded files or with knowledge-base-grounded sources. Access is through single sign-on with Microsoft Entra ID.

## Architecture
| Layer | Technology |
|---|---|
| Frontend | React single-page app, MSAL (Entra ID), Ant Design |
| API | ASP.NET Core on .NET 8: persona chat, file chat, knowledge retrieval, notifications, admin, user profile and consent |
| Background jobs | Azure Functions timers for conversation-retention cleanup and deletion notices |
| Data | Cosmos DB (threads, messages, personas, profiles), Redis distributed cache, Blob Storage |
| AI and search | Azure API Management in front of the LLM endpoints, Azure AI Document Intelligence, Azure AI Search, enterprise search API |
| Integrations | Microsoft Graph, SendGrid, PDF export function |
| Platform | Key Vault references, Application Insights, Bicep IaC, Azure DevOps YAML pipelines |

**Key runtime flows:** streaming persona chat · chat with files (upload, chunk, summarise, retrieve) · knowledge-base-grounded answers with sources · notification lifecycle and scheduled retention cleanup.

## My contributions
I was part of the team that built the original platform, and I contributed to the revamp from architecture through implementation.

- Hands-on development on both the initial build and the revamped platform, working with the wider team.
- Produced the **Architecture Revamp Knowledge Pack**: system context and container diagrams, runtime flows, ten end-to-end use cases, security and identity model, and operations review.
- Assessed the platform and set revamp priorities: secret hygiene with Key Vault-only retrieval, tighter CORS, consistent resilience policies (timeouts, retries, circuit breakers), idempotent message saves under stream interruption, versioned schemas, and Cosmos partitioning review.
- Recommended a target architecture of a modular monolith with vertical slices (transport, use-case, domain, and infrastructure-adapter layers) and a strangler-pattern migration that keeps existing API contracts and introduces v2 contracts only for breaking changes.
- Defined five delivery workstreams: security hardening, chat-orchestration refactor with test harnesses, data and event contract normalisation, reliability and observability, and performance and cost.

## Tech stack
React · MSAL · Ant Design · C# / .NET 8 · Azure Functions · Cosmos DB · Redis · Blob Storage · Azure AI Search · Document Intelligence · API Management · Key Vault · Application Insights · Microsoft Graph · SendGrid · Bicep · Azure DevOps

## Resume bullets
- Built an enterprise GenAI assistant (React, .NET 8, Cosmos DB, Redis, Azure AI Search, API Management) as part of the delivery team, then contributed to its revamp, including a prioritised modernisation roadmap covering security hardening, resilience, and a strangler-pattern migration.
- Defined a target modular-monolith architecture with vertical slices and five delivery workstreams to simplify chat orchestration and standardise streaming contracts.
