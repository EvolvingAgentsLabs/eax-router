# **EAX Router 🧠⚡**

**(Evolving Agents Experimental Router)**

The intelligent task router for **[LLMunix](https://github.com/EvolvingAgentsLabs/llmunix)** networks. It dynamically selects the optimal LLM for any given task based on cost, latency, and quality, enabling sophisticated multi-model agent workflows.

<p align="center">
  <a href="https://github.com/EvolvingAgentsLabs/llmunix/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="License"></a>
  <a href="https://evolvingagentslabs.github.io/"><img src="https://img.shields.io/badge/labs-EvolvingAgentsLabs-brightgreen" alt="Labs"></a>
  <a href="#"><img src="https://img.shields.io/badge/status-alpha_implementation-orange.svg" alt="Status"></a>
</p>

> 🌐 **Part of the [Evolving Agents Labs](https://evolvingagentslabs.github.io) Research Initiative**
>
> This project is a foundational component for building adaptive AI systems, inspired by our research in the `LLMunix` project.

---

## **The Problem: One Model Can't Rule Them All**

A single, powerful LLM is like a brilliant but expensive senior consultant. You wouldn't ask them to do routine data entry. Modern agentic workflows involve diverse tasks: quick summarizations, complex code generation, creative writing, and deep analysis. Using one "max-power" model for everything is inefficient, slow, and costly.

`LLMunix` needs a "Chief Logistics Officer"—an agent that can look at an incoming task, understand its requirements, and delegate it to the right cognitive engine, whether that's a fast, cheap local model or a powerful cloud-based one.

## **The Solution: A `RouterAgent` for `LLMunix`**

The EAX Router is not a separate application but a **specialized agent role** within the `LLMunix` ecosystem. It is an `LLMunix` agent whose `GEMINI.md` firmware is programmed to perform one job: **intelligent task routing**.

It works by leveraging the existing virtual tools in `LLMunix`, especially the `LocalLLMTool` and `CloudLLMTool`, to create a multi-model execution layer.

### **How It Works: An Implemented `LLMunix` Workflow**

1.  **Task Arrival:** An agent (e.g., `SystemAgent`) receives a complex goal from the user.
2.  **Request for Routing:** Instead of executing a sub-task itself, the `SystemAgent` sends a **[SAL-CP](https://github.com/EvolvingAgentsLabs/sal-cp)** message to the `RouterAgent`.
    ```json
    {
      "to": "RouterAgent",
      "payload": {
        "type": "ROUTING_REQUEST",
        "content": "Summarize this 10-page document.",
        "constraints": ["priority: cost", "max_latency: 5_seconds"]
      }
    }
    ```
3.  **Intelligent Decision:** The `RouterAgent`'s firmware (`GEMINI.md`) guides its logic:
    *   "The task is 'summarization' and the priority is 'cost'."
    *   "My knowledge base (`permanent_memory`) says that the local `phi3` model is cheap and excellent for summarization."
    *   "I will route this task to the `LocalLLMTool`."
4.  **Tool Execution:** The `RouterAgent` calls the appropriate `LLMunix` virtual tool.
    ```
    run_tool(
        path="components/tools/LocalLLMTool.md", 
        arguments={ "model": "phi3", "prompt": "Summarize: ..." }
    )
    ```
5.  **Response Forwarding:** The `RouterAgent` receives the result from the tool and sends it back to the original requesting agent via another SAL-CP message.

### **The RouterAgent's Firmware (`GEMINI.md`)**

The entire logic of the router lives in its `GEMINI.md` manifest.

```markdown
# RouterAgent Firmware v1.0
You are an intelligent task router for an LLM agent network. Your goal is to select the most appropriate cognitive engine (LLM) for a given task based on its type and constraints.

**Your Decision-Making Process:**
1.  Analyze the `ROUTING_REQUEST` payload and its `constraints`.
2.  Classify the task type (e.g., `summarization`, `code_generation`, `creative_writing`, `complex_reasoning`).
3.  Consult your `permanent_memory` for model performance data (your "fingerprints").
4.  Select the optimal tool (`LocalLLMTool` for Ollama, `CloudLLMTool` for OpenAI/Anthropic) and model.
5.  Execute the tool and return the result to the requesting agent.

**Routing Rules:**
-   **IF** priority is `cost` or `privacy`, **THEN** prefer `LocalLLMTool` (Ollama).
-   **IF** task is `complex_reasoning` or priority is `quality`, **THEN** prefer `CloudLLMTool` with a frontier model like `gpt-4o` or `claude-3-5-sonnet`.
-   **IF** task is `summarization` or `data_extraction`, **THEN** a small local model like `phi3` is sufficient.
-   **IF** a cloud tool fails, **THEN** fallback to a local model as a backup.
```
This firmware turns a generic `LLMunix` agent into a powerful, specialized router.

## **Core Research Areas Implemented in this Approach**

### **1. The Model Fingerprint as `Permanent Memory`**

The `EAX Router`'s "fingerprint" database is implemented directly using the `LLMunix` permanent memory system. The `RouterAgent` learns and stores performance data over time.

**Example Memory Entry:** `system/memory/permanent/model_fingerprint_claude-3-5-sonnet.md`
```markdown
[2025-07-06 11:00:00] **Capability:** code_generation, **Quality Score:** 9.5/10, **Avg Latency:** 2500ms, **Notes:** Excellent for Python script generation.
[2025-07-06 11:05:15] **Capability:** summarization, **Quality Score:** 9.0/10, **Avg Latency:** 1500ms, **Notes:** Very coherent but can be verbose.
```
The `RouterAgent` uses `memory_search` on this data to inform its routing decisions.

### **2. Pluggable Routing Strategies via Firmware**

Different routing strategies (`CostOptimized`, `QualityOptimized`) are not complex code modules but simply different instruction sets within the `RouterAgent`'s `GEMINI.md` firmware. To switch strategies, you can simply provide the agent with a different manifest file.

### **3. Provider-Agnosticism through Virtual Tools**

`LLMunix` already provides the foundation for this. The `SystemAgent` doesn't need to know about OpenAI or Ollama's specific APIs. It only needs to know about the `run_tool` command and the existence of:
*   `components/tools/LocalLLMTool.md`
*   `components/tools/CloudLLMTool.md`

The low-level implementation details (like `curl` commands and API key handling) are abstracted away inside these tool definitions, making the routing logic clean and high-level.

## **Example Usage: Running the EAX Router in `LLMunix`**

This workflow can be executed today with the `LLMunix` framework.

1.  **Ensure RouterAgent Exists:** Create `components/agents/RouterAgent.md` with the firmware described above.
2.  **Ensure LLM Tools Exist:** Ensure `LocalLLMTool.md` and `CloudLLMTool.md` are in `components/tools/`.
3.  **Launch `LLMunix`:**
    ```bash
    ./llmunix-boot
    gemini
    ```
4.  **Provide a Goal That Requires Routing:**
    > `"I need to summarize a long document, but I want to keep my costs as low as possible. Please route this task appropriately."`

**Expected Autonomous Behavior:**

1.  The `SystemAgent` receives the goal and recognizes the "route this task" instruction.
2.  It sends a `ROUTING_REQUEST` message to the `RouterAgent` via `send_message`, including the constraint `"priority: cost"`.
3.  The `RouterAgent` checks its messages, parses the request, and its firmware guides it to choose the local model.
4.  It executes `run_tool` with the path to `LocalLLMTool.md`, passing the document for summarization.
5.  The result from the local model is returned to the `SystemAgent`, which then presents it to the user.
6.  (Optional) The `RouterAgent` could then use `memory_store` to log the performance of this routing decision, refining its future choices.

## **Research Roadmap & Future Potential**

The implementation of the `RouterAgent` within `LLMunix` is just the first step. This foundation enables further research into:

*   **Learning Router Strategies:** An advanced `RouterAgent` could analyze the results of its past routing decisions (e.g., "When I routed a coding task to `phi3`, the `QAReviewAgent` rejected the output 80% of the time") and update its own `GEMINI.md` firmware to improve its routing rules.
*   **Integration with EAX Marketplace:** The router can act as the entry point to the marketplace. Instead of executing a task itself, it could post an RFB to the marketplace, letting specialized agents bid on the work.
*   **Load Balancing and Health Checks:** The router could be extended with a `check_agent_health` tool that pings other models/agents to check their status and current load before routing a task.

By implementing the EAX Router as a specialized `LLMunix` agent, we create a powerful, transparent, and highly extensible system for exploring the future of intelligent, multi-model AI workflows.

## License

EAX Router is licensed under the Apache 2.0 License.

---

## Connect

**Research Contributors**: [Matias Molinas](https://github.com/matiasmolinas) • [Ismael Faro](https://github.com/ismaelfaro)

**Organization**: [EvolvingAgentsLabs](https://github.com/EvolvingAgentsLabs) • **Lab Site**: [evolvingagentslabs.github.io](https://evolvingagentslabs.github.io)

---
*EAX Router is a core component of the experimental agent architecture being developed at [Evolving Agents Labs](https://evolvingagentslabs.github.io). Our goal is to build the foundational tools for truly intelligent and adaptive AI systems.*