# Chapter 4: Building Agents with Microsoft Foundry — Exam Cheat Sheet

## Agent Architecture Components
| Component | Purpose | Exam Tip |
|-----------|---------|----------|
| Role | Defines agent's persona/expertise | Used by coordinator for routing decisions |
| Goals | Agent's objectives | Guides tool selection and response style |
| Instructions | System prompt with behaviors | Most important for tool usage accuracy |
| Conversation tracking | Maintains dialogue state | Chooses which memory type to use |
| State management | Stores variables across turns | Exam: distinguish from conversation memory |

## Memory Systems Comparison
| Type | Use Case | Token Impact | When Exam Says... |
|------|----------|--------------|-------------------|
| Conversation history | Short conversations (<20 turns) | High (full verbatim) | "Need exact conversation replay" |
| Summary memory | Long conversations (50+ turns) | Medium (progressive summaries) | "Long conversation + token limits" |
| Entity memory | Track people, dates, IDs | Low (structured data only) | "Need to remember customer details" |
| Vector memory | Semantic search past conversations | Medium (embeddings) | "Retrieve similar past interactions" |

## Tool Integration Patterns
```json
// Function calling schema (memorize structure)
{
  "name": "get_inventory",
  "description": "Retrieves current stock levels for product",
  "parameters": {
    "type": "object",
    "properties": {
      "product_id": {"type": "string", "description": "SKU or ID"},
      "location": {"type": "string", "description": "Warehouse code"}
    },
    "required": ["product_id"]
  }
}
```
**Exam trap:** If parameter missing and marked required → agent asks user. If optional → proceeds without it.

## Multi-Agent Orchestration Patterns
| Pattern | Workflow | Use When | Exam Keywords |
|---------|----------|----------|---------------|
| Sequential | A → B → C (output chains) | Dependencies between agents | "Approval workflow", "review then process" |
| Parallel | A, B, C simultaneously | Independent tasks | "Simultaneously", "merge results" |
| Hierarchical | A spawns B, B spawns C | Nested subtasks | "Specialist spawns sub-agents" (rare answer) |

## Safeguards — Critical for Exam
| Safeguard | What It Does | Exam Scenario |
|-----------|--------------|---------------|
| Tool-access controls | Permission check before function execution | "External users cannot access pricing data" |
| Approval workflows | Human approves before execution | "DELETE operations need approval" |
| Autonomous mode | Agent executes all tools freely | "Fully automated responses" |
| Semiautonomous mode | Agent asks approval for flagged functions | "High-risk operations need oversight" |
| Conversation boundaries | Limits allowed topics/domains | "Agent should not discuss politics" |

**Exam rule:** DELETE, financial transactions, data modification → semiautonomous mode

## Agent Evaluation Metrics
| Metric | Measures | Good Score | Exam: If Low Score Indicates... |
|--------|----------|------------|--------------------------------|
| Task completion rate | % of successfully finished tasks | >90% | Agent failing to complete objectives |
| Tool usage accuracy | % correct tool selection | >85% | Poor tool descriptions or too many similar tools |
| Conversation coherence | Multi-turn dialogue quality | >0.8 | Memory/context management issues |
| Error rate | % of responses with errors | <5% | Hallucinations or bad function calls |

## Common Exam Traps
1. **Multi-agent vs single-agent:** Don't over-architect. If agent just needs to choose between tools → single agent. If complex workflow with handoffs → multi-agent.
2. **Memory type confusion:** Summary memory ≠ entity memory. Summary = condensed conversation. Entity = extracted structured data.
3. **Parallel vs sequential:** If one agent's OUTPUT is another's INPUT → sequential. If tasks are independent → parallel.
4. **Tool schema 'required' field:** Agent will ask user for required parameters. Optional parameters → agent proceeds without asking.
5. **Autonomous vs semiautonomous:** Autonomous = zero human approval. Semiautonomous = approval for flagged functions only (not all functions).

## Commands to Memorize
```python
# Tool registration (pseudo-code pattern)
agent.register_tool(
    name="query_database",
    function=execute_query,
    schema=tool_schema,
    requires_approval=True  # Exam: high-risk operations
)

# Orchestration pattern
coordinator.execute(
    mode="parallel",
    agents=[legal_agent, finance_agent, hr_agent],
    wait_for_all=False,  # Exam: allows partial results
    timeout=30
)
```

## Quick Decision Tree for Exam
- **Question mentions "approval before execution"** → Semiautonomous mode
- **Question mentions "long conversation + token limits"** → Summary memory
- **Question mentions "Agent A's output feeds Agent B"** → Sequential orchestration
- **Question mentions "multiple agents execute simultaneously"** → Parallel orchestration
- **Question mentions "role-based data access"** → Tool-access controls
- **Question mentions "topic restrictions"** → Conversation boundaries
