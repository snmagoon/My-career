# Company Research Pack: UiPath and Copilot Studio Automation

| | |
|---|---|
| **Company** | 10Pearls Pakistan |
| **Client** | Confidential enterprise client (EVA AI platform) |
| **Period** | 2026 |
| **Role** | Staff Lead Developer |
| **Domain** | Intelligent automation, AI-driven due diligence |
| **Type** | Platform replication / proof of concept |

## Overview
The Company Research Pack is an AI-driven due-diligence flow, originally a Python / FastAPI / CrewAI multi-agent application with 18 steps. It researches a UK company and produces a branded PDF report. I replicated the whole flow twice on low-code and RPA platforms, **UiPath** and **Microsoft Copilot Studio**, to evaluate how far each platform can match a custom-coded agentic system.

**What the flow does:** takes a company name, number and website; pulls officers, filings, company record and persons with significant control from the Companies House API; extracts financial data from filings; researches directors, competitors, news, industry trends and HMRC rulings with web-search agents; analyses sentiment and Russia/China country ties; checks the UK sanctions list; generates a consolidated PDF report; and uploads the result to the EVA platform with progress events published to Azure Service Bus.

## UiPath implementation
- Modelled the process as a **Maestro agentic process** (BPMN 2.0), with Service Tasks for each step and gateways for branching and parallel paths.
- Built **RPA workflows in Studio Web** for the deterministic steps: Companies House API integration, website scraping, financial-data extraction, sentiment analysis, country-ties analysis, UK sanctions matching, and PDF report generation, using HTTP Request, For Each, Do While, Switch, If and Download File activities.
- Built **autonomous AI agents** for the reasoning steps: media and news research, director research, competitor research, HMRC rulings, and industry trends, each with prompts, tools, and evaluation sets to measure quality.
- Used **Orchestrator assets** for secrets and configuration, the **AI Trust Layer** for governed LLM access, and **Context Grounding** where retrieval was needed.
- Specified a per-step **Service Bus event contract** (start, success, failure, and progress messages) so the platform could track each step.
- Wrote a complete **step-by-step replication guide** (version 2.3, April 2026), cross-checked against the Python source to fix discrepancies in crew tasks, country-ties logic, sanctions matching, and sentiment scoring.

## Copilot Studio implementation
- Created a **Company Research agent** with a main orchestration **workflow** that mirrors steps 00–17, using typed input parameters and execution-scoped variables in place of CrewAI's flow state.
- Built **custom connectors** for the Companies House API, an internal organisation-graph API, the financial-data extraction service, and the EVA platform's file-upload and completion endpoints, with secrets in **Key Vault-backed environment variables**.
- Replaced the five CrewAI crews with **generative actions** plus web-search tools, and reproduced the evaluate-and-retry pattern with **Do Until** loops (up to 3 retries).
- Used **parallel branches** for independent Companies House calls, and error branches that publish failure events.
- Chose deterministic matching (an Azure Function behind a connector) for sanctions screening, because LLMs are unreliable for legal name matching.
- Compared two report options: a Word template with PDF conversion, or an Azure Function that reuses the existing Python report code.
- Produced an honest **gap analysis**: Service Bus event granularity, concurrency control, RPA-based financial extraction, PDF branding parity, agent iteration limits, and cost model.

## Outcomes
- A working blueprint for moving a code-first multi-agent system onto UiPath and onto Copilot Studio, with a mapping of every step to platform building blocks.
- Clear guidance on where each platform fits and where custom code (Azure Functions) is still needed.

## Tech stack
UiPath (Maestro, Studio Web, Orchestrator, autonomous agents, AI Trust Layer, Context Grounding) · Microsoft Copilot Studio (agents, workflows, agent flows) · Power Automate · Power Platform custom connectors · Azure Service Bus · Azure Functions · Azure Key Vault · Azure OpenAI · Companies House API · Python / FastAPI / CrewAI (source system)

## Resume bullets
- Replicated an 18-step AI due-diligence pipeline (Python / CrewAI) on UiPath using Maestro BPMN orchestration, Studio Web RPA workflows, and autonomous LLM agents with evaluation sets.
- Rebuilt the same pipeline in Copilot Studio with custom connectors, generative actions, retry loops, and Key Vault-backed secrets, and documented platform gaps and workarounds for architecture decision-making.
- Authored step-by-step implementation guides and a Service Bus event specification used to evaluate low-code and RPA platforms against a custom-coded agentic system.
