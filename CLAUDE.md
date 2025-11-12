# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **progressive learning collection** of 20 Jupyter notebooks teaching LangGraph — a framework for building agentic AI systems with language models. The notebooks are designed to be completed **sequentially**, with each building on concepts from previous ones.

## Environment Setup

**Required API Keys:**
- `OPENAI_API_KEY` - For GPT-4o/GPT-4o-mini models
- `TAVILY_API_KEY` - For web search functionality

These should be configured as environment variables or in a `.env` file (gitignored).

**Core Dependencies:**
- `langgraph` (v0.6.8+)
- `langchain-core`, `langchain-openai`, `langchain-tavily`, `langchain-community`
- `python-dotenv`
- `pydantic`

**Execution Environment:**
Notebooks must be run in Jupyter or VS Code Notebook environment. Python 3.10+ required.

## Learning Path Architecture

The repository follows a strict progression:

**Phase 1: Foundations (1-4)**
- Graph basics, state management, tool integration, simple agents
- No memory between invocations

**Phase 2: Intermediate (5-11)**
- Introduces memory (MemorySaver), checkpointing, thread-based persistence
- Message manipulation, state schemas (TypedDict/Dataclass/Pydantic)
- Streaming modes, human-in-the-loop patterns
- Real-world example: Multi-agent research assistant (Notebook 11)

**Phase 3: Advanced (12-14)**
- Cross-thread memory (InMemoryStore)
- Long-term user profiles and preferences
- Production chatbot patterns

**Phase 4: Design Patterns (15.1-15.7)**
- **15.1**: Prompt Chaining - Sequential task decomposition
- **15.2**: Routing - Dynamic decision-making (LLM/embedding/rule-based)
- **15.3**: Parallelization - Concurrent execution with Send()
- **15.4**: Reflection - Producer-critic self-improvement loops
- **15.5**: Tool Calling - External function integration
- **15.6**: Planning - Goal decomposition with adaptive replanning
- **15.7**: Multi-Agent - Supervisor and subordinate coordination

## Key Architectural Concepts

### State Management
- **State Schemas**: Use TypedDict for simplicity with LangGraph reducers, Pydantic when validation needed
- **Reducers**: Functions that merge state updates (e.g., `add_messages`, `operator.add`)
- **Partial Updates**: Nodes return dictionaries with only changed keys

### Memory Architecture
Two-tier system introduced progressively:
- **Short-term (MemorySaver)**: Thread-scoped conversation history using `thread_id`
- **Long-term (InMemoryStore)**: Cross-thread persistence using namespace organization (introduced in Notebook 12)

### Graph Execution Model
```
Node Function → State Update → Checkpoint → Conditional Edge → Next Node
```
- **Nodes**: Transform state, return partial updates
- **Edges**: Regular (deterministic) or conditional (function-based routing)
- **Checkpointer**: Enables persistence, time-travel, and interrupts

### Message Flow Pattern
```
HumanMessage → AIMessage (with tool_calls) → ToolMessage → AIMessage (final)
```
- LLM decides which tools to call (generates `tool_calls` in AIMessage)
- ToolNode executes tools (generates ToolMessages)
- Graph orchestrates the flow with conditional routing

### Human-in-the-Loop Pattern
- `interrupt_before=["node_name"]`: Pause execution before specified nodes
- `graph.update_state()`: Manually modify state during pause
- `get_state()`: Inspect current state and history
- Enable approval workflows and manual overrides

## Critical Patterns Across Notebooks

### ReAct Agent Loop (Notebook 4+)
```
Agent Node (LLM reasoning) → Tools Node (execution) → Conditional Check → [Continue | End]
```
Foundation for most agent implementations.

### Map-Reduce with Send() (Notebook 11)
For parallel operations on collections:
```python
return [Send("worker_node", {"data": item}) for item in items]
```
All Send() invocations execute in parallel, results collected automatically.

### Structured Outputs (Notebook 11+)
Use `llm.with_structured_output(PydanticModel)` for predictable, parseable LLM responses. Critical for multi-step workflows requiring specific data formats.

### Streaming Modes
- `stream_mode="values"`: Full state after each node (default)
- `stream_mode="updates"`: Only state changes per node
- `astream_events()`: Token-level streaming for UI updates

### State History & Time Travel (Notebook 9)
```python
for state in graph.get_state_history(config):
    # Access historical checkpoints
    graph.update_state(state.config, values, as_node=...)  # Replay from checkpoint
```

## Working with Notebooks

### Understanding Node Definitions
Nodes are functions matching this signature:
```python
def node_name(state: StateSchemaType) -> dict:
    # Transform state
    return {"key": updated_value}  # Partial update merged via reducer
```

### Conditional Edges
Return routing decision based on state:
```python
def router(state: StateType) -> Literal["next_node", "__end__"]:
    if state["condition"]:
        return "next_node"
    return END
```

### Thread Management
Each conversation requires unique `thread_id` in config:
```python
config = {"configurable": {"thread_id": "unique-id"}}
graph.invoke(input, config)  # State persisted to this thread
```

### Debugging Graph Structure
All notebooks include graph visualization:
```python
from IPython.display import Image, display
display(Image(graph.get_graph().draw_mermaid_png()))
```

## Notable Implementation: Research Assistant (Notebook 11)

Most complex example demonstrating production patterns:
1. **Generate Analysts**: LLM creates specialized AI personas using `with_structured_output()`
2. **Human Feedback**: `interrupt_before` allows analyst approval/modification
3. **Interview Subgraphs**: Nested graphs for multi-turn expert interviews
4. **Parallel Execution**: Map-reduce pattern with `Send()` for concurrent interviews
5. **Report Synthesis**: Aggregates findings from multiple sources with citations

Key techniques:
- Subgraph composition for reusable workflows
- Dynamic parallel task generation
- Human-in-the-loop with state updates
- Multi-source information gathering (Tavily + Wikipedia)

## Notebook Dependencies

Later notebooks assume concepts from earlier ones:
- **Memory concepts** (5) required for chatbot patterns (8, 14)
- **Message manipulation** (6) required for understanding streaming (8)
- **State schemas** (7) required for complex agents (11+)
- **Controllability** (9) required for human-in-the-loop patterns (11)
- **Basic patterns** (15.1-15.5) required for planning (15.6) and multi-agent (15.7)

When modifying or creating notebooks, maintain the progressive complexity curve.

## Common Patterns to Reference

- **Simple agent with tools**: Notebook 4
- **Memory-enabled conversation**: Notebook 5
- **Streaming implementation**: Notebook 8
- **Human approval workflow**: Notebooks 9, 11
- **Multi-agent coordination**: Notebooks 11, 15.7
- **Iterative refinement**: Notebook 15.4
- **Dynamic planning**: Notebook 15.6

## Graph Compilation

Standard pattern across all notebooks:
```python
graph = StateGraph(StateSchema)
# Add nodes, edges
graph.set_entry_point("start_node")
graph.add_conditional_edges("node", router, {...})
graph.add_edge("node", "next")
app = graph.compile(checkpointer=memory, interrupt_before=[...])
```

Compilation parameters:
- `checkpointer`: Enable persistence (MemorySaver, etc.)
- `interrupt_before`: Human-in-the-loop breakpoints
- `interrupt_after`: Post-execution pauses
