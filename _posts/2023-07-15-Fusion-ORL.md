# Reinforcement Learning: Multi-Armed Bandit Problem with Unobserved Confounders
In many reinforcement learning tasks with complex environments, active learning agents make decisions according to their observation and receive feedback that depends on the quality of their choices. If data collected from observing other agents are available, then one may exploit these data for more efficient experimentation. 

However, data from different sources are often collected under heterogenous conditions, with unobserved confounders that influence the action an agent would take as well as the outcome of
this action. Averaging out these confounders possibly introduces bias to our estimation.

The goal of our agent is to quickly learn about its environment by consolidating data collected from observing other agents and data collected through its own experience. Three types of data may be employed by an autonomous agent to inform its decision-making:
+ **Observational data** is gathered through passive examination of the actions and rewards of agents other than the actor, but for whom the actor is assumed to be exchangeable.
+ **Experimental data** is gathered through randomization (e.g., standard MAB exploration), or from a fixed, non-reactive policy.
+ **Counterfactual data** (though traditionally computed from a fully specified model or under specific empirical conditions) represents the rewards associated with actions under a particular (or “personalized”) instantiation of the unobserved confounders.

## $K$-Armed Bandits with Unobserved Confounders
