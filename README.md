# Planning Engine (PlanoGraph)

> **Status:** Future Project / Research & Design

## Vision

Planning Engine is a Python library for building rule-driven scheduling
engines.

Instead of implementing scheduling logic directly in application code,
users describe:

-   Activities (services/tasks)
-   Duration rules
-   Dependency rules
-   Execution context

The engine interprets these definitions, builds a directed graph,
resolves all applicable rules and generates a schedule using classical
graph algorithms such as Topological Sort and Critical Path Method
(CPM).

The goal is to separate **planning logic** from **business-specific
data**, allowing the same engine to be reused across multiple domains.

------------------------------------------------------------------------

# Motivation

The project originated from a construction planning engine where
schedules are generated from configurable rules.

The objective is to extract the generic scheduling concepts into a
reusable library instead of keeping them embedded in a single
application.

Potential domains include:

-   Construction
-   Manufacturing
-   Logistics
-   Infrastructure deployment
-   Maintenance planning
-   Business workflows

------------------------------------------------------------------------

# Core Concepts

## Activity

Represents a node in the scheduling graph.

Example attributes:

-   id
-   name
-   metadata

------------------------------------------------------------------------

## DurationRule

Determines how long an activity takes under a specific context.

Examples:

-   Building type
-   Construction method
-   Product family
-   Team size
-   User-defined conditions

------------------------------------------------------------------------

## DependencyRule

Represents an edge between two activities.

Contains:

-   predecessor
-   successor
-   dependency type (FS, SS, FF, SF)
-   lag
-   conditions
-   optional policies

------------------------------------------------------------------------

## PlanningContext

Runtime information used to evaluate rules.

Examples:

-   building_type
-   floors
-   method
-   region

The engine itself remains domain-agnostic.

------------------------------------------------------------------------

## PlanningEngine

High-level API responsible for:

1.  Loading definitions
2.  Resolving duration rules
3.  Resolving dependency rules
4.  Building the graph
5.  Running scheduling algorithms
6.  Returning the schedule

------------------------------------------------------------------------

# Proposed Architecture

``` text
                User Definitions
          (JSON / YAML / Excel / Database)

                     │
                     ▼

              Planning Engine

         ┌────────────────────┐
         │ Rule Resolver      │
         │ Graph Builder      │
         │ Scheduler          │
         └────────────────────┘

                     │
                     ▼

             Graph Backend
          (NetworkX initially)

                     │
                     ▼

              Schedule Result
```

------------------------------------------------------------------------

# Initial Modules

``` text
planning_engine/

    domain/
        activity.py
        duration_rule.py
        dependency_rule.py
        schedule.py

    engine/
        planning_engine.py

    graph/
        builder.py

    scheduler/
        cpm.py
        topological.py

    providers/
        excel.py
        json.py
        yaml.py

    adapters/
        networkx.py

    exceptions/

    tests/

    examples/
```

------------------------------------------------------------------------

# External Libraries

The project should leverage mature open-source libraries instead of
reimplementing them.

Initial candidates:

-   NetworkX (graph representation)
-   Pydantic (models)
-   Pandas (optional data loading)
-   OpenPyXL (Excel provider)

The value of the project is **not** implementing graph algorithms from
scratch, but providing a declarative scheduling engine built on top of
them.

------------------------------------------------------------------------

# Public API (Draft)

``` python
engine = PlanningEngine()

engine.load(...)

schedule = engine.build(context)

print(schedule.total_duration)
print(schedule.critical_path)
```

------------------------------------------------------------------------

# Future Features

-   Multiple providers (Excel, YAML, JSON, SQL)
-   Plugin system
-   Backend abstraction for graph libraries
-   Custom rule evaluators
-   Validation engine
-   Visualization helpers
-   Graph export
-   Schedule export
-   Resource constraints
-   Calendar support
-   Monte Carlo simulations
-   Critical Chain scheduling

------------------------------------------------------------------------

# Development Roadmap

## Phase 1 --- Core

-   Domain model
-   Rule evaluation
-   Graph builder
-   CPM
-   Topological sort
-   Basic API

## Phase 2 --- Providers

-   Excel provider
-   JSON provider
-   YAML provider
-   Validation

## Phase 3 --- Extensibility

-   Plugin architecture
-   Backend abstraction
-   Events/hooks

## Phase 4 --- Ecosystem

-   Documentation
-   Examples
-   Benchmarks
-   PyPI publication

------------------------------------------------------------------------

# Guiding Principles

-   Domain-driven design
-   Declarative configuration
-   Extensibility
-   Composition over reinvention
-   Clean Architecture
-   High test coverage
-   Stable public API

------------------------------------------------------------------------

# Long-Term Goal

Create a reusable Python library that allows developers to describe
planning problems declaratively while delegating graph construction,
rule evaluation and schedule generation to a generic engine.

The library should serve as the foundation for multiple planning
applications rather than being tied to a single business domain.
