# About

## Overview

This repository contains a Python re-derivation and algorithmic implementation of the coalition model originally introduced by Noe (1994) to explain patterns of male reproductive coalition formation in primates, particularly baboons. Rather than aiming to exactly reproduce the figures or numerical results of the original paper, the goal of this project is to make the underlying logic of the model explicit, formalize the assumptions that are often left implicit in verbal or analytical treatments, and to translate the model into a transparent computational framework that can be extended beyond the original model's mechanisms.

Therefore, this project should be understood as both a re-derivation and an interpretation of Noe’s original model. The code is written with the intention that it can serve as a conceptual bridge between classic behavioral ecology theory and more modern computational or agent-based modeling approaches. Future extensions are planned and discussed in sections below which extend the model beyond Noe's initial conceptualization and analysis.

**I highly reccomend that you also read the `BACKGROUND_AND_DESIGN.md` document in this repository after viewing this file. It provides additional context that is key to understanding the model background and provides a formalization of Noe's original assumptions.**


## The Original Model (Noe 1994)

In Noe (1994), the central problem being addressed is why lower-ranking male primates sometimes form coalitions to challenge higher-ranking males for access to reproductive opportunities, even in cases where coalition partners may not share reproductive benefits equally. The model abstracts away many behavioral and ecological details in order to focus on a small number of structural constraints that determine whether coalitions are even feasible in the first place.

The key idea is that males are ranked by fighting ability, that this ranking determines who can plausibly challenge whom, and that coalition success depends on whether the combined fighting ability of two males exceeds that of a higher-ranking target male. Rather than modeling learning, reciprocity, or repeated interactions explicitly, Noe’s approach is primarily concerned with characterizing the *space of possible coalitions* available to males at different rank positions.


## Core Assumptions and Quantities

### Fighting Ability and Rank

Each male is assigned a scalar fighting ability \( F_i \), and individuals are ranked such that fighting ability decreases monotonically with rank:


F_1 >= F_2 >= ... F_N

Rank is treated as deterministic and fixed within a given evaluation of the model, meaning that the analysis asks what coalition opportunities exist *given* a particular ordering of fighting ability, rather than modeling how that ordering itself emerges.
The 'steepness' of the rank-fighting ability curve is a key parameter in the model, as the magnitude of difference between male fighting abilities influences which partner-target combinations are possible. Changing the steepness changes the model results, and any 
degree of steepness of this curve is permitted as long as all male fighting abilities are bounded such that:

0 <= F_i <= 100, for all males


### Coalition Feasibility

A coalition between two males \( i \) and \( j \) is considered capable of successfully challenging a target male \( k \) if the following condition holds:

F_ij > F_k
 
where:
* F_ij = c(F_i + F_j)
and: 
* c = a multiplicative 'synergy' factor(standard = 1, if set to c > 1 this indicates synergistic benefits to the coalition partners, while c < 1 is a detrimental synergy in cases where partners try to take advantage of one another or don't coordinate well)

Only males ranked higher than both coalition partners are considered valid targets, reflecting the assumption that coalitions are formed to challenge dominant individuals rather than lower-ranking ones. This simple inequality is the core constraint that generates the structure of coalition opportunities across the dominance hierarchy.


## Algorithmic Design Decisions

This implementation makes several modeling decisions explicit that are either implicit or only loosely specified in the original paper, largely because translating the model into code requires that these ambiguities be resolved in a precise way. To see the 
full formalization of these assumptions and the justifications I made in designing them, please refer to the `BACKGROUND_AND_DESIGN` document.

### Rank-Based Computation

Rather than operating on individuals as abstract agents, the model works directly on a rank-ordered array of fighting abilities. Coalition opportunities are therefore computed as a function of rank position, which makes it straightforward to ask questions such as how many coalition opportunities are available to a male of a given rank, or how these opportunities change as population size or the distribution of fighting ability changes.

This design choice also makes the relationship between rank structure and coalition feasibility more transparent than it would be in a purely agent-based implementation.


### Partner and Target Constraints

The main function allows for explicit control over several aspects of coalition formation logic, including whether coalitions are allowed to target only strictly higher-ranking males, and whether coalition attempts should be restricted to only the top-ranked individuals (for example, when modeling situations in which only the highest-ranking males hold consortships).

These options are not intended to represent different biological hypotheses so much as they are tools for probing the sensitivity of the model to assumptions that are often left unstated.


### Counting Opportunities Rather Than Outcomes

Importantly, this model counts *coalition opportunities* rather than realized coalition events. In other words, it characterizes which coalitions *could* succeed based on fighting ability constraints, not which coalitions actually form, persist, or result in reproductive success.

This reflects the original analytical goal of Noë (1994), which was to describe the structural conditions under which coalitions are possible, rather than to model behavioral dynamics such as partner choice, negotiation, or reciprocity. Those processes are intentionally left to future extensions of the model.


## Core Functionality

The primary analytical work in this repository is performed by the function:

```python
coalition_opportunities(F, top_targets=None, strict=True)
```
where `F` is a NumPy array of fighting abilities ordered from strongest to weakest. Optional arguments allow the user to restrict which ranks can be targeted by coalitions or to relax strict rank-ordering constraints.

The function returns coalition opportunity counts by rank, which can be directly compared to the qualitative predictions and figures presented in Noë (1994). 


## How to use this Program

To use this mode, the user needs to specify a rank-ordered array of fighting abilities. For example:

```python
F = np.array([10.0, 9.2, 8.7, 7.5, 6.1])
```
Coalition opportunities for each pairing of coalitionary males can the be computed by calling:

```python
coalition_opportunities(F)
```
Optional parameters can be included to restrict target ranks (e.g. only top 3 ranking males can be targeted) or modify coalition constraints based on the analysis interests of the user.

The code design is intended to be modular, allowing for the embedding of this model into larger simulations and parameter sweeps. The intention is to also ustilize this as a foundational program which allows for further model extensions.

Once the `coalition_opportunity` function has been run with the users' specified parameter values, the `make_model_visuals` and `plot_sigmoid_function` functions are used to plot and visualize the rank-fighting ability curves and distribution of coalitionary male
partner-target combinations possible within the model's state space. I reccomend using the `plot_sigmoid_function` as this captures the non-linear nature of the decline in male fightin abilities between ranks more accuratley than the `make_model_visuals` function. 


## Current Scope and Future Extensions

The repository as it currently exists meets the goal of re-deriving Noe's original mathematical model and implementing an algorithm to do the mathematical calculations for us. Apart from the design of the algorithm and visualization functions, none of the core
intellectual contributions are extended beyond Noe's initial model. With that being said, I am planning on implementing novel model extensions which add additional mechanism components and adjust the model assumptions to capture biologically informed dynamics of the baboon system which have been abstracted away or ignored in Noe's original model. Planned extensions include:

* Asymmetrical Payoffs within Pairs of Coalition Partners
* Probabilistic, rather than Deterministic, Coalition Success
* Coalition Participation Costs
* Multi-Male Coalitions

## Citation

If you use or adapt this code, be sure to cite Noe's original work:

Noë, R. (1994). A model of coalition formation among male baboons. Behavioral Ecology and Sociobiology.
