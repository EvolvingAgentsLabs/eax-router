# EAX Router 🧠⚡

**(Evolving Agents Experimental Router)**

An experimental, intelligent, and pluggable routing layer for Large Language Models.

<p align="center">
  <a href="https://github.com/EvolvingAgentsLabs/eax-router/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="License"></a>
  <a href="https://evolvingagentslabs.github.io/"><img src="https://img.shields.io/badge/labs-EvolvingAgentsLabs-brightgreen" alt="Labs"></a>
  <a href="#"><img src="https://img.shields.io/badge/status-alpha_experiment-orange.svg" alt="Status"></a>
</p>

> 🌐 **Part of the [Evolving Agents Labs](https://evolvingagentslabs.github.io) Research Initiative**
>
> This project is a foundational component for building adaptive AI systems, inspired by our research in the [llmunix-spec](https://github.com/EvolvingAgentsLabs/llmunix-spec) project.

---

## ⚠️ Experimental Research Project

**Important**: This is an early-stage research prototype exploring intelligent LLM routing. It should be treated as research material, not a production-ready system. This project will remain permanently in alpha status as we explore the future of adaptive agent architecture.

---

## The Research Hypothesis: Context-Aware Model Selection

Modern AI systems and agents perform many different cognitive tasks within a single workflow: summarizing text, writing code, analyzing data, and reasoning through a plan. However, most systems are hardcoded to use a single, general-purpose LLM for all these tasks. This is inefficient and expensive.

Our research explores a fundamental question: **Can an agentic system dynamically select the optimal LLM for each discrete cognitive task, based on the task's unique requirements and constraints?**

This leads to several problems we are investigating:
- **High Costs:** Using frontier models for simple tasks is wasteful.
- **Suboptimal Performance:** Using cheap models for complex reasoning leads to failure.
- **Lack of Adaptability:** A system cannot adapt its "cognitive engine" (the LLM) in response to changing constraints, like a user's urgency or a strict budget.

## The Experiment: An Intelligent Cognitive Router

EAX Router is our experimental implementation of a **Cognitive Routing Layer**. It introduces a decision engine between an agent's intent and the LLM providers. The agent defines the *task and its constraints*, and the router intelligently selects the *best model* based on a given optimization priority: **cost, latency, or quality.**

### How It Works: A Conceptual Example

Imagine an agent defined by the `llmunix-spec`. At one step, it needs to summarize a document. Instead of calling an LLM directly, it calls the router.

```python
from eax_router import Router, Task

# 1. Initialize the experimental router with a strategy
router = Router(strategy="CostOptimized")

# 2. An agent defines a cognitive task based on its current state
#    (e.g., from its constraints.md file)
task = Task(
    prompt="Summarize this 500-word article into three bullet points.",
    task_type="summarization",
    priority="cost" # The agent needs to be efficient
)

# 3. The router makes an intelligent decision
model_choice = router.route(task)
print(f"Routing Decision: {model_choice.reason}")
# >>> Routing Decision: Selected 'claude-3-haiku' for its high quality-to-cost ratio on summarization tasks.

# 4. The agent executes the task using the optimal model
# response = model_choice.execute(task.prompt)
```

## Core Research Areas

### 1. The Open Model Fingerprint Standard (`fingerprint.json`)
A core part of our research is creating a standardized, open schema for describing LLM capabilities. This allows for objective, data-driven routing decisions.

```json
{
  "model_id": "claude-3-haiku-20240307",
  "provider": "anthropic",
  "cost_per_million_tokens": { "input": 0.25, "output": 1.25 },
  "avg_latency_ms_per_1k_tokens": 350,
  "context_window": 200000,
  "capabilities": {
    "summarization": { "quality_score": 8.5, "confidence": 0.92 },
    "code_generation": { "quality_score": 6.0, "confidence": 0.78 },
    "reasoning": { "quality_score": 7.2, "confidence": 0.85 }
  },
  "strengths": ["low_latency", "high_throughput"],
  "last_updated": "2024-06-25",
  "benchmarked_by": "EvolvingAgentsLabs"
}
```
**Help our research!** We need community contributions to benchmark models and expand our fingerprint database.

### 2. Pluggable Routing Strategies
We are experimenting with different algorithms for decision-making. The `Strategy` pattern allows anyone to invent new routing logic.
- **`CostOptimized`**: Finds the cheapest model that meets a quality threshold.
- **`LatencyOptimized`**: Finds the fastest model available.
- **`QualityOptimized`**: Finds the model with the highest quality score for the task, ignoring cost.
- **`Balanced`**: A default strategy that weighs all factors.

### 3. Provider-Agnostic Architecture
An agent should not be tied to a single provider. We are building a flexible architecture to support:
- **Major Providers:** OpenAI, Anthropic, Google, etc.
- **Open-Source Models:** Via wrappers for Ollama, Together AI, and other services.
- **Extensible Interface:** A simple base class for adding new providers.

## Architecture & Installation

*(This section can remain largely the same, detailing the file structure and installation steps. The context provided above gives it a stronger "why.")*

## Research Roadmap

Our goal is to build the cognitive engine for the next generation of adaptive agents.

### Phase 1: Foundation (Current)
- [x] Basic routing framework & provider abstraction.
- [x] Initial `fingerprint.json` standard definition.
- [ ] Develop a comprehensive, open-source benchmark suite for fingerprinting.
- [ ] Refine the initial set of routing strategies.

### Phase 2: Intelligence & Adaptation
- [ ] Research ML-based routing strategies that learn from outcomes.
- [ ] Develop a mechanism for the router to detect "context drift" (when a model's real-world performance no longer matches its fingerprint).
- [ ] Integrate with the [EAX Protocol (SAL-CP)](https://github.com/EvolvingAgentsLabs/sal-cp) to use an agent's cognitive state as a routing signal.

### Phase 3: Ecosystem
- [ ] Launch a community-run, versioned database for model fingerprints.
- [ ] Formalize integration patterns with major AI frameworks.
- [ ] Build tools for visualizing routing decisions and cost savings.

## How to Contribute

This is a community-driven research project. The most valuable contribution you can make is helping us build a rich, objective database of model fingerprints.

1.  **Clone the repo** and set up the environment.
2.  **Run the benchmark suite** against a model you have access to: `python -m eax_router.research.benchmark --model "your-model-id"`.
3.  **Submit a Pull Request** with the generated `fingerprint.json` file.

We also welcome contributions to new routing strategies, provider integrations, and improvements to the core framework. Please read our `CONTRIBUTING.md`.

## License

EAX Router is licensed under the Apache 2.0 License.

---

## Connect

**Research Contributors**: [Matias Molinas](https://github.com/matiasmolinas) • [Ismael Faro](https://github.com/ismaelfaro)

**Organization**: [EvolvingAgentsLabs](https://github.com/EvolvingAgentsLabs) • **Lab Site**: [evolvingagentslabs.github.io](https://evolvingagentslabs.github.io)

---
*EAX Router is a core component of the experimental agent architecture being developed at [Evolving Agents Labs](https://evolvingagentslabs.github.io). Our goal is to build the foundational tools for truly intelligent and adaptive AI systems.*
