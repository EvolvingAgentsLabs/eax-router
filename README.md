# EAX Router 🧠⚡

**(Evolving Agents Experimental Router)**

The DNS and Load Balancer for distributed agent networks - routing tasks to specialized LLMunix instances.

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

In a world of specialized AI agents, we need intelligent orchestration:
- **Agent Discovery**: How do agents find each other in a distributed network?
- **Task Routing**: How do we match tasks to the most capable specialized agents?
- **Load Balancing**: How do we distribute work efficiently across agent instances?
- **Protocol Standards**: How do agents communicate their capabilities and availability?

Our research explores a fundamental question: **Can an agentic system dynamically select the optimal LLM for each discrete cognitive task, based on the task's unique requirements and constraints?**

This leads to several problems we are investigating:
- **High Costs:** Using frontier models for simple tasks is wasteful.
- **Suboptimal Performance:** Using cheap models for complex reasoning leads to failure.
- **Lack of Adaptability:** A system cannot adapt its "cognitive engine" (the LLM) in response to changing constraints, like a user's urgency or a strict budget.

## The Experiment: Intelligent Cognitive Router

EAX Router is our experimental implementation of a **Cognitive Routing Layer**. It introduces a decision engine between an agent's intent and the LLM providers. The agent defines the *task and its constraints*, and the router intelligently selects the *best model* based on a given optimization priority: **cost, latency, or quality.**

EAX Router acts as the "Chief Logistics Officer" for networks of LLMunix instances, providing DNS-like discovery and intelligent task distribution.

### How It Works: A Conceptual Example

Imagine an agent defined by the `llmunix-spec`. At one step, it needs to summarize a document. Instead of calling an LLM directly, it calls the router.

```yaml
# EAX Router's GEMINI.md firmware configuration
agent_role: "Chief Logistics Officer"
primary_goal: "Route tasks to specialized worker agents"

virtual_tools:
  - name: list_available_workers
    description: "Check registry of online LLMunix instances"
    
  - name: evaluate_request
    description: "Classify incoming task type and requirements"
    
  - name: forward_task
    description: "Send task to chosen worker agent via message bus"
```

```python
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

### Execution Flow

1. **Task Reception**: Router receives a goal from a user or another agent
2. **Task Analysis**: Uses `evaluate_request` to classify the task type
3. **Agent Discovery**: Uses `list_available_workers` to find suitable agents
4. **Intelligent Routing**: Selects the best agent based on specialization and availability
5. **Task Forwarding**: Uses `forward_task` to delegate work via the message bus

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

### 🔌 Agent Discovery Mechanisms
- **Registry-Based**: Agents register their capabilities in a shared directory
- **Broadcast Discovery**: Agents announce availability on the message bus
- **Capability Matching**: Routes tasks based on agent specializations
- **Health Monitoring**: Tracks agent availability and performance metrics

### 📊 Agent Capability Registry
Each LLMunix instance advertises its capabilities through a standardized format:

```json
{
  "agent_id": "haiku-writer-specialist",
  "instance_type": "llmunix",
  "base_model": "claude-3-haiku-20240307",
  "specialization": "creative_writing",
  "capabilities": {
    "haiku_composition": { "quality_score": 9.5, "confidence": 0.95 },
    "poetry_analysis": { "quality_score": 8.8, "confidence": 0.90 },
    "creative_constraints": { "quality_score": 9.2, "confidence": 0.93 }
  },
  "resource_requirements": {
    "avg_response_time_ms": 350,
    "memory_usage_mb": 512,
    "concurrent_capacity": 10
  },
  "status": "online",
  "message_endpoint": "workspace/messages/haiku-writer/",
  "last_heartbeat": "2024-06-25T10:30:00Z"
}
```

### 🌐 Multi-Agent Network Support
EAX Router enables communication between diverse agent types:
- **Specialist Agents**: Task-specific LLMunix instances (writers, coders, analysts)
- **Generalist Agents**: Broad-capability instances for varied tasks
- **Tool Agents**: Agents specialized in using specific external tools
- **Observer Agents**: Monitoring and logging instances (llmunix-canvas)
- **Gateway Agents**: Bridge instances connecting to external services

### 3. Provider-Agnostic Architecture
An agent should not be tied to a single provider. We are building a flexible architecture to support:
- **Major Providers:** OpenAI, Anthropic, Google, etc.
- **Open-Source Models:** Via wrappers for Ollama, Together AI, and other services.
- **Extensible Interface:** A simple base class for adding new providers.

### ⚡ Intelligent Routing Algorithms

```python
# Agent selection algorithm
def route_to_best_agent(task, available_agents):
    # Filter agents by specialization match
    suitable_agents = [
        agent for agent in available_agents
        if task.type in agent.capabilities
    ]
    
    # Score agents based on multiple factors
    for agent in suitable_agents:
        agent.routing_score = calculate_score(
            capability_match=agent.capabilities[task.type].quality_score,
            current_load=agent.concurrent_tasks / agent.capacity,
            response_time=agent.avg_response_time,
            specialization_bonus=1.5 if agent.specialization == task.type else 1.0
        )
    
    return max(suitable_agents, key=lambda a: a.routing_score)
```

## Research Architecture

### Core Components

```
eax_router/
├── GEMINI.md              # Router agent firmware configuration
├── core/
│   ├── router_agent.py    # Main router LLMunix instance
│   ├── task_classifier.py # Task type identification
│   └── agent_registry.py  # Available agents tracking
├── messaging/
│   ├── message_bus.py     # Inter-agent communication layer
│   ├── sal_cp_protocol.py # SAL-CP message formatting
│   └── broadcast.py       # Multi-agent broadcast support
├── discovery/
│   ├── agent_monitor.py   # Health check and heartbeat tracking
│   ├── capability_index.py # Agent capability database
│   └── registration.py    # New agent onboarding
├── routing/
│   ├── load_balancer.py   # Distribute tasks across agents
│   ├── specialization.py  # Match tasks to specialist agents
│   └── fallback.py        # Handle unavailable agents
└── integration/
    ├── llmunix_bridge.py  # Interface with LLMunix instances
    ├── marketplace_api.py # Connect to EAX Marketplace
    └── canvas_reporter.py # Send metrics to observer agents
```

## Experimental Installation & Setup

**Note**: EAX Router runs as a specialized LLMunix instance.

```bash
# Clone the router instance
git clone https://github.com/EvolvingAgentsLabs/eax-router.git
cd eax-router

# Copy LLMunix framework
cp -r ../llmunix/* ./

# Configure router firmware
cat > GEMINI.md << 'EOF'
# EAX Router - Chief Logistics Officer

You are the routing agent for a network of specialized LLMunix instances.

## Virtual Tools
- list_available_workers: Check agent registry
- evaluate_request: Classify task requirements  
- forward_task: Route to selected agent
- check_agent_health: Monitor agent status

## Primary Directive
Route incoming tasks to the most capable available agent based on:
1. Task type and requirements
2. Agent specializations and capabilities
3. Current load and availability
4. Performance history
EOF

# Launch router instance
gemini-cli --agentic-markdown GEMINI.md
```

### Setting Up Worker Agents

```bash
# Create a specialist agent
mkdir ../haiku-writer-agent
cd ../haiku-writer-agent

# Configure specialist firmware
cat > GEMINI.md << 'EOF'
# Haiku Writer Specialist

You are a creative writing agent specialized in haiku composition.

## Capabilities
- haiku_composition: 9.5/10
- poetry_analysis: 8.8/10
- creative_constraints: 9.2/10

## Tools
- write_haiku: Compose traditional 5-7-5 haiku
- analyze_structure: Evaluate haiku form
- suggest_improvements: Refine existing haiku
EOF

# Launch specialist
gemini-cli --agentic-markdown GEMINI.md
```

## Research Roadmap

### Phase 1: Foundation (Current)
- [x] Basic LLMunix integration concept
- [x] Message bus architecture design
- [x] Basic routing framework & provider abstraction
- [x] Initial `fingerprint.json` standard definition
- [ ] Agent registry implementation
- [ ] Task classification system
- [ ] Basic routing algorithms
- [ ] Develop a comprehensive, open-source benchmark suite for fingerprinting
- [ ] Refine the initial set of routing strategies

### Phase 2: Network Intelligence
- [ ] Dynamic agent discovery protocols
- [ ] Load balancing algorithms
- [ ] Failure detection and recovery
- [ ] Performance-based routing
- [ ] SAL-CP protocol integration
- [ ] Research ML-based routing strategies that learn from outcomes
- [ ] Develop a mechanism for the router to detect "context drift"
- [ ] Integrate with the [EAX Protocol (SAL-CP)](https://github.com/EvolvingAgentsLabs/sal-cp) to use an agent's cognitive state as a routing signal

### Phase 3: Ecosystem Scale
- [ ] Multi-router federation
- [ ] Hierarchical routing networks
- [ ] Cross-network agent discovery
- [ ] Marketplace integration for economic routing
- [ ] Global agent capability index
- [ ] Launch a community-run, versioned database for model fingerprints
- [ ] Formalize integration patterns with major AI frameworks
- [ ] Build tools for visualizing routing decisions and cost savings

## Research Contributions Welcome

### How to Contribute Model Fingerprints

```bash
# 1. Test a model with our benchmark suite
python -m eax_router.research.benchmark --model "your-model-id"

# 2. Generate fingerprint
python -m eax_router.research.fingerprint --model "your-model-id" --output fingerprints/

# 3. Submit via pull request
```

### Benchmark Categories
- **Summarization**: News articles, research papers, legal documents
- **Code Generation**: Python, JavaScript, SQL queries
- **Reasoning**: Logic puzzles, math problems, causal inference
- **Creative Writing**: Stories, poetry, marketing copy
- **Analysis**: Data interpretation, report generation
- **Translation**: Multi-language capabilities
- **Instruction Following**: Complex multi-step tasks

## Research Ethics & Considerations

### Responsible AI Research
- **Transparency**: All routing decisions are explainable
- **Fairness**: No bias toward specific providers
- **Privacy**: No user data stored in routing decisions
- **Efficiency**: Minimize computational overhead

### Known Limitations
- Model performance varies by domain and use case
- Fingerprints require regular updates as models evolve
- Quality scores are subjective and task-dependent
- Provider availability and pricing change frequently

## How to Contribute

This is a community-driven research project. The most valuable contribution you can make is helping us build a rich, objective database of model fingerprints.

1. **Fork** the repository
2. **Clone the repo** and set up the environment
3. **Run the benchmark suite** against a model you have access to: `python -m eax_router.research.benchmark --model "your-model-id"`
4. **Submit a Pull Request** with the generated `fingerprint.json` file

We also welcome contributions to new routing strategies, provider integrations, and improvements to the core framework. Please read our [Contributing Guidelines](CONTRIBUTING.md) for details.

## License

EAX Router is licensed under the Apache 2.0 License.

---

## Connect

**Research Contributors**: [Matias Molinas](https://github.com/matiasmolinas) • [Ismael Faro](https://github.com/ismaelfaro)

**Organization**: [EvolvingAgentsLabs](https://github.com/EvolvingAgentsLabs) • **Lab Site**: [evolvingagentslabs.github.io](https://evolvingagentslabs.github.io)

---
*EAX Router is a core component of the experimental agent architecture being developed at [Evolving Agents Labs](https://evolvingagentslabs.github.io). Our goal is to build the foundational tools for truly intelligent and adaptive AI systems.*