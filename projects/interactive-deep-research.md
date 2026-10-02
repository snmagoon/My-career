# Interactive Deep Research

| | |
|---|---|
| **Company** | 10Pearls Pakistan |
| **Client** | Confidential enterprise client |
| **Period** | 2026 |
| **Role** | Staff Software Consultant |
| **Domain** | Agentic AI, research automation |

## Overview
A full-stack deep-research platform. A user enters a research question; the system asks clarifying questions and builds an expanded prompt, then runs OpenAI's o3-deep-research model with web search and an optional code interpreter. Progress is tracked live, and the result is a cited report that can be exported to Markdown or text. The prototype was then hardened into an enterprise service with automated quality assessment, an asynchronous task queue, and API Management governance.

## Architecture
React 18 SPA → ASP.NET Core Minimal API (.NET 9) → Semantic Kernel → API Management → OpenAI / Azure OpenAI, with Azure Table Storage for task state and Service Bus for events.

## Key capabilities
| Capability | Detail |
|---|---|
| Prompt expansion | 3–5 clarifying questions and an optimised prompt (GPT-4.1) |
| Deep research | Web search, code interpreter, configurable tool-call limit (default 50), live status polling |
| Quality auto-retry | Each result is scored; below 92% or misaligned triggers up to 3 retries, with the prompt enriched by QA feedback and no feedback stacking |
| Async task queue | `POST /api/kickoff` returns a task ID immediately; Azure Table Storage holds state; background workers run up to 8 tasks concurrently; Service Bus publishes events; clients poll `/api/status/{id}` |
| API Management governance | A `KernelFactory` routes every Semantic Kernel call through APIM with Azure AD client-credential auth, closing a gap where some calls bypassed APIM |
| Production hardening | Prompts and thresholds externalised to hot-reloadable JSON config, strongly typed settings, dependency injection |
| Export | Markdown and text with citations |

## My contributions
- Designed and built the .NET 9 / Semantic Kernel backend and the React frontend.
- Analysed the existing Python (FastAPI) company-research service and ported its async task-queue pattern to .NET.
- Implemented the QA scoring and auto-retry loop, APIM integration, and configuration externalisation.
- Audited APIM coverage and fixed the AI calls that bypassed it.
- Wrote the developer documentation, quick-start, security checklist, and PowerShell test scripts.

## Tech stack
C# · .NET 9 · ASP.NET Core Minimal API · Semantic Kernel · OpenAI o3-deep-research · GPT-4.1 · React 18 · Axios · React-Markdown · Azure Table Storage · Service Bus · API Management · Azure AD · Swagger

## Resume bullets
- Built an interactive deep-research platform on .NET 9, Semantic Kernel, and OpenAI o3-deep-research with prompt expansion, live progress tracking, and cited report export.
- Implemented automated quality scoring with a retry loop (92% threshold, up to 3 retries) that feeds assessment feedback into the prompt to raise output quality.
- Re-architected the service to an asynchronous task-queue pattern (Azure Table Storage, background workers, Service Bus events, 8-way concurrency control) and routed all LLM traffic through API Management.
