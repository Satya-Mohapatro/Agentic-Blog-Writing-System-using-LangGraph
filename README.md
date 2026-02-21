# Agentic Blog Writing System (LangGraph Multi-Agent Pipeline)

An autonomous, multi-agent blog generation system built using **LangGraph**, **OpenAI**, **Tavily**, **Gemini Image Model**, and **Streamlit**.

This project implements a state-driven, multi-agent architecture that intelligently routes topics, performs optional web research, decomposes content into structured tasks, generates sections in parallel, and automatically produces technical diagrams — delivering publication-ready Markdown blogs with downloadable images.

---
##  Project Architecture

The following diagram illustrates the end-to-end architecture of the Agentic Blog Writing System.

![Project Architecture](Documents/Architecture-Daigram.png)

---
##  Key Features

- **Intelligent Routing Agent**
  - Classifies topics into:
    - `closed_book` (evergreen)
    - `hybrid` (semi-recent)
    - `open_book` (news / latest updates)
  - Decides whether web research is required.

- **Research Agent (Tool-Integrated)**
  - Uses Tavily Search API
  - Extracts structured evidence
  - Applies recency filtering
  - Prevents hallucinated citations

- **Planning Agent (Orchestrator)**
  - Generates structured blog outline
  - Breaks blog into 5–9 tasks
  - Assigns section goals and word limits
  - Flags citation/code requirements

- **Parallel Worker Agents**
  - Each worker writes one blog section
  - Executes in parallel via LangGraph fan-out
  - Enforces structured Markdown output
  - Restricts citations to approved evidence

- **Reducer Subgraph**
  - Merges sections deterministically
  - Runs Image Planning Agent
  - Generates diagrams using Gemini
  - Inserts images into Markdown

- **AI-Generated Diagrams**
  - Automatically creates technical visuals
  - Inserts labeled images with captions
  - Gracefully handles generation failures

- **Streamlit UI**
  - Real-time graph execution streaming
  - Plan, Evidence, Preview, Images tabs
  - Download Markdown
  - Download full bundle (MD + images)
  - Load past generated blogs

---
## Agent Roles

| Agent | Responsibility |
|--------|---------------|
| Router | Determines strategy & research mode |
| Research | Fetches and structures web evidence |
| Orchestrator | Decomposes blog into structured tasks |
| Worker | Writes individual sections |
| Reducer | Merges sections |
| Image Planner | Decides where visuals are needed |
| Image Generator | Creates diagrams via Gemini |

---
## Technologies Used

- **LangGraph** – Multi-agent orchestration framework used to build a state-driven execution graph with conditional routing, parallel workers, and reducer subgraphs.

- **OpenAI GPT-4.1-mini** – Core reasoning and content generation model used for routing decisions, blog planning, structured task decomposition, and section writing.

- **Tavily API** – Web research tool integrated into the Research Agent for fetching up-to-date information and grounding blog content with structured evidence.

- **Gemini Image Model** – AI-powered image generation model used to create technical diagrams and visuals dynamically during the reducer phase.

- **Streamlit** – Frontend interface for user interaction, real-time graph execution streaming, blog preview rendering, and download functionality.

- **Pydantic** – Schema validation library used to enforce structured LLM outputs (e.g., Plan, Task, EvidenceItem, RouterDecision, ImageSpec).
