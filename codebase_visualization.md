# DAIV Codebase Visualization

## Project Overview

DAIV (Development AI Assistant) is an open-source automation assistant designed to enhance developer productivity by integrating with GitLab repositories to automate common software development tasks.

```
                                 +-------------------+
                                 |       DAIV        |
                                 | (Django Backend)  |
                                 +-------------------+
                                          |
                +-----------------------------------------------------+
                |                         |                           |
    +-----------v-----------+  +---------v---------+  +-------------v------------+
    |      Automation       |  |     Codebase      |  |          Chat           |
    | (AI Agents & Tools)   |  | (Code Management) |  | (Interaction Interface) |
    +-----------------------+  +-------------------+  +--------------------------+
                |                         |                           |
    +-----------v-----------+  +---------v---------+  +-------------v------------+
    |        Agents         |  |  Search Engines   |  |         API Layer        |
    | (Task-specific Bots)  |  | (Code Retrieval)  |  | (OpenAI-compatible API) |
    +-----------------------+  +-------------------+  +--------------------------+
```

## Core Components

### 1. Automation System

The automation system is responsible for executing AI-driven tasks on the codebase.

```
+---------------------------+
|      Automation Agents    |
+---------------------------+
| - issue_addressor         | <- Resolves GitLab issues
| - review_addressor        | <- Handles code review comments
| - pipeline_fixer          | <- Fixes failing CI/CD pipelines
| - codebase_chat           | <- Enables chat with codebase
| - codebase_search         | <- Searches through code
| - code_describer          | <- Generates code descriptions
| - pr_describer            | <- Describes pull requests
| - snippet_replacer        | <- Replaces code snippets
| - plan_and_execute        | <- Plans and executes tasks
| - image_url_extractor     | <- Extracts image URLs
+---------------------------+
            |
            v
+---------------------------+
|      Automation Tools     |
+---------------------------+
| - repository.py           | <- Repository interaction
| - sandbox.py              | <- Safe code execution
| - web_search.py           | <- Web search capabilities
| - toolkits.py             | <- Tool collections
+---------------------------+
```

### 2. Codebase Management

The codebase management system handles code indexing, storage, and retrieval.

```
+---------------------------+
|    Codebase Management    |
+---------------------------+
| - clients.py              | <- GitLab API clients
| - indexes.py              | <- Code indexing
| - document_loaders.py     | <- Code loading
| - models.py               | <- Database models
| - tasks.py                | <- Async tasks
+---------------------------+
            |
            v
+---------------------------+
|      Search Engines       |
+---------------------------+
| - semantic.py             | <- Vector-based search (PGVector)
| - lexical.py              | <- Keyword-based search (Tantivy)
| - retrievers.py           | <- Search result retrieval
+---------------------------+
```

### 3. Chat Interface

The chat interface provides an OpenAI-compatible API for interacting with the codebase.

```
+---------------------------+
|      Chat Interface       |
+---------------------------+
| - api/                    | <- API endpoints
| - conf.py                 | <- Configuration
+---------------------------+
```

## Technology Stack

```
+---------------------------+
|     Technology Stack      |
+---------------------------+
| - Django                  | <- Web framework
| - Celery + Redis          | <- Async task processing
| - LangChain & LangGraph   | <- LLM integration
| - PGVector                | <- Semantic search
| - Tantivy                 | <- Lexical search
| - GitLab API              | <- Repository integration
| - Langsmith               | <- Observability
+---------------------------+
```

## Data Flow

```
+-------------+     +-------------+     +-------------+
|  GitLab     |     |    DAIV     |     |    User     |
|  Repository |<--->|   System    |<--->|  Interface  |
+-------------+     +-------------+     +-------------+
                          |
                          v
                    +-------------+
                    |     LLM     |
                    |   Services  |
                    +-------------+
```

## Key Workflows

### Issue Resolution Workflow

```
User creates issue
       |
       v
DAIV parses issue
       |
       v
Agent proposes plan
       |
       v
Agent executes code changes
       |
       v
Create merge request
```

### Code Review Workflow

```
User requests review
       |
       v
DAIV analyzes code
       |
       v
Agent provides feedback
       |
       v
Apply suggested changes
```

### Pipeline Fix Workflow

```
CI/CD pipeline fails
       |
       v
DAIV analyzes failure
       |
       v
Agent identifies issue
       |
       v
Agent proposes/applies fix
```

### Codebase Chat Workflow

```
User asks question
       |
       v
DAIV searches codebase
       |
       v
Retrieve relevant context
       |
       v
Generate context-aware answer
```