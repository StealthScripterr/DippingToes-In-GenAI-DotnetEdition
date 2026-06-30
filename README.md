# DippingToes-In-GenAI-DotnetEdition
## .NET + Azure AI Roadmap (12 Weeks)
This repo documents a 12‑week, industry‑oriented learning and project roadmap for building production‑grade AI applications using .NET, Azure OpenAI, and Azure AI Search.  The goal is to move from LLM fundamentals to a fully deployed, enterprise‑ready capstone, following patterns aligned with Microsoft’s .NET RAG and Azure OpenAI guidance.
Below is a detailed `README.md` you can drop into your repo and adapt. It assumes a 12‑week, .NET + Azure focused learning and project roadmap building toward an enterprise-grade RAG/agentic app.
***

This repository documents a 12‑week, industry‑oriented learning and project roadmap for building production‑grade AI applications using **.NET**, **Azure OpenAI**, and **Azure AI Search**.

The goal is to move from LLM fundamentals to a fully deployed, enterprise‑ready capstone (e.g., NotebookLM‑style document assistant or AI pitch deck generator), following patterns aligned with Microsoft’s official .NET RAG and Azure OpenAI guidance. [learn.microsoft](https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-dotnet)

***

## Objectives

By the end of this roadmap, you will be able to:

- Explain core LLM concepts (tokens, embeddings, context windows, temperature/top‑p, attention) and tie them to real system behavior.
- Build streaming chat applications in ASP.NET Core or Blazor using Azure OpenAI’s .NET client library. [learn.microsoft](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/ai.openai-readme?view=azure-dotnet)
- Design and implement a full **RAG** (Retrieval‑Augmented Generation) pipeline on Azure, using Azure AI Search for hybrid vector + keyword retrieval with semantic ranking. [learn.microsoft](https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-dotnet)
- Implement non‑blocking, production‑grade ingestion pipelines using Azure Service Bus and .NET Worker Services.
- Use **Semantic Kernel** to orchestrate tools, retrieval, and agentic workflows in C#. [learn.microsoft](https://learn.microsoft.com/en-us/semantic-kernel/frameworks/agent/agent-types/azure-ai-agent)
- Deploy and harden applications on Azure App Service or Container Apps with managed identity, observability, and cost controls. [learn.microsoft](https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-dotnet)

***

## Tech Stack

- **Language & Runtime**
  - .NET 8/9 (C#)
- **Backend**
  - ASP.NET Core (Minimal APIs or MVC)
  - Optional: Blazor Server/WASM for UI
- **AI / LLM**
  - Azure OpenAI Service (chat/completions, embeddings) [learn.microsoft](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/ai.openai-readme?view=azure-dotnet)
  - Optional: Local models via Ollama + .NET client
- **Retrieval & Data**
  - Azure AI Search (hybrid search, semantic ranker, vector search) [learn.microsoft](https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-dotnet)
  - Azure Blob Storage (raw documents)
  - Azure SQL / Cosmos DB (structured data, logging, metadata)
- **Orchestration / Agents**
  - Microsoft Semantic Kernel (.NET)
- **Async Processing**
  - Azure Service Bus
  - .NET Worker Service (BackgroundService)
- **Security & Config**
  - Azure Entra ID / Managed Identity
  - Azure Key Vault
- **Hosting & Ops**
  - Azure App Service or Container Apps
  - Application Insights, Azure Monitor

***

## Project Structure (Suggested)

You can organize this repo by week and by project:

```text
/Week01-03_Foundations
  /Notes
  /PlaygroundConsole

/Week04_StreamingChat
  /StreamingChat.Api
  /StreamingChat.Web

/Week06-08_RAG
  /Rag.Api
  /Rag.Web
  /Rag.Ingestion.Worker

/Week09-10_AgenticRag
  /AgenticRag.Api
  /AgenticRag.Orchestration

/Week11-12_Capstone
  /Capstone.App
  /infra (Bicep / ARM / azd)
/docs
  architecture.md
  decisions-log.md
```

This README describes the **overall roadmap**. Each project folder should have its own smaller README explaining local setup and design decisions.

***

## Week‑by‑Week Roadmap

### Week 1 – LLM Foundations

**Focus**

- Understand what happens when you send a message to an LLM:
  - Tokenization, embeddings, attention, context window limits.
  - Temperature, top‑p, and how they affect randomness.
- Map these to practical system limitations (latency, cost, truncation behavior).

**Deliverables**

- A short markdown document (`/Week01-03_Foundations/Notes/llm_fundamentals.md`) explaining:
  - How a prompt is converted into tokens and processed.
  - What context windows are and what happens when you exceed them.
  - How you would choose temperature and top‑p for different use cases.

**Industry bar**

- Explanations are tied to behavior you’ll see in your app (e.g., “If we set temperature high, answers vary more, which affects caching and testing strategies.”).

***

### Week 2 – .NET + Azure OpenAI Setup

**Focus**

- Setup Azure OpenAI in Azure, and configure .NET to call it using the **Azure OpenAI .NET client** and managed identity or Entra ID. [learn.microsoft](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/ai.openai-readme?view=azure-dotnet)
- Learn configuration best practices: `IOptions<T>`, `DefaultAzureCredential`, environment‑specific settings.

**Tasks**

- Create `/Week01-03_Foundations/PlaygroundConsole`:
  - A .NET console app that:
    - Uses `Azure.AI.OpenAI` to call a chat or completion model.
    - Uses `DefaultAzureCredential` so no API keys are hard‑coded. [learn.microsoft](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/ai.openai-readme?view=azure-dotnet)
    - Reads model/deployment names from configuration.

**Industry bar**

- No secrets in the codebase.
- Clear separation of concerns (client factory, configuration, and business logic separated).

***

### Week 3 – Prompt Engineering in C#

**Focus**

- Implement:
  - Zero‑shot, few‑shot, and role prompting.
  - Chain‑of‑thought style prompts.
  - JSON / structured output prompts with validation.

**Tasks**

- Extend the Playground app with:
  - A small “prompt lab” class with methods like:
    - `RunZeroShotPrompt`
    - `RunFewShotPrompt`
    - `RunJsonOutputPrompt`
  - A simple evaluation harness:
    - Fixed test inputs and assertions or checks for required fields.

**Industry bar**

- Prompts and templates versioned and organized (e.g., in a `Prompts/` folder).
- Structured outputs validated before use (e.g., try‑parse JSON, fallback logic).

***

### Week 4 – Streaming Chat Application

**Focus**

- Build a ChatGPT‑style streaming chat clone using ASP.NET Core + Azure OpenAI streaming. [learn.microsoft](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/ai.openai-readme?view=azure-dotnet)
- Implement markdown rendering and conversation history.

**Tasks**

- Create `/Week04_StreamingChat`:
  - Backend (`StreamingChat.Api`):
    - Minimal API or controller that:
      - Accepts user messages and conversation history.
      - Streams responses using Azure OpenAI’s streaming API (e.g., `IAsyncEnumerable` of updates). [learn.microsoft](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/ai.openai-readme?view=azure-dotnet)
  - Frontend (`StreamingChat.Web`):
    - Blazor or React page that:
      - Shows streaming tokens/segments as they arrive.
      - Renders markdown and code blocks.
    - Persists conversation history in memory or a simple DB.

**Industry bar**

- Graceful handling of:
  - Cancellation (user stopping generation).
  - Partial responses (timeout, error).
  - Token usage logging for cost awareness.
- Basic telemetry: log requests, durations, and failures.

***

### Week 5 – Model Abstraction & Local Models

**Focus**

- Abstract the LLM provider so you can plug in:
  - Azure OpenAI (cloud).
  - Optional local model via Ollama.

**Tasks**

- Introduce an interface, e.g., `IAiChatClient`:
  - Implement `AzureOpenAiChatClient`.
  - Optionally implement `OllamaChatClient`.
- Add configuration‑driven model selection:
  - Per environment or per feature, choose which implementation to use.

**Industry bar**

- No direct SDK calls scattered through controllers; all go through the abstraction.
- Logging includes which model/provider was used.

***

### Week 6 – RAG Fundamentals

**Focus**

- Learn the RAG pattern:
  - Chunking strategies (fixed size, recursive, semantic).
  - Document parsing (PDF/HTML/Markdown).
  - Generating embeddings and doing vector similarity.

**Tasks**

- Create `/Week06-08_RAG/Rag.Api` and `/Rag.Web` (initial version):
  - Implement:
    - Ingestion flow (for now, can be synchronous and local):
      - Upload a small document.
      - Parse it (e.g., PDF → text, Markdown → HTML → text).
      - Chunk and create embeddings with Azure OpenAI embedding models. [learn.microsoft](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/ai.openai-readme?view=azure-dotnet)
    - In‑memory or simple persistent store (e.g., file/SQLite) of embeddings.
    - Retrieval + answer generation:
      - Retrieve relevant chunks by similarity.
      - Pass them into an LLM prompt to answer questions with citations.

**Industry bar**

- Answers visibly show citations and linked sources.
- Known failure modes are handled:
  - “I don’t know” if retrieval fails.
  - Answer limited strictly to retrieved content.

***

### Week 7 – Azure AI Search Integration

**Focus**

- Move from local/in‑memory vector search to **Azure AI Search**:
  - Index design (fields, analyzers, vector fields, metadata).
  - Hybrid search (keyword + vector) and semantic ranking. [learn.microsoft](https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-dotnet)

**Tasks**

- Extend `/Rag.Api`:
  - Use Azure AI Search .NET SDK to:
    - Create an index suited for RAG (content, title, metadata, embedding vectors).
    - Ingest chunks + metadata and embeddings into the index.
    - Implement retrieval calls that:
      - Combine vector queries with keyword filters (e.g., by document type/user).
      - Use semantic ranking and captions/answers when appropriate. [learn.microsoft](https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-dotnet)
- Update `/Rag.Web` to call the new API and display:
  - Retrieved passages.
  - Confidence/rank scores (even if only in logs/diagnostics).

**Industry bar**

- Index schema is deliberate and documented (why these fields, which filters etc.).
- Retrieval quality is observed and adjusted (e.g., by tuning number of chunks, fields, or semantic config).

***

### Week 8 – Production‑Grade Ingestion Pipeline

**Focus**

- Move ingestion out of web requests.
- Use **Azure Service Bus** + .NET Worker Service for long‑running ingestion.

**Tasks**

- Create `/Rag.Ingestion.Worker`:
  - On document upload:
    - Web API stores file in Blob Storage.
    - Sends a message to Service Bus containing document ID and metadata.
  - Worker service:
    - Reads from Service Bus.
    - Downloads the document from Blob Storage.
    - Parses, chunks, embeds, and indexes content into Azure AI Search.
    - Handles retries and poison messages.

**Industry bar**

- Upload API responds quickly (no blocking on indexing).
- Failures are visible (e.g., logs, a dead‑letter queue).
- Progress tracking:
  - Store ingestion status per document (Pending/In‑Progress/Indexed/Failed).

***

### Week 9 – Advanced Retrieval & Evaluation

**Focus**

- Experiment with:
  - Different chunking strategies (fixed, recursive, semantic).
  - Reranking strategies and query rewriting.
- Evaluate retrieval quality systematically.

**Tasks**

- Extend `/Week06-08_RAG`:
  - Implement at least two different chunkers.
  - Instrument the retrieval pipeline to:
    - Log which chunks were retrieved.
    - Associate answers with retrieval metadata.
- Create a small evaluation harness:
  - A set of test questions and expected answer elements.
  - Use them to compare retrieval strategies (e.g., precision/recall qualitatively).

**Industry bar**

- You have data (even if small) on which retrieval strategy works better for your domain.
- Code supports toggling strategies via configuration (no forks of business logic).

***

### Week 10 – Agentic Workflows with Semantic Kernel

**Focus**

- Introduce **Semantic Kernel** to orchestrate:
  - Tools (plugins).
  - Retrieval.
  - Multi‑step reasoning flows. [learn.microsoft](https://learn.microsoft.com/en-us/semantic-kernel/frameworks/agent/agent-types/azure-ai-agent)

**Tasks**

- Create `/Week09-10_AgenticRag`:
  - Use Semantic Kernel to:
    - Wrap the Azure OpenAI model as a “chat completion” service.
    - Expose tools such as:
      - `SearchDocuments` (calls your RAG API or Azure AI Search).
      - `GetUserProfile` or other app‑specific tools.
    - Define an agent that:
      - Interprets a user’s intent.
      - Decides when to call tools.
      - Produces a final answer referencing retrieval results. [learn.microsoft](https://learn.microsoft.com/en-us/semantic-kernel/frameworks/agent/agent-types/azure-ai-agent)
- Optionally explore **Azure AI Agent** types documented for Semantic Kernel. [learn.microsoft](https://learn.microsoft.com/en-us/semantic-kernel/frameworks/agent/agent-types/azure-ai-agent)

**Industry bar**

- Tool use is auditable (you can see which tools were invoked with what parameters).
- Guardrails: agents cannot arbitrarily run unsafe operations; tools are narrow, well‑defined.

***

### Week 11 – Production Hardening

**Focus**

- Add production‑grade features:
  - Authentication & authorization.
  - Observability & monitoring.
  - Rate limiting and cost control.
  - Basic security and compliance considerations.

**Tasks**

- Enhance one of the apps (RAG or Agentic RAG) to be “enterprise ready”:
  - Auth:
    - Use Microsoft Entra ID (Azure AD) to protect the UI and APIs.
    - Use managed identity from the app to Azure OpenAI and Azure AI Search. [learn.microsoft](https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-dotnet)
  - Observability:
    - Integrate Application Insights.
    - Log prompt/response lengths, latency, errors, and model choices.
  - Policies:
    - Configure basic rate limiting per user/tenant.
    - Add simple cost monitoring (e.g., daily token usage log).

**Industry bar**

- You can answer:
  - Who used the system, when, and for what.
  - Which prompts and tools are the most expensive.
  - Where failures occur in the pipeline.

***

### Week 12 – Capstone Project

**Focus**

- Build a polished, end‑to‑end capstone based on your interests:
  - NotebookLM‑style multi‑document assistant.
  - AI pitch deck generator.
  - Enterprise knowledge copilot, etc.

**Tasks**

- Create `/Week11-12_Capstone`:
  - Choose one primary project; examples:
    - **NotebookLM‑style assistant**:
      - Upload multiple docs.
      - Index via your ingestion pipeline.
      - Multi‑document chat with citations.
      - “Notes” or “summary” generation.
    - **AI pitch deck generator**:
      - Prompt → outline.
      - Outline → slide content (structured JSON).
      - Export to PowerPoint or HTML slides.
- Add:
  - `docs/architecture.md`: diagrams and explanations of:
    - Components.
    - Data flow.
    - Deployment.
  - Automated deployment:
    - Azure Developer CLI, Bicep/ARM templates, or GitHub Actions for CI/CD. [learn.microsoft](https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-dotnet)

**Industry bar**

- A third‑party developer or team can:
  - Clone the repo.
  - Deploy the app using documented steps.
  - Understand the architecture from documentation alone.
- App is safe, observable, and maintainable in a realistic team setting.

***

## How to Use This Roadmap

1. **Follow weeks in order**, but feel free to adjust pace (e.g., 2 weeks per block if you’re part‑time).
2. **Create a branch per week** (e.g., `week03-prompts`), merging into `main` after you complete the week’s goals.
3. **Keep a `decisions-log.md`** in `/docs`:
   - Record why you chose certain models, chunk sizes, indexes, and agents.
4. **Refactor backward as you learn**:
   - Use insights from later weeks (e.g., better chunking, observability) to improve earlier code.

***

## References

- Azure OpenAI .NET client library overview. [learn.microsoft](https://learn.microsoft.com/en-us/dotnet/api/overview/azure/ai.openai-readme?view=azure-dotnet)
- Official .NET RAG tutorial with Azure OpenAI and Azure AI Search on App Service. [learn.microsoft](https://learn.microsoft.com/en-us/azure/app-service/tutorial-ai-openai-search-dotnet)
- Semantic Kernel agent and Azure AI Search vector store connector documentation for C#. [learn.microsoft](https://learn.microsoft.com/en-us/semantic-kernel/concepts/vector-store-connectors/out-of-the-box-connectors/azure-ai-search-connector)

***

If you share your preferred capstone (NotebookLM‑style vs. AI pitch deck vs. internal copilot), a more specialized section can be added to this README with domain‑specific requirements and acceptance criteria. What capstone idea are you currently leaning toward?
