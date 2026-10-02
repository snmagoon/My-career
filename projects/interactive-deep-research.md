# Interactive Deep Research

**Company:** 10Pearls Pakistan (client context CONFIRM) · **Period:** about 01/2026 – 2026
**My role:** CONFIRM (appears to be lead engineer/architect) · **Domain:** Agentic AI, research automation

## Overview
A full-stack deep-research application. A user enters a research question; the system generates clarifying questions and an expanded prompt, then runs OpenAI's o3-deep-research model with web search and optional code interpreter, tracks progress in real time, and returns a cited report that can be exported to Markdown or text. It later evolved into an enterprise-grade service: an "enhanced" research pipeline with automated quality assessment, async task processing, and APIM-governed AI calls.

## Key features
- **Prompt expansion** with 3–5 clarifying questions (GPT-4.1).
- **Deep research engine** with web search, code interpreter, configurable max tool calls (default 50), and real-time status polling.
- **QA auto-retry:** quality assessment scores each result; if the score is below 92% or output is misaligned, the system retries up to 3 times with prompts enriched by QA feedback (without stacking old feedback).
- **Async task queue** (ported from a Python/FastAPI company-research app): `POST /api/kickoff` returns a task ID immediately; Azure Table Storage holds state; a background process manager runs up to 8 concurrent tasks; Service Bus events notify external systems; clients poll `/api/status/{id}`.
- **APIM governance:** a `KernelFactory` routes all Semantic Kernel calls through Azure API Management with Azure AD client-credential auth, closing a gap where some calls bypassed APIM.
- **Production hardening:** prompts and thresholds externalised to JSON config (hot-reloadable), strongly typed settings, dependency injection throughout.
- **Export:** Markdown and text, with citations.

## Architecture
React 18 SPA → ASP.NET Core Minimal API (.NET 9) → Semantic Kernel → APIM → OpenAI / Azure OpenAI; Azure Table Storage for task state; Service Bus for events.

## My contributions
- Designed and built the backend (.NET 9, Semantic Kernel) and React frontend.
- Reviewed the Python reference implementation and ported its async task pattern to .NET.
- Implemented the QA scoring and auto-retry loop, APIM integration, and config externalisation.
- Wrote developer docs, quick-start, security checklist, and PowerShell test scripts.

## Tech stack
C#, .NET 9, ASP.NET Core Minimal API, Semantic Kernel, OpenAI o3-deep-research, GPT-4.1, React 18, Axios, React-Markdown, Azure Table Storage, Service Bus, APIM, Azure AD (ClientSecretCredential), Swagger.

## Resume bullets
- Built an interactive deep-research platform on .NET 9, Semantic Kernel, and OpenAI o3-deep-research with prompt expansion, live progress tracking, and cited report export.
- Implemented automated QA scoring with a retry loop (threshold 92%, up to 3 retries) that feeds assessment feedback back into the prompt to raise output quality.
- Re-architected the service to an async task-queue pattern (Azure Table Storage, background workers, Service Bus events, 8-way concurrency control) and routed all LLM traffic through API Management.
