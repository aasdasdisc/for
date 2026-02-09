# Web API Test Platform for Traffic Simulation & CityFlow Research

## Overview

This repository presents an **enterprise-grade Web API testing and experimentation platform**, specifically **tailored for traffic simulation, intelligent transportation systems (ITS), and CityFlow-based research**. It is designed to support **academic research, thesis work, and large-scale experimental evaluation** of traffic control algorithms, data-driven mobility models, and simulation-backed APIs.

The platform integrates:
- **Node.js (TypeScript)** for API orchestration, experiment control, and data services
- **Go** for high-performance load testing, traffic data simulation, and metrics aggregation
- **CityFlow / microscopic traffic simulators** as external or integrated backends

This makes the system suitable for **traffic flow modeling, signal control evaluation, RL-based traffic optimization, and scalability testing of smart-city APIs**.

---

## Research Objectives

This platform is designed to address the following research needs:

- Evaluate **traffic signal control strategies** under realistic API workloads
- Benchmark **reinforcement learning (RL) and rule-based controllers**
- Simulate **high-frequency vehicle and sensor data streams**
- Measure **latency, throughput, and stability** of traffic-management APIs
- Support **reproducible experiments** for academic publication

Typical use cases include:
- Pure intersection traffic flow experiments
- City-scale simulation using CityFlow
- API stress testing under peak traffic demand
- Comparative analysis of control algorithms

---

## System Architecture

```
┌──────────────┐     REST / gRPC     ┌──────────────────┐
│  Go Load     │ ─────────────────▶ │  Node.js API     │
│  Generators  │                    │  Gateway         │
└──────────────┘                    └─────────┬────────┘
        ▲                                      │
        │ Metrics / Events                     │
        │                                      ▼
┌──────────────┐                    ┌──────────────────┐
│ CityFlow /   │ ◀───────────────── │ Simulation       │
│ Traffic Sim  │   Control Actions  │ Services         │
└──────────────┘                    └──────────────────┘
        │
        ▼
┌──────────────────┐
│ Data Storage &   │
│ Experiment Logs  │
└──────────────────┘
```

---

## Project Structure (Research-Oriented)

### `src/api`
- Experiment control endpoints
- Traffic state ingestion APIs
- Control action dispatch (signals, phases, timings)

### `src/domain`
- Traffic entities (Vehicle, Lane, Intersection)
- Flow, density, queue-length models
- Experiment and scenario definitions

### `src/infrastructure/database`
- Multi-DB support for experiment data
- Time-series and relational storage
- Simulation result persistence

### `go/`
- High-throughput traffic event generators
- Load testing for smart traffic APIs
- Performance and stability monitoring tools

### `tests/`
- Deterministic unit tests for models
- Integration tests with CityFlow
- End-to-end experiment validation

---

## CityFlow Integration

This platform is designed to integrate seamlessly with **CityFlow** and similar microscopic simulators.

Supported interaction patterns:
- Step-based simulation control
- Real-time state polling
- Batch experiment execution
- Reinforcement learning loop integration

Typical workflow:
1. CityFlow simulates vehicle movement
2. Traffic state is exposed via API
3. Control logic (rule-based or RL) computes actions
4. Actions are applied back to the simulator
5. Metrics are logged for analysis

---

## Experiment Workflow

1. Define scenario and traffic demand
2. Configure simulator and API endpoints
3. Run Go-based load and data simulators
4. Collect metrics (queue, delay, throughput)
5. Export results for statistical analysis

---

## Metrics & Evaluation

The platform supports quantitative evaluation of:
- Average vehicle delay
- Queue length distribution
- Throughput per intersection
- API response latency
- System stability under load

Results can be exported for:
- Python / MATLAB analysis
- LaTeX tables and plots
- Research paper figures

---

## Experiment Results & Analysis

This platform is designed to produce **publication-ready experimental results**. Results are logged in structured formats (CSV / JSON / DB) and can be directly consumed by **Python, MATLAB, or R** for statistical analysis.

### Key Result Tables (Example)

| Metric                         | Baseline Control | RL Controller | Improvement |
|--------------------------------|------------------|---------------|-------------|
| Avg Vehicle Delay (s)          | 42.6             | 28.9          | ↓ 32.1%     |
| Avg Queue Length (vehicles)    | 17.4             | 10.2          | ↓ 41.3%     |
| Throughput (veh/hour)          | 1820             | 2150          | ↑ 18.1%     |
| API Latency (ms, p95)          | 96               | 104           | +8.3%       |
| System Stability (crash rate)  | Stable           | Stable        | —           |

> These metrics are typically evaluated per intersection and aggregated across simulation runs.

### Supported Plots

- Vehicle delay vs. time
- Queue length distribution
- Throughput comparison
- API latency under load
- Reward convergence (RL)

Plots can be exported directly into **LaTeX-compatible figures** for thesis or paper inclusion.

---

## Reinforcement Learning Control Loop

The platform natively supports **closed-loop reinforcement learning (RL)** for traffic signal control, enabling direct comparison with fixed-time and actuated controllers.

### Conceptual RL Loop

```
┌─────────────┐
│  Simulator  │  (CityFlow)
└──────┬──────┘
       │ State: queue, speed, density
       ▼
┌─────────────┐
│  RL Agent   │  (DQN / PPO / A2C)
└──────┬──────┘
       │ Action: signal phase, timing
       ▼
┌─────────────┐
│ Traffic API │  (Node.js Gateway)
└──────┬──────┘
       │ Apply control
       ▼
┌─────────────┐
│  Simulator  │
└──────┬──────┘
       │ Reward: delay, throughput
       └───────────────┐
                       ▼
                  Policy Update
```

### State Representation

- Queue length per lane
- Average vehicle speed
- Lane occupancy / density
- Signal phase encoding

### Action Space

- Phase selection
- Phase duration adjustment
- Adaptive green splitting

### Reward Design (Examples)

- Negative average delay
- Negative queue length
- Throughput maximization
- Penalized phase switching

---

## Control Strategy Comparison

This platform enables **systematic comparison** of three major traffic signal control paradigms under identical traffic demand and simulation conditions.

### 1. Fixed-Time Control

**Description:**
- Predefined signal phases and cycle lengths
- No adaptation to real-time traffic conditions

**Characteristics:**
- Deterministic and simple
- Low computational cost
- Poor performance under fluctuating demand

**Use Case:**
- Baseline comparison
- Traditional urban traffic systems

---

### 2. Actuated Control

**Description:**
- Signal timings adapt based on detector input
- Uses threshold-based logic (e.g., queue length, occupancy)

**Characteristics:**
- Moderate adaptability
- No long-term optimization
- Sensitive to detector placement

**Use Case:**
- Intermediate benchmark
- Real-world adaptive systems

---

### 3. Reinforcement Learning Control

**Description:**
- Learns optimal signal policies via interaction
- Optimizes long-term traffic performance

**Characteristics:**
- High adaptability
- Requires training and tuning
- Scales well with complex intersections

**Use Case:**
- Advanced ITS research
- Smart city traffic optimization

---

### Quantitative Comparison (Example)

| Control Strategy | Avg Delay (s) | Avg Queue | Throughput (veh/h) | Adaptability |
|------------------|---------------|-----------|---------------------|--------------|
| Fixed-Time       | 42.6          | 17.4      | 1820                | None         |
| Actuated         | 34.1          | 13.6      | 1985                | Medium       |
| RL-Based         | 28.9          | 10.2      | 2150                | High         |

---


## Reproducibility & Research Quality

- Versioned experiment configurations
- Deterministic simulation seeds
- Containerized environments
- CI-backed experiment validation

This ensures results are **repeatable and publishable**.

---

## Deployment Modes

- Local research environment
- Docker-based experiment runner
- CI-driven automated benchmarking
- Cloud-based large-scale simulation

---

## Test API Interaction Flow (With Error Handling)

The Test API operates as a **robust middleware layer** between the traffic simulator (CityFlow) and control logic. In addition to normal operation, it explicitly handles **failure cases, invalid actions, simulator lag, and communication errors**, which are critical for realistic ITS research.

---

## Explicit Mapping to CityFlow API Calls

The Test API wraps CityFlow core functions with validation, logging, and fault tolerance.

### CityFlow ↔ Test API Mapping

| Test API Responsibility | CityFlow API Call | Description |
|------------------------|------------------|-------------|
| Initialize simulation | `Engine(config.json)` | Load road network & flows |
| Advance simulation | `engine.next_step()` | Move simulation forward |
| Get lane vehicles | `engine.get_lane_vehicles()` | Vehicle IDs per lane |
| Get vehicle speeds | `engine.get_vehicle_speed()` | Speed per vehicle |
| Queue estimation | `engine.get_lane_waiting_vehicle_count()` | Waiting vehicles |
| Read signal phase | `engine.get_traffic_light_phase()` | Current phase |
| Apply control | `engine.set_traffic_light_phase()` | Change signal |

---

## Normal Operation Sequence (Mapped)

1. Query state from CityFlow APIs
2. Normalize raw traffic data
3. Validate and apply control action
4. Advance simulation step

---

## Error Handling & Failure Scenarios

### Invalid Control Action
- Rejected with HTTP 400
- Simulator state preserved

### Late or Missing Decision
- Fallback to safe policy
- Logged as delay event

### Simulator Desynchronization
- Step-level locking enforced
- Requests queued or dropped

### CityFlow Runtime Failure
- Experiment marked failed
- Logs preserved

### High API Load
- Rate limiting activated
- HTTP 429 returned

---

## Failure Handling in RL Context

| Failure | RL Treatment |
|-------|-------------|
| Invalid action | Negative reward |
| Late decision | Step truncation |
| Simulator crash | Episode termination |
| API overload | Observation noise |

---

## Summary (Research Perspective)

Explicit mapping to CityFlow and structured failure handling ensures **safe, reproducible, and realistic traffic control experiments**.

---

## Intended Audience

- Traffic & transportation researchers
- CSE students working on simulation & modeling
- Smart city & ITS developers
- Reinforcement learning researchers

---

## Citation (Suggested)


If you use this platform in academic work:

```
@software{traffic_api_platform,
  title  = {Web API Test Platform for Traffic Simulation},
  author = {Ahmed M. S. Albreem},
  year   = {2026}
}
```

---

## License

MIT License

