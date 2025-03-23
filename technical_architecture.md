# DAIV Technical Architecture

## System Architecture

```mermaid
graph TD
    subgraph Django_Framework
        daiv[DAIV Django Project]
        daiv --> accounts[Accounts App]
        daiv --> automation[Automation App]
        daiv --> chat[Chat App]
        daiv --> codebase[Codebase App]
        daiv --> core[Core App]
    end

    subgraph External_Services
        gitlab[GitLab API]
        llm[LLM Services]
        redis[Redis]
        postgres[PostgreSQL + PGVector]
        tantivy[Tantivy Search]
        langsmith[Langsmith]
    end

    subgraph Task_Processing
        celery[Celery Workers]
    end

    automation --> agents[AI Agents]
    automation --> tools[Agent Tools]
    
    codebase --> search_engines[Search Engines]
    codebase --> document_loaders[Document Loaders]
    codebase --> indexes[Code Indexes]
    
    agents --> tools
    tools --> gitlab
    tools --> llm
    
    codebase --> gitlab
    search_engines --> postgres
    search_engines --> tantivy
    
    daiv --> celery
    celery --> redis
    daiv --> postgres
    
    agents --> langsmith
```

## Component Dependencies

```mermaid
classDiagram
    class AutomationAgent {
        +run()
        +get_tools()
        +get_prompt()
    }
    
    class Tool {
        +name: str
        +description: str
        +_run()
        +run()
    }
    
    class RepositoryTool {
        +get_file()
        +list_files()
        +search_files()
        +apply_changes()
    }
    
    class SearchEngine {
        +index()
        +search()
        +delete()
    }
    
    class SemanticSearch {
        +index()
        +search()
        +delete()
    }
    
    class LexicalSearch {
        +index()
        +search()
        +delete()
    }
    
    class CodebaseManager {
        +index_repository()
        +search()
        +get_file()
    }
    
    class GitLabClient {
        +get_file()
        +list_files()
        +create_merge_request()
        +get_issues()
    }
    
    AutomationAgent --> Tool : uses
    Tool <|-- RepositoryTool : extends
    RepositoryTool --> GitLabClient : uses
    CodebaseManager --> SearchEngine : uses
    SearchEngine <|-- SemanticSearch : implements
    SearchEngine <|-- LexicalSearch : implements
    CodebaseManager --> GitLabClient : uses
```

## Data Models

```mermaid
erDiagram
    Repository ||--o{ CodebaseIndex : has
    Repository {
        string url
        string name
        string default_branch
        datetime last_indexed
    }
    
    CodebaseIndex ||--o{ Document : contains
    CodebaseIndex {
        string name
        string engine_type
        datetime created_at
        datetime updated_at
    }
    
    Document {
        string path
        string content
        string metadata
        datetime indexed_at
    }
    
    Repository ||--o{ Issue : has
    Issue {
        int id
        string title
        string description
        string status
        datetime created_at
    }
    
    Repository ||--o{ MergeRequest : has
    MergeRequest {
        int id
        string title
        string description
        string source_branch
        string target_branch
        string status
    }
```

## Key Workflows

### Issue Resolution Flow

```mermaid
sequenceDiagram
    participant User
    participant GitLab
    participant DAIV
    participant IssueAddressor
    participant RepositoryTools
    participant LLM
    
    User->>GitLab: Create issue
    GitLab->>DAIV: Webhook notification
    DAIV->>IssueAddressor: Process issue
    IssueAddressor->>RepositoryTools: Get repository info
    RepositoryTools->>GitLab: Fetch repository files
    GitLab-->>RepositoryTools: Return files
    IssueAddressor->>LLM: Generate solution plan
    LLM-->>IssueAddressor: Return plan
    IssueAddressor->>RepositoryTools: Apply code changes
    RepositoryTools->>GitLab: Create merge request
    GitLab-->>User: Notify of merge request
```

### Codebase Chat Flow

```mermaid
sequenceDiagram
    participant User
    participant ChatAPI
    participant CodebaseChat
    participant SearchEngines
    participant LLM
    
    User->>ChatAPI: Ask question
    ChatAPI->>CodebaseChat: Process question
    CodebaseChat->>SearchEngines: Search for relevant code
    SearchEngines-->>CodebaseChat: Return code snippets
    CodebaseChat->>LLM: Generate answer with context
    LLM-->>CodebaseChat: Return answer
    CodebaseChat-->>ChatAPI: Return formatted response
    ChatAPI-->>User: Display answer
```

## Technology Integration

```mermaid
flowchart TD
    subgraph Frontend
        api[OpenAI-compatible API]
    end
    
    subgraph Backend
        django[Django]
        celery[Celery]
        langchain[LangChain]
        langgraph[LangGraph]
    end
    
    subgraph Storage
        postgres[PostgreSQL]
        pgvector[PGVector Extension]
        redis[Redis]
        tantivy[Tantivy]
    end
    
    subgraph External
        gitlab[GitLab API]
        llm[LLM Services]
        langsmith[Langsmith]
    end
    
    api --> django
    django --> langchain
    django --> celery
    celery --> redis
    langchain --> langgraph
    langchain --> llm
    langgraph --> langsmith
    django --> postgres
    postgres --> pgvector
    django --> tantivy
    django --> gitlab
```