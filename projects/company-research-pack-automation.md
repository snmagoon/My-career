# Company Research Pack: UiPath and Copilot Studio Replication

| | |
|---|---|
| **Company** | 10Pearls Pakistan |
| **Client** | Confidential enterprise client |
| **Period** | 2025 – 2026 (past year) |
| **Role** | Staff Lead Developer |
| **Domain** | Intelligent automation, RPA, agentic AI, due diligence |

## Overview
The Company Research Pack is an AI-driven due-diligence system, originally built in Python with FastAPI and CrewAI. Given a UK company's name, number and website, it runs a 15+ step research pipeline and produces a branded PDF report. I rebuilt the same end-to-end flow on two low-code automation platforms to prove it could be delivered without custom Python hosting: **UiPath Automation Cloud** and **Microsoft Copilot Studio**. For each platform I wrote a complete step-by-step implementation guide.

## What the pipeline does
1. Validates inputs, then retrieves officers, filings, company record, and persons with significant control from the Companies House API.
2. Scrapes the company website and extracts financial data from filed accounts.
3. Researches directors, competitors, news and media, HMRC rulings, and industry trends using web-search agents.
4. Runs multi-method sentiment analysis, a Russia/China country-ties risk analysis, and a UK sanctions check.
5. Generates a branded PDF report and uploads it to the enterprise AI platform.
6. Publishes start, success and failure events for every step to Azure Service Bus.

## UiPath implementation
| Python original | UiPath building block |
|---|---|
| Sequential CrewAI flow | **Maestro agentic process** (BPMN 2.0) as the master orchestrator, with validation gateways and optional parallel branches |
| API integration steps | **Studio Web RPA workflows** using HTTP Request, Assign, If, For Each, Switch and Do While activities |
| CrewAI crews (media, directors, competitors, HMRC, industry trends) | **Autonomous agents** with prompts, tools, context grounding, and evaluation sets with health scoring |
| Secrets and configuration | **Orchestrator assets** and credentials; LLM access governed by the **AI Trust Layer** |
| Flow state | Process-level variables passed between Maestro tasks |
| Event publishing | Service Bus events posted through the middleware for every step (start, success, failure, progress) |

The guide covers 17 build steps plus testing, debugging, publishing and deployment, and went through several revisions, including a full review against the Python source that corrected seven discrepancies.

## Copilot Studio implementation
| Python original | Copilot Studio building block |
|---|---|
| CrewAI flow | Agent plus a **workflow** triggered with typed input parameters, run on the GitHub Copilot harness |
| REST API steps | **Custom connectors**: Companies House, internal graph API, financial-data extraction, EVA platform upload |
| CrewAI crews | **Generative action** nodes with web-search tools |
| Evaluate-and-retry loops | **Do Until** loops with an evaluation prompt (up to 3 retries) |
| Flow state | Execution-scoped workflow variables with typed records and tables |
| Sanctions matching | Deterministic **Azure Function** exposed as a connector, because LLMs are unreliable for exact compliance matching |
| PDF branding | Word template plus PDF conversion, or an **Azure Function** that reuses the existing Python report code |

The Copilot Studio guide includes an honest gap analysis covering Service Bus event granularity, concurrency control, RPA-based financial extraction via Power Automate Desktop, PDF branding parity, and cost of the usage-based model.

## My contributions
- Analysed the Python source and mapped every step to a UiPath and a Copilot Studio equivalent.
- Built the end-to-end workflow design on both platforms: orchestration, API integration, AI agents, state handling, retries, and error paths.
- Designed the Service Bus event contract for each step (start, success, failure, progress), written up as one specification per step.
- Wrote two detailed implementation guides for the team, including prerequisites, secrets handling, testing and deployment.
- Identified and documented platform limitations with recommended workarounds.

## Tech stack
UiPath Automation Cloud (Maestro, Studio Web, Agents, Orchestrator, AI Trust Layer, Context Grounding, Integration Service) · Microsoft Copilot Studio · Power Automate · Power Platform custom connectors · Azure Functions · Azure Service Bus · Azure Key Vault · Azure OpenAI · Companies House API · Python / CrewAI (source system)

## Resume bullets
- Rebuilt a Python/CrewAI multi-agent due-diligence pipeline (15+ steps) on UiPath Maestro, Studio Web RPA workflows, and autonomous agents, with Orchestrator-managed secrets and AI Trust Layer governance.
- Replicated the same flow in Microsoft Copilot Studio using workflows, custom connectors, generative actions, and retry loops, with Azure Functions for deterministic sanctions matching and report generation.
- Authored implementation guides and per-step event specifications that let other teams build and operate the automation on either platform.
