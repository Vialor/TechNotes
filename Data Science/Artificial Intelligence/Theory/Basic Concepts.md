# Four possible definitions:
Think Humanly: Cognitive Modeling Approach
Think Rationally: The Laws of Thought Approach
Act Humanly: Turing Test Approach
**Act Rationally: The Rational Agent Approach (more general than Think Rationally)
# Agent

> an entity that perceives its environment through **sensors** and acts upon its **environment** through **actuators**

**percept sequence**: complete history of **percepts**
**agent function**: maps a **percept sequence** to an **action**
**rational agent**: rational agents choose the **action** that is expected to maximize its **performance measure**, given the evidence provided by the **percept sequence** and whatever **built-in knowledge** the agent has.
# Task Environment
> **task environment**: PEAS = performance measure + environment+ actuators + sensors

**performance measure**: evaluation of any given sequence of **environment** states:
**completeness**: if there is an answer, the algorithm will find the answer and stop; otherwise report not found)
**cost optimality**: find the solution with the lowest path cost
cost optimality ⇒ completeness
**time & space complexity**
## Properties:
fully observable, partially observable, unobservable
single-agent, multi-agent: competitive & cooperative
episodic, sequential (will action affect future?)
deterministic, stochastic, nondeterministic (probability in environment)
static, semi-dynamic, dynamic (does thinking time matter?)
discrete, continuous (nature of env states)
# Agent Program

> **agent program**: implementation of the **agent function**

**simple reflex agent**: agent function acts only based the current percept using **condition-action rules**
percept ⇒ action
**model-based reflex agent**: maintain internal state to track aspects of the world that may not be evident in the current percept
**transition model**: description of how the next states depends on the current state and action
**sensor model**: description of how the current world state is reflected in the agent’s percepts
(state, action, percept, transition model, sensor model): current state ⇒ action
**goal based agent**: act to achieve their goals
(state, action, percept, transition model, sensor model): current state ⇒ expands next possible actions: is it goal? ⇒ action
**utility based agent**: maximize their own expected “happiness.”
(state, action, percept, transition model, sensor model): current state ⇒ expands next possible actions: utility ⇒ action