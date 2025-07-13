# EAX Router 🧠⚡

**(Evolving Agents Experimental Router)**

The DNS and Load Balancer for distributed agent networks - routing tasks to specialized LLMunix instances.

<p align="center">
  <a href="https://github.com/EvolvingAgentsLabs/eax-router/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="License"></a>
  <a href="https://evolvingagentslabs.github.io/"><img src="https://img.shields.io/badge/labs-EvolvingAgentsLabs-brightgreen" alt="Labs"></a>
  <a href="#"><img src="https://img.shields.io/badge/status-alpha_experiment-orange.svg" alt="Status"></a>
</p>

> 🌐 **Part of [Evolving Agents Labs](https://evolvingagentslabs.github.io)** | 🔬 [View All Experiments](https://evolvingagentslabs.github.io#experiments) | 📖 [Project Details](https://evolvingagentslabs.github.io/experiments/eax-router.html)

---

## ⚠️ Experimental Research Project

**Important**: This is an experimental research prototype exploring intelligent LLM routing concepts. It should be treated as research material rather than a production-ready system. This project will remain permanently in alpha status as ongoing research.

---

## The Problem (Our Research Hypothesis)

In a world of specialized AI agents, we need intelligent orchestration:
- **Agent Discovery**: How do agents find each other in a distributed network?
- **Task Routing**: How do we match tasks to the most capable specialized agents?
- **Load Balancing**: How do we distribute work efficiently across agent instances?
- **Protocol Standards**: How do agents communicate their capabilities and availability?

EAX Router acts as the "Chief Logistics Officer" for networks of LLMunix instances, providing DNS-like discovery and intelligent task distribution.

## The Experiment: Agent Network Orchestration

EAX Router is a specialized LLMunix instance that orchestrates a network of other agent instances. It receives task requests and routes them to the most appropriate specialized worker agents based on their capabilities, availability, and performance metrics.

### How It Works: Agent-to-Agent Communication

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

### Execution Flow

1. **Task Reception**: Router receives a goal from a user or another agent
2. **Task Analysis**: Uses `evaluate_request` to classify the task type
3. **Agent Discovery**: Uses `list_available_workers` to find suitable agents
4. **Intelligent Routing**: Selects the best agent based on specialization and availability
5. **Task Forwarding**: Uses `forward_task` to delegate work via the message bus

## Key Research Features

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

## Research Roadmap

### Phase 1: Foundation (Current)
- [x] Basic LLMunix integration concept
- [x] Message bus architecture design
- [ ] Agent registry implementation
- [ ] Task classification system
- [ ] Basic routing algorithms

### Phase 2: Network Intelligence
- [ ] Dynamic agent discovery protocols
- [ ] Load balancing algorithms
- [ ] Failure detection and recovery
- [ ] Performance-based routing
- [ ] SAL-CP protocol integration

### Phase 3: Ecosystem Scale
- [ ] Multi-router federation
- [ ] Hierarchical routing networks
- [ ] Cross-network agent discovery
- [ ] Marketplace integration for economic routing
- [ ] Global agent capability index

## Performance Research

Early agent network routing results (simulated):

| Task Type | Specialized Agent | Generalist Agent | Routing Time | Success Rate |
|-----------|------------------|------------------|--------------|--------------|
| Haiku Writing | Haiku Specialist | GPT-4 Agent | 45ms | 98% |
| Code Review | Code Analyst | General Assistant | 62ms | 95% |
| Data Analysis | Stats Expert | Claude Agent | 38ms | 97% |
| Translation | Polyglot Agent | General Model | 51ms | 96% |

### Network Metrics
- **Agent Discovery Time**: ~20-50ms average
- **Message Routing Overhead**: <100ms per hop
- **Network Resilience**: 94% uptime with 3+ agents
- **Load Distribution**: 40% improvement vs single agent

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

## Community & Research

### Join Our Research
- **📧 Discussions**: [GitHub Discussions](https://github.com/EvolvingAgentsLabs/eax-router/discussions)
- **🐛 Issues**: Report bugs or suggest improvements
- **📊 Benchmarks**: Contribute model performance data
- **🔬 Experiments**: Share routing strategy research

### Citation
If you use EAX Router in your research, please cite:

```bibtex
@software{eax_router_2024,
  title={EAX Router: Experimental Intelligent LLM Routing Layer},
  author={Molinas, Matias and Faro, Ismael},
  year={2024},
  organization={Evolving Agents Labs},
  url={https://github.com/EvolvingAgentsLabs/eax-router}
}
```

## Contributing

We welcome contributions from researchers and developers:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/routing-strategy`)
3. **Commit** your changes (`git commit -m 'Add intelligent routing strategy'`)
4. **Push** to the branch (`git push origin feature/routing-strategy`)
5. **Open** a Pull Request

Please read our [Contributing Guidelines](CONTRIBUTING.md) for details.

## License

EAX Router is licensed under the Apache 2.0 License. See [LICENSE](LICENSE) for details.

---

## Connect

**Research Contributors**: [Matias Molinas](https://github.com/matiasmolinas) • [Ismael Faro](https://github.com/ismaelfaro)

**Organization**: [EvolvingAgentsLabs](https://github.com/EvolvingAgentsLabs) • **Lab Site**: [evolvingagentslabs.github.io](https://evolvingagentslabs.github.io)

---

*Part of the EAX Protocol Suite from [Evolving Agents Labs](https://evolvingagentslabs.github.io) - Building the future of intelligent agents through experimental research*