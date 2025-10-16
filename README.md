# relation-primitives

Core primitives for representing connections, dependencies, and causality across domains. This module provides the building blocks for expressing how entities influence, depend on, or relate to each other within the Spiral architecture.

## Purpose

- Provide a domain-agnostic vocabulary for relations (e.g., causal, dependency, equivalence, containment)
- Support composition of relations into graphs and higher-order structures
- Enable analysis and transformation of relational structures across physics, economics, governance, cognition, and more

## Structure

- Primitive types: Relation, CausalRelation, Dependency, Equivalence, Containment
- Composite structures: RelationSet, RelationGraph, MultilayerGraph
- Utilities: validators, traversal, reachability, influence measures, path constraints

## Quick Start

```python
from relation_primitives import Relation, CausalRelation, RelationGraph

# Basic relation between two entities
r = Relation(source="A", target="B", label="connected_to", properties={"weight": 0.7})

# Causal relation with direction and strength
c = CausalRelation(cause="X", effect="Y", strength=0.9, delay="2s")

# Build a graph
G = RelationGraph()
G.add(r)
G.add(c)

# Query and analyze
paths = G.paths("A", "Y", max_length=4)
influence = G.total_influence("X", "Y")
```

## Design Principles

- Relations are first-class primitives with introspectable structure
- All relations are composable and can be lifted to graphs
- Direction, weight, uncertainty, and constraints are explicit properties
- Invariants: acyclicity (optional), referential integrity, type-consistency

## Examples

1) Modeling a supply chain dependency graph
```python
from relation_primitives import Dependency, RelationGraph

G = RelationGraph()
G.add(Dependency(source="Supplier", target="Manufacturer", critical=True))
G.add(Dependency(source="Manufacturer", target="Distributor"))
G.add(Dependency(source="Distributor", target="Retailer"))

critical_paths = G.critical_paths("Supplier", "Retailer")
```

2) Cross-domain causal map (policy -> environment -> health)
```python
from relation_primitives import CausalRelation, RelationGraph

G = RelationGraph()
G.add(CausalRelation("CarbonTax", "Emissions", strength=-0.6))
G.add(CausalRelation("Emissions", "AirQuality", strength=-0.8))
G.add(CausalRelation("AirQuality", "PublicHealth", strength=0.7))

impact = G.cascade_effect("CarbonTax", "PublicHealth")
```

## Integration with Spiral Base

- Implements ModuleInterface: register transformations and validators
- Exposes primitive schemas for cross-module compatibility
- Plays well with unit-primitives (weights as units) and choice-primitives (decisions affecting relations)

```python
from spiral_base import ModuleInterface
from relation_primitives import validators, transforms

class RelationModule(ModuleInterface):
    required_primitives = []
    def register(self, system):
        system.add_validators(validators.all())
        system.add_transformations(transforms.all())
```

## Roadmap

- [ ] Formalize type system for relation kinds
- [ ] Probabilistic relations and confidence intervals
- [ ] Temporal relations (lags, memory, windows)
- [ ] Multilayer graphs and cross-layer coupling
- [ ] Efficient algorithms for large graphs
- [ ] Visualization helpers and exporters

## Contribution Guidelines

We welcome contributions from graph theory, network science, systems engineering, social sciences, and more.

- Open an issue to propose new relation types or algorithms
- Include benchmarks and tests for new graph operations
- Document mathematical assumptions and invariants
- Provide small, clear examples for each feature

## Code Style and Testing

```python
from relation_primitives import Relation, RelationGraph

def test_relation_addition():
    G = RelationGraph()
    G.add(Relation("a","b"))
    assert G.has_edge("a","b")

def test_influence_monotonicity():
    # Stronger edge should not reduce total influence along a fixed path
    ...
```

## Cross-Disciplinary Invitation

- Network scientists: measures and algorithms
- Economists: market relations and feedbacks
- Policy experts: governance and institutional relations
- Biologists: ecological and regulatory networks
- Mathematicians: categorical and algebraic perspectives on relations

## License

MIT License
