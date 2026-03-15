# TOPOLOGY — Evolutionary Agent Organization Discovery

> "Intelligence lives in the topology, not the nodes."

Every agent framework makes you design the team structure upfront. We built a system that **discovers** it.

## What It Does

10 teams of LLM agents compete on a task. Each team has a randomly generated **organizational genome** — hierarchy, communication pattern, decision-making style, work distribution, roles, and team size. The weakest teams dissolve. The strongest mutate and propagate. Over 5 generations, evolution finds the optimal agent architecture that no human would have designed.

**Every gene has mechanical consequences:**

| Gene | What It Controls |
|------|-----------------|
| **Hierarchy** | Execution flow — flat teams run parallel, hierarchical teams run leader-first |
| **Communication** | Context visibility — broadcast (all see all), chain (sequential), hub (siloed), free-form (random subset) |
| **Decision-Making** | Output merging — leader synthesis, consensus concatenation, or autonomous single-best |
| **Work Distribution** | Task scoping — equal, specialized by role, or dynamic self-selection |
| **Roles** | System prompt personality — task-specific, generated dynamically per run |
| **Team Size** | 3-7 agents per team — structural tradeoff between coverage and coordination cost |

This isn't a visualization of evolution. It's a real evolutionary system where organizational structure directly determines agent behavior and output quality.

## The Demo

A real-time canvas shows all 10 teams as topology diagrams. You can **see** the organizational structure:

- **Hub-spoke** teams show a magenta center with cyan spokes radiating outward
- **Flat** teams show a circle of equal cyan nodes
- **Hierarchical** teams show a pyramid — leader on top, workers below
- **Chain** communication teams light up sequentially — each agent waits for the previous one's output

When teams die, they fade to gray. When new teams spawn from survivors, they bloom outward from their parent's position. The fitness sparkline tracks convergence in real time.

After 5 generations, DeepSeek-R1 synthesizes the evolutionary trajectory — naming the winning archetype, explaining what lost and why, and giving concrete organizational recommendations for the task.

## Architecture

**Three Nebius model tiers:**
- **Llama 3.1 8B-fast** — Agent execution (50+ calls per generation)
- **Llama 3.3 70B-fast** — Evaluation (harsh 4-dimension rubric) + per-generation insight
- **DeepSeek-R1** — Final synthesis (names the winning archetype)

**Two files:**
- `server.py` — Evolution engine, WebSocket server, all agent orchestration
- `index.html` — Canvas2D godmode visualization, sidebar panels, action feed

**Key design decisions:**
- Roles are generated dynamically per task (one 8B call at initialization)
- Evaluation penalizes redundancy/bloat (efficiency dimension), preventing verbosity bias from corrupting selection
- Chain communication forces sequential execution — slower but potentially more coherent. Evolution discovers whether the tradeoff is worth it
- Recording mode captures every broadcast message for demo playback
- Three-layer fallback: infrastructure auto-switch (3s), pre-stage test, manual hotkey (D)

## Evaluation Rubric

The 70B evaluator uses harsh grading (most teams score 2-3 out of 5):

1. **Completeness** — Did they solve the core task?
2. **Coherence** — Unified team output or disjointed mess?
3. **Depth** — Profound strategy or surface-level?
4. **Efficiency** — Laser-focused or redundant bloat? (The bloat penalty)

Fitness = average of all four dimensions, normalized to 0-1.

## Run It

```bash
pip install -r requirements.txt
export NEBIUS_API_KEY=your_key_here
python server.py
# Open index.html in a browser
```

A full 5-generation evolution run takes under 60 seconds.

## The Thesis

Every competitor in the agent framework space — LangChain, CrewAI, AutoGen — is scaffolding. They provide hammers and nails but assume the human engineer already knows what the building should look like.

Topology assumes the human engineer is wrong. We don't build the house. We run a thousand evolutionary simulations to discover the optimal blueprint, and then we hand it to them.

**Stop designing. Start discovering.**

## Built At

[Nebius.Build Hackathon](https://nebius.builders) — March 15, 2026, SHACK15 Ferry Building, San Francisco.

Built by [Nigel Robinson](https://github.com/nxrobins).
