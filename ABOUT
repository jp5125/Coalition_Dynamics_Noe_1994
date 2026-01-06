# About

## Overview

This repository contains a Python re-derivation and algorithmic implementation of the coalition model originally introduced by Noë (1994) to explain patterns of male reproductive coalition formation in primates, particularly baboons. Rather than aiming to exactly reproduce the figures or numerical results of the original paper, the goal of this project is to make the underlying logic of the model explicit, to formalize the assumptions that are often left implicit in verbal or analytical treatments, and to translate the model into a transparent computational framework that can be inspected, modified, and extended.

In that sense, this project should be understood as both a re-derivation and an interpretation of Noë’s original model, with an emphasis on clarity, algorithmic structure, and reproducibility. The code is written with the intention that it can serve as a conceptual bridge between classic behavioral ecology theory and more modern computational or agent-based modeling approaches.

---

## The Original Model (Noë 1994)

In Noë (1994), the central problem being addressed is why lower-ranking male primates sometimes form coalitions to challenge higher-ranking males for access to reproductive opportunities, even in cases where coalition partners may not share reproductive benefits equally. The model abstracts away many behavioral and ecological details in order to focus on a small number of structural constraints that determine whether coalitions are even feasible in the first place.

The key idea is that males are ranked by fighting ability, that this ranking determines who can plausibly challenge whom, and that coalition success depends on whether the combined fighting ability of two males exceeds that of a higher-ranking target male. Rather than modeling learning, reciprocity, or repeated interactions explicitly, Noë’s approach is primarily concerned with characterizing the *space of possible coalitions* available to males at different rank positions.

---

## Core Assumptions and Quantities

### Fighting Ability and Rank

Each male is assigned a scalar fighting ability \( F_i \), and individuals are ranked such that fighting ability decreases monotonically with rank:

\[
F_1 \ge F_2 \ge \dots \ge F_N
\]

Rank is treated as deterministic and fixed within a given evaluation of the model, meaning that the analysis asks what coalition opportunities exist *given* a particular ordering of fighting ability, rather than modeling how that ordering itself emerges.

---

### Coalition Feasibility

A coalition between two males \( i \) and \( j \) is considered capable of successfully challenging a target male \( k \) if the following condition holds:

\[
F_i + F_j > F_k
\]

Only males ranked higher than both coalition partners are considered valid targets, reflecting the assumption that coalitions are formed to challenge dominant individuals rather than lower-ranking ones. This simple inequality is the core constraint that generates the structure of coalition opportunities across the dominance hierarchy.

---

## Algorithmic Design Decisions

This implementation makes several modeling decisions explicit that are either implicit or only loosely specified in the original paper, largely because translating the model into code requires that these ambiguities be resolved in a precise way.

### Rank-Based Computation

Rather than operating on individuals as abstract agents, the model works directly on a rank-ordered array of fighting abilities. Coalition opportunities are therefore computed as a function of rank position, which makes it straightforward to ask questions such as how many coalition opportunities are available to a male of a given rank, or how these opportunities change as population size or the distribution of fighting ability changes.

This design choice also makes the relationship between rank structure and coalition feasibility more transparent than it would be in a purely agent-based implementation.

---

### Partner and Target Constraints

The main function allows for explicit control over several aspects of coalition formation logic, including whether coalitions are allowed to target only strictly higher-ranking males, and whether coalition attempts should be restricted to only the top-ranked individuals (for example, when modeling situations in which only the highest-ranking males hold consortships).

These options are not intended to represent different biological hypotheses so much as they are tools for probing the sensitivity of the model to assumptions that are often left unstated.

---

### Counting Opportunities Rather Than Outcomes

Importantly, this model counts *coalition opportunities* rather than realized coalition events. In other words, it characterizes which coalitions *could* succeed based on fighting ability constraints, not which coalitions actually form, persist, or result in reproductive success.

This reflects the original analytical goal of Noë (1994), which was to describe the structural conditions under which coalitions are possible, rather than to model behavioral dynamics such as partner choice, negotiation, or reciprocity. Those processes are intentionally left to future extensions of the model.

---

## Core Functionality

The primary analytical work in this repository is performed by the function:

```python
coalition_opportunities(F, top_targets=None, strict=True)
