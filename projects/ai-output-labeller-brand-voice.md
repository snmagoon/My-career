# AI Output Labelling and Brand Voice Document Pipeline

| | |
|---|---|
| **Company** | 10Pearls Pakistan |
| **Client** | Confidential enterprise client |
| **Period** | 2026 (recent) |
| **Role** | Staff Lead Developer |
| **Domain** | Generative AI, document automation, responsible AI |
| **Status** | Completed |

> **Resume-ready project entry**
>
> **AI Output Labelling and Brand Voice Pipeline | Confidential enterprise client**  
> Designed and delivered a .NET document-processing API that accepts AI-generated text and common business documents, applies configurable AI-generated disclaimers and watermarks, and returns polished PDF, DOCX, HTML, Markdown, or text output. Integrated an optional Brand Voice rewrite workflow using centrally managed style guidance and an Azure OpenAI model accessed through API Management. Built document-aware stamping paths to preserve the original body and layout where possible, plus an opt-in extract/rewrite/regenerate path when brand-voice rewriting is requested. Deployed the Output Labeler API to Azure App Service and verified it with a PowerShell end-to-end suite covering 32 scenarios.

**Core technologies:** C#, .NET 9, ASP.NET Core Web API, .NET 8 minimal API, Azure App Service, Azure API Management, Azure OpenAI, HTTP/JSON, OpenXML SDK, PdfSharp, PdfPig, QuestPDF, HtmlAgilityPack, Markdig, PowerShell 7, Swagger/OpenAPI.

---

## Project Overview

The pipeline addresses two related needs in AI-assisted document workflows: make AI-generated output visibly identifiable and give teams a reusable way to align generated drafts with the client's writing guidance. The Output Labeler exposes a multipart HTTP API for inline content or file uploads. Callers can keep the default stamping flow or opt into a brand-voice rewrite before the result is formatted.

The system is split into independently deployable services:

1. **Output Labeler API** — .NET 9 ASP.NET Core service that validates requests, handles document parsing and stamping, optionally orchestrates a rewrite, and returns a downloadable document.
2. **Brand Voice Guardrail API** — standalone .NET 8 minimal API that serves brand-voice guidance through a stable HTTP contract. Its storage abstraction supports filesystem and Azure Blob-backed content stores, with caching in the service layer.
3. **AI rewrite provider** — an Azure OpenAI chat-completions deployment reached through Azure API Management. The model and gateway configuration are externalized in application options rather than embedded in the rewrite logic.

```text
Caller
  |
  | multipart/form-data: content or file, output options, ApplyBrandVoice
  v
Output Labeler API (.NET 9)
  |-- default path: stamp document and preserve source body/layout
  |
  |-- optional rewrite path:
  |     GET brand-voice guidance
  |     POST rewrite request through API Management
  |     parse / rewrite / regenerate where required
  v
Downloadable PDF, DOCX, HTML, Markdown, or TXT
```

## Technical Contributions

### Document-processing API

- Designed a single `POST /api/format-output` endpoint supporting inline content and uploads of `.md`, `.txt`, `.html`/`.htm`, `.docx`, and `.pdf` files.
- Added request validation for missing content, unsupported extensions, invalid watermark combinations, and mismatched input/output formats.
- Supported five output formats: PDF, DOCX, HTML, Markdown, and plain text.
- Implemented configurable headers, footers, output filenames, and text or image watermarks, including opacity and position options.
- Added Swagger/OpenAPI documentation and deployed the API to Azure App Service.

### Structure-preserving stamping

- Implemented separate stamping paths for PDF, DOCX, and HTML so the normal labelling operation adds headers, footers, disclaimers, or watermarks without rebuilding the source document's main content.
- Used PDF overlays, OpenXML header/footer parts, and HTML DOM modification for their respective document formats.
- Kept content rewriting opt-in, so callers that only need an AI disclaimer or watermark can avoid an LLM call and preserve document layout.
- Enforced same-format output for uploaded files to avoid silently reflowing a document during a stamping-only request.

### Brand Voice and AI rewrite integration

- Added an `ApplyBrandVoice` request option to make brand-voice rewriting an explicit, caller-controlled step.
- Retrieved the current combined guidance from the independent Brand Voice Guardrail API rather than copying brand rules into the Output Labeler.
- Constructed a chat-completions request with a rewrite instruction and the retrieved guidance in the system message, and the source content in the user message.
- Configured gateway endpoint, deployment name, API version, route, timeout, and sampling temperature using options and named `HttpClient` instances.
- Used a fail-open policy for this optional feature: if guidance retrieval or the AI request fails or returns empty content, the original source content continues through the formatting pipeline. Failures are logged for diagnosis.
- Supported rewriting for inline content and uploaded document types. For PDF and DOCX, the rewrite path extracts text and regenerates the document; exact original layout, tables, images, and pagination are not guaranteed to survive this path. HTML and text/Markdown use format-specific processing.

### Reusable brand-guidance service

- Developed the Guardrail API as a separate service with a small HTTP contract, decoupling consumers from the physical location and storage implementation of the guidance.
- Applied a storage abstraction so brand-voice segments can be sourced from local files during development or Azure Blob Storage in a production-style configuration.
- Combined named guidance segments into a response for downstream consumers, with server-side caching to reduce repeated reads.
- Kept the Labeler-to-Guardrail integration HTTP-based so the services can evolve and deploy independently without a shared runtime library.

### Validation and operational quality

- Built a PowerShell 7 end-to-end demo suite that exercises document generation, input handling, stamping, watermarking, validation errors, and the optional brand-voice rewrite flow.
- Covered all five supported file types and all five output formats, including rewrite scenarios across inline content, Markdown, HTML, DOCX, and PDF.
- Validated expected HTTP status codes and response content types, and generated artifacts for manual review.
- The project documentation records a 32-scenario suite; run the current `Demo/Run-Demo.ps1` against the required services before quoting a current pass count.
- Verified the Output Labeler deployment on Azure App Service, including Swagger/OpenAPI and a successful document-generation smoke test. The Guardrail and rewrite dependencies must be separately configured and reachable for live brand-voice rewriting.

## Selected Design Decisions

| Decision | Rationale and trade-off |
|---|---|
| Separate Guardrail from the document API | Centralizes the source of brand guidance and makes it reusable by other consumers. Adds a network dependency that must be configured and monitored. |
| Make rewriting optional | Keeps deterministic stamping available without an LLM call, extra latency, or model cost. |
| Fail open when the rewrite dependency fails | Preserves document-generation availability. This means brand-voice alignment is not guaranteed during dependency outages and should not be treated as a hard compliance control. |
| Stamp original files rather than regenerate by default | Preserves source body and layout during standard labelling. Rewriting necessarily changes text and uses extraction/regeneration for some formats, with possible layout loss. |
| Keep model and gateway settings configurable | Allows deployment-specific configuration without coupling the content pipeline to one endpoint or model deployment. Secrets must be supplied through secure configuration, not source code. |

## Résumé Bullet Options

### Concise

- Built and deployed a .NET document-labelling API on Azure App Service, supporting five document inputs and five output formats, configurable AI disclaimers and watermarks, and document-aware stamping that preserves source layout where possible.
- Integrated an optional Brand Voice rewrite workflow using centrally served guidance and Azure OpenAI through API Management; implemented fail-open behavior, format-specific parsing/regeneration, and a 32-scenario PowerShell end-to-end suite.

### More technical

- Designed a modular .NET 9 ASP.NET Core document pipeline with multipart API validation, format-specific PDF/OpenXML/HTML stamping, and PDF/DOCX/HTML/Markdown/TXT generation; deployed and smoke-tested the service on Azure App Service.
- Connected the API to a separate .NET 8 brand-guidance service and Azure OpenAI via API Management, using named `HttpClient` clients, configurable options, explicit rewrite prompting, cancellation-aware async calls, structured logging, and original-content fallback on dependency failure.

## Interview Talking Points

- **Why two services?** Brand guidance belongs to a shared, centrally managed service; the document API consumes it over HTTP and does not own a duplicated copy of the rules.
- **How is layout handled?** Stamping-only operations modify or overlay document metadata/content without reflowing the body. Brand-voice rewriting changes body text, so PDF and DOCX must be parsed and regenerated, with layout fidelity as a known trade-off.
- **What happens if the AI service is unavailable?** Rewriting is optional and fail-open: the original content is returned through the normal labelling path, with warnings logged. The output still receives its configured AI-generated label.
- **How was it tested?** A PowerShell integration harness exercises real HTTP requests against running services, covering valid formats, validation failures, watermarks, and rewrite paths; generated files can be inspected manually.
- **What remains for production hardening?** Measure latency, throughput, and model cost; add dependency health/availability monitoring; ensure API credentials are held in a secret store; and decide whether any workflow requires fail-closed brand enforcement rather than the pilot's availability-first behavior.

## Accuracy and Confidentiality Notes

- The Output Labeler API has been deployed to Azure App Service. The brand-voice rewrite path depends on the independently hosted/configured Guardrail and AI gateway; do not imply that all dependencies share the same production deployment unless that has been verified for the role or project being described.
- Do not include internal hostnames, API keys, subscription details, source guidance files, or confidential examples in a public résumé or portfolio.
- No performance, cost-saving, latency, or productivity percentage is claimed here because the project materials do not record a measured benchmark for those outcomes.
