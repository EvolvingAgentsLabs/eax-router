# EAX Router 🧠⚡

**(Evolving Agents Experimental Router)**

An experimental, intelligent, and pluggable routing layer for Large Language Models.

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

Developers today hardcode their applications to use a single LLM (e.g., `gpt-4o`), leading to:
- **High costs** from always using expensive models
- **Vendor lock-in** preventing exploration of alternatives
- **Lack of flexibility** in matching models to task requirements
- **Suboptimal performance** when simpler tasks use complex models

Our research explores whether a smart routing layer can allow developers to use the *right model for the right job* automatically.

## The Experiment: Intelligent, Dynamic Router

EAX Router introduces a decision layer between applications and LLMs. Developers define the *task*, and the router intelligently selects the *best model* based on optimization priorities: **cost, latency, or quality.**

### Experimental Usage Pattern

```python
from eax_router import Router, Task

# 1. Initialize the experimental router
router = Router()

# 2. Define your task with priority
task = Task(
    prompt="Summarize this 500-word article into three bullet points.",
    priority="cost",  # or "latency", "quality"
    context_length=2000,
    expected_output_length="short"
)

# 3. Intelligent routing decision
model_choice = router.route(task)
print(f"Selected: {model_choice.model_id} (reason: {model_choice.reason})")

# 4. Execute with chosen model
response = model_choice.execute(task.prompt)
# >>> Selected: claude-3-haiku-20240307 (reason: optimal cost for summarization task)
```

## Key Research Features

### 🔌 Pluggable Routing Strategies
- **CostOptimized**: Prioritizes cheapest model that meets quality thresholds
- **LatencyOptimized**: Selects fastest model for time-sensitive applications
- **QualityOptimized**: Chooses highest-performing model regardless of cost
- **CustomStrategy**: Interface for implementing domain-specific routing logic

### 📊 Open Model Fingerprinting Standard
Core research into standardized LLM capability description through `fingerprint.json`:

```json
{
  "model_id": "claude-3-haiku-20240307",
  "provider": "anthropic",
  "cost_per_million_tokens": { 
    "input": 0.25, 
    "output": 1.25 
  },
  "avg_latency_ms_per_1k_tokens": 350,
  "context_window": 200000,
  "capabilities": {
    "summarization": { "quality_score": 8.5, "confidence": 0.92 },
    "code_generation": { "quality_score": 6.0, "confidence": 0.78 },
    "reasoning": { "quality_score": 7.2, "confidence": 0.85 },
    "creative_writing": { "quality_score": 8.8, "confidence": 0.89 }
  },
  "limitations": [
    "struggles_with_math_beyond_basic_arithmetic",
    "may_hallucinate_specific_facts"
  ],
  "last_updated": "2024-06-25",
  "benchmarked_by": "EvolvingAgentsLabs"
}
```

### 🌐 Provider-Agnostic Architecture
Experimental support for multiple LLM providers:
- **OpenAI**: GPT-3.5, GPT-4, GPT-4o families
- **Anthropic**: Claude 3 families (Haiku, Sonnet, Opus)
- **Google**: Gemini Pro, Gemini Flash
- **Open Source**: Via Ollama, Together AI, Replicate
- **Custom Providers**: Extensible interface for new providers

### ⚡ Intelligent Optimization
Research into decision algorithms:

```python
# Cost optimization example
def cost_optimize_strategy(task, available_models):
    # Filter models that can handle task requirements
    suitable_models = filter_by_capability(available_models, task)
    
    # Calculate cost-effectiveness score
    for model in suitable_models:
        model.score = (
            model.quality_score * 0.7 +  # Quality weight
            (1 / model.cost_per_token) * 0.3  # Cost efficiency weight
        )
    
    return max(suitable_models, key=lambda m: m.score)
```

## Research Architecture

### Core Components

```
eax_router/
├── core/
│   ├── router.py           # Main routing engine
│   ├── task.py            # Task definition and analysis
│   └── strategy.py        # Routing strategy interfaces
├── providers/
│   ├── openai_provider.py  # OpenAI integration
│   ├── anthropic_provider.py # Anthropic integration
│   ├── google_provider.py  # Google integration
│   └── ollama_provider.py  # Local model support
├── fingerprints/
│   ├── claude_models.json  # Anthropic model capabilities
│   ├── gpt_models.json     # OpenAI model capabilities
│   └── gemini_models.json  # Google model capabilities
├── strategies/
│   ├── cost_optimized.py   # Cost-focused routing
│   ├── latency_optimized.py # Speed-focused routing
│   └── quality_optimized.py # Quality-focused routing
└── research/
    ├── benchmarks/         # Model performance studies
    ├── experiments/        # Routing strategy experiments
    └── analysis/          # Research findings and insights
```

## Experimental Installation & Setup

**Note**: This is research software. Use in controlled environments only.

```bash
# Clone the research repository
git clone https://github.com/EvolvingAgentsLabs/eax-router.git
cd eax-router

# Install in development mode
pip install -e .

# Set up provider API keys (for experimentation)
export OPENAI_API_KEY="your-key"
export ANTHROPIC_API_KEY="your-key"
export GOOGLE_API_KEY="your-key"
```

### Basic Experimental Usage

```python
from eax_router import Router
from eax_router.strategies import CostOptimized, QualityOptimized

# Initialize router with experimental strategy
router = Router(strategy=CostOptimized())

# Simple routing experiment
result = router.route_and_execute(
    prompt="Explain quantum computing in simple terms",
    priority="cost",
    max_tokens=200
)

print(f"Model used: {result.model_id}")
print(f"Cost: ${result.cost:.4f}")
print(f"Response: {result.text}")
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
- [x] Basic routing framework
- [x] Provider abstraction layer
- [x] Fingerprint standard definition
- [ ] Initial benchmark suite
- [ ] Cost/latency/quality strategies

### Phase 2: Intelligence
- [ ] Machine learning routing decisions
- [ ] Dynamic model discovery
- [ ] Usage pattern learning
- [ ] Automatic fingerprint updates

### Phase 3: Ecosystem
- [ ] Community fingerprint database
- [ ] Plugin architecture for custom strategies
- [ ] Integration with major AI frameworks
- [ ] Real-time model performance monitoring

## Performance Research

Early experimental results (preliminary):

| Task Type | Best Model (Cost) | Best Model (Quality) | Cost Savings | Quality Trade-off |
|-----------|------------------|---------------------|--------------|------------------|
| Summarization | Claude Haiku | Claude Opus | 85% | -12% |
| Code Gen | GPT-3.5 Turbo | GPT-4 | 90% | -25% |
| Translation | Gemini Flash | Claude Opus | 70% | -8% |
| Creative | Claude Haiku | GPT-4 | 80% | -18% |

*Results based on limited experimental data. Your results may vary.*

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