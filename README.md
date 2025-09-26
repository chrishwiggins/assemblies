# Assembly Model of Brain Computation

This repository contains Python implementations for simulating neural computation using the assembly model of the brain. This computational framework models how groups of neurons (assemblies) in different brain areas interact, form memories, and process information.

## Overview

The assembly model is based on the idea that cognition emerges from the dynamic formation and interaction of neural assemblies across different brain areas. This codebase provides tools to:

- Simulate neural areas with configurable parameters
- Model synaptic plasticity and learning
- Study memory formation and pattern completion
- Investigate language acquisition and syntax processing
- Analyze convergence properties and stability

## Getting Started

### Prerequisites

```bash
pip install numpy scipy matplotlib pptree
```

### Basic Usage

Start with a simple projection simulation:

```python
import brain
import simulations

# Run a basic projection simulation
results = simulations.project_sim(n=100000, k=1000, p=0.01, beta=0.05, t=50)
print("Assembly sizes over time:", results)
```

## Core Files and What They Do

### Core Framework

- **`brain.py`** - Main neural simulation engine
  - Defines `Brain` class for managing multiple brain areas
  - Implements `Area` class representing individual brain regions
  - Handles projection operations between areas
  - Manages synaptic plasticity and winner selection

- **`brain_util.py`** - Utility functions for brain operations
  - Helper functions for common brain computations
  - Statistical utilities for analyzing results

### Simulation Libraries

- **`simulations.py`** - Standard simulation experiments
  - `project_sim()` - Basic projection convergence experiments
  - `project_beta_sim()` - Studies effect of different plasticity parameters
  - `merge_sim()` - Assembly merging experiments
  - `pattern_completion_sim()` - Memory recall simulations
  - `association_sim()` - Cross-area association experiments

- **`overlap_sim.py`** - Specialized overlap analysis simulations
  - Studies overlap patterns between assemblies
  - Analyzes density properties in neural populations

- **`turing_sim.py`** - Turing machine-like computational experiments
  - Advanced simulations exploring computational capabilities
  - Studies larger assemblies with different parameters

### Language and Learning Models

- **`learner.py`** - Language acquisition framework
  - `LearnBrain` class for word/syntax learning experiments
  - Models phonological, lexical, and syntactic areas
  - Supports bilingual learning experiments
  - Example usage:
    ```python
    import learner
    brain = learner.LearnBrain(0.05, LEX_k=100)
    brain.train_simple(30)
    brain.test_verb("RUN")
    ```

- **`parser.py`** - Syntactic parsing simulations
  - Models sentence parsing using neural assemblies
  - Supports multiple languages (English, Russian)
  - Implements grammatical rules as neural projections

- **`recursive_parser.py`** - Advanced recursive parsing
  - Handles complex nested syntactic structures
  - Models hierarchical language processing

- **`word_order_int.py`** - Word order learning with intermediate areas
  - Studies how brains learn different word orders (SVO, SOV, etc.)
  - Models specialized temporal-parietal junction (TPJ) areas

### Testing and Examples

- **`project.py`** - Simple example simulation
  - Minimal working example of the assembly model
  - Good starting point for understanding the framework

- **`tests.py`** - Unit tests for core functionality
  - Run with: `python tests.py`

- **`simulations_test.py`** - Tests for simulation functions
  - Validates simulation outputs and parameters

## Key Concepts

### Brain Areas
Each area has:
- `n`: Total number of neurons
- `k`: Number of neurons that fire (winners)
- `beta`: Plasticity parameter controlling learning rate
- `p`: Sparsity/connectivity parameter

### Projections
- **Feedforward**: `brain.project({"stimulus": ["area_A"]}, {})`
- **Recurrent**: `brain.project({}, {"area_A": ["area_A"]})`
- **Cross-area**: `brain.project({}, {"area_A": ["area_B"], "area_B": ["area_A"]})`

### Assembly Formation
Repeated stimulation causes:
1. Winner selection (top-k neurons fire)
2. Synaptic strengthening between winners
3. Assembly stabilization over time
4. Pattern completion capabilities

## Example Experiments

### 1. Basic Assembly Formation
```python
import brain

b = brain.Brain(p=0.01)
b.add_stimulus("stim", k=100)
b.add_area("A", n=10000, k=100, beta=0.05)

# Repeated stimulation forms stable assembly
for i in range(20):
    b.project({"stim": ["A"]}, {"A": ["A"]})
    print(f"Round {i}: Assembly size = {b.areas['A'].w}")
```

### 2. Memory Association
```python
import simulations

# Study how two stimuli become associated
results = simulations.association_sim(
    n1=100000, n2=100000, k=1000, p=0.01, beta=0.05
)
```

### 3. Language Learning
```python
import learner

# Train brain to distinguish verbs from nouns
brain = learner.LearnBrain(beta=0.05, LEX_k=100)
brain.train_simple(30)
brain.test_word("CAT")  # Should activate noun areas
brain.test_verb("RUN")  # Should activate verb areas
```

## Running Experiments

Most simulation functions return data that can be analyzed:

```python
import simulations
import matplotlib.pyplot as plt

# Study convergence for different plasticity values
results = simulations.project_beta_sim(n=100000, k=317, p=0.01, t=100)

# Plot convergence curves
for beta, assembly_sizes in results.items():
    plt.plot(assembly_sizes, label=f'β={beta}')
plt.xlabel('Time Steps')
plt.ylabel('Assembly Size')
plt.legend()
plt.show()
```

## Advanced Features

- **Explicit vs. Sparse Simulation**: Choose between full neuron tracking or efficient sparse computation
- **Multi-area Networks**: Connect multiple brain regions with configurable connectivity
- **Custom Plasticity Rules**: Define area-specific learning parameters
- **Bilingual Modeling**: Study language interaction and interference
- **Syntax Processing**: Model grammatical rule acquisition and application

## Contributing

When adding new simulations:
1. Follow the naming convention: `*_sim.py` for simulation libraries
2. Include docstrings explaining parameters and expected outputs
3. Add tests to validate new functionality
4. Use the existing `Brain` and `Area` classes as building blocks

## References

This implementation is based on the assembly model of brain computation as described in computational neuroscience literature. The model captures key principles of neural plasticity, competition, and memory formation in simplified but biologically-inspired simulations.