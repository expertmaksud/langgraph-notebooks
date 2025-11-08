### 📘 Overview
This repository is a **curated, ordered collection of Jupyter notebooks** created while learning **LangGraph** — an advanced framework for building agentic systems and control flows around language models.  

The notebooks progress **from basic graph concepts** to **intermediate agent design** and culminate in **advanced agentic patterns** such as planning, routing, reflection, and multi-agent collaboration.

---

### 🚀 Learning Path (Ordered Collection)

| # | Notebook | Topic / Concept Covered |
|---|-----------|--------------------------|
| 1 | `1_introduction.ipynb` | Introduction to LangGraph — key concepts and architecture overview |
| 2 | `2_simple_graph.ipynb` | Building and executing a simple LangGraph |
| 3 | `3_build_chat_tool_graph.ipynb` | Creating a chat tool using LangGraph nodes and edges |
| 4 | `4_BasicAgent.ipynb` | Constructing a basic agent using LangGraph primitives |
| 5 | `5_BasicAgent-with-memory-checkpointing.ipynb` | Adding memory and checkpointing to a basic agent |
| 6 | `6_more_on_messages.ipynb` | Deep dive into message types, flow, and transformations |
| 7 | `7_more_on_states.ipynb` | Understanding and managing states within LangGraph |
| 8 | `8_build_chatbot_summarization_and_stream.ipynb` | Implementing a chatbot with summarization and streaming |
| 9 | `9_controllability-breakpoints-and-timetravel.ipynb` | Introducing controllability: breakpoints, replay, and time travel |
| 10 | `10_controllability-parallelization_subgraph.ipynb` | Exploring parallelization and subgraph execution |
| 11 | `11_build_research_assistant.ipynb` | Building an AI-powered research assistant using LangGraph |
| 12 | `12_memory.ipynb` | Implementing memory architectures in LangGraph agents |
| 13 | `13_memory_with_profile-and-collection.ipynb` | Advanced memory with profiles and context collections |
| 14 | `14_Building_Chatbot_Agent.ipynb` | Building a fully functional chatbot agent end-to-end |
| 15.1 | `15_1_pattern_prompt_chaining.ipynb` | Pattern 1: Prompt chaining for structured reasoning |
| 15.2 | `15_2_pattern_routing.ipynb` | Pattern 2: Intelligent routing between nodes or agents |
| 15.3 | `15_3_pattern_parallelization.ipynb` | Pattern 3: Running subgraphs in parallel for efficiency |
| 15.4 | `15_4_pattern_reflection.ipynb` | Pattern 4: Implementing self-reflection and improvement loops |
| 15.5 | `15_5_pattern_tool_calling.ipynb` | Pattern 5: Tool use and external function integration |
| 15.6 | `15_6_pattern_planning.ipynb` | Pattern 6: Planning and decomposition of complex tasks |
| 15.7 | `15_7_multi-agent.ipynb` | Pattern 7: Multi-agent collaboration and coordination |

---

### 🧩 Learning Goals
By following this notebook series, you will learn to:
- Understand the **core concepts of LangGraph** — nodes, edges, states, and messages.  
- Build **memory-aware agents** capable of reasoning over context.  
- Apply **controllability tools** (breakpoints, time-travel, subgraphs).  
- Design **advanced agentic behaviors** using patterns such as reflection, routing, planning, and collaboration.  
- Construct **multi-agent systems** with context isolation and shared memory.  

---

### 🧱 Structure & Progression
Each notebook builds on the previous one, gradually introducing new capabilities:
- **Basic (1–4)** → Foundations of LangGraph and simple agents  
- **Intermediate (5–11)** → Adding memory, control, and real-world use cases  
- **Advanced (12–15.7)** → Patterns for scalable, multi-agent design  

---

### 🏗️ Prerequisites
- Python 3.10+  
- Installed packages: `langgraph`, `langchain-core`, `openai` or equivalent LLM backend, `python-dotenv` 
- Jupyter / VS Code Notebook environment  

---

### 💡 Tip
Use this collection as a **hands-on learning roadmap** — run each notebook sequentially to see LangGraph’s full power evolve from simple graphs to autonomous, context-driven agents.

---