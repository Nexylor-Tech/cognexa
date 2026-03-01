# Cognexa
An AI-powered workflow orchestration platform designed for students, researchers, and early professionals. It transforms scattered tasks, documents, meetings, and conversations into an intelligent, connected workspace that reduces administrative overload and increases time for deep, meaningful work.
# Features
### Core Features:
- Task creation, updating, and deletion
- AI-powered document upload and embedding
- Semantic document search
- Conversational chatbot with task awareness
- Task extraction from uploaded documents
### Upcoming / Scalable Features:
- Google Calendar integration
- Slack notification integration
- Role-based automation execution
- Background file processing with Redis queue
- Action logging and execution tracking
- Workflow pattern detection
- Meeting bot with summarization
- Research citation assistance
- Role based Multi-user team knowledge hubs

> [!NOTE]
> Not all the features are still hosted for lack of fund.


# How it works
```mermaid
graph TD
    User((🧑‍💻 You / User))

    subgraph The_Cognexa_System [The Cognexa Platform]
        App[💻 User Interface<br/>Dashboard & Chat]
        Librarian[📚 'The Librarian'<br/>Reads & Organizes Files]
        Memory[🗄️ Secure Project Memory<br/>Data & Knowledge Base]
        Brain[🧠 'The Brain'<br/>AI Agent & Planner]
    end

    subgraph Your_Workspace [Your Connected Workspace]
        Tasks[✅ Task Manager]
        Slack[💬 Slack]
        Calendar[📅 Google Calendar]
    end

    %% Flow 1: Learning
    User -- "1. Uploads Documents" --> App
    App -. "Sends to" .-> Librarian
    Librarian -- "Extracts & Safely Stores" --> Memory

    %% Flow 2: Interacting
    User -- "2. Gives a command\n(e.g., 'Schedule a meeting about Document A')" --> App
    App -- "Forwards to" --> Brain

    %% Flow 3: Thinking and Acting
    Brain -- "3. Looks up relevant info" --> Memory
    Brain -- "4. Takes action on your behalf" --> Tasks
    Brain -- "4. Takes action on your behalf" --> Slack
    Brain -- "4. Takes action on your behalf" --> Calendar

```
# Development
### Tech Stack
- [Backend](https://github.com/Nexylor-Tech/cognexa-backend) 
- [Frontend](https://github.com/Nexylor-Tech/cognexa-frontend)
- [Worker](https://github.com/Nexylor-Tech/worker)
- [MCP Agent](https://github.com/Nexylor-Tech/cognexa-agent)
- [MeetingBot](https://github.com/Nexylor-Tech/meetingbot) (Under development)

<details>

<summary>Backend Workflow </summary>

```mermaid

  graph TD
      subgraph Frontend_Layer [Frontend]
          Client[Web Browser / Mobile App]
      end
  
      subgraph Orchestration_Layer [AI Agent Service - Python/LangGraph]
          Agent[FastAPI Agent Service]
          Planner[LangGraph Planner]
          GeminiLLM[Gemini 1.5 Pro]
      end
  
      subgraph Backend_Layer [Core Backend - Bun/Elysia]
          API[Public REST API]
          InternalAPI[Internal Tool API]
          BetterAuth[Better Auth]
          ToolRegistry[Tool Execution Registry]
      end
  
      subgraph Async_Layer [Background Processing - Python/Dramatiq]
          Worker[Python Worker]
          Redis[(Redis Broker)]
          Chunker[Text Chunker]
          GeminiEmbed[Gemini embedding-001]
      end
  
      subgraph Storage_Layer [Data & Persistence]
          Postgres[(Postgres / Neon DB)]
          VectorDB[pgvector Extension]
          S3[(Backblaze S3 Storage)]
      end
  
      subgraph Third_Party [External Integrations]
          Slack[Slack API]
          Calendar[Google Calendar API]
      end
  
      %% Frontend Interactions
      Client -- "REST API (Auth/Projects/Files)" --> API
      Client -- "Orchestration Request" --> Agent
  
      %% Agent Flow
      Agent -- "1. Plan Workflow" --> Planner
      Planner -- "Reasoning" --> GeminiLLM
      Planner -- "2. Execute Tool (AGENT_SECRET)" --> InternalAPI
  
      %% Backend Flow
      API -- "Drizzle ORM" --> Postgres
      API -- "Upload Presigned URL" --> S3
      API -- "Enqueue Processing" --> Redis
      InternalAPI -- "Validate & Execute" --> ToolRegistry
      ToolRegistry -- "Write Tasks/Logs" --> Postgres
      ToolRegistry -- "Integrations" --> Third_Party
  
      %% Worker Flow
      Redis -- "Pick Task" --> Worker
      Worker -- "Download File" --> S3
      Worker -- "Generate Embeddings" --> GeminiEmbed
      Worker -- "Store Chunks" --> VectorDB

```

</details>



