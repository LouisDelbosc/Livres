##Chapter 5
##Monte Carlo Methods

The characteristics of *Monte Carlo Methods* is :
- We do not assume complete knowledge of the environment
  - in fact, we only need experience (sample data)
  - even if we have no prior knowledge of the environment dynamics, we can still attain optimal behavior
- A model is needed
  - but only the generate sample transitions are mandatory
  - not the complete probability distributions of all possible transition
    - which is require in DP
- It is a way of solving RL problème based on averaging sample returns
- We assume we are on episodic tasks
  - it means experience is divided into episodes

To make a parallel to the work done previously, monte-carlo methods behave like the bandit 
methods but each state is now a bandit problem in which each bandit problems are 
interrelated.

Monte Carlo Methods use methods we saw in previous chapter (policy evaluation ...)

###5.1 Monte Carlo Prediction

*value function* : expected return (cumulative future reward) starting from that state.

The idea underlies MC methods is to estimate a value function by experience by averaging 
the returns observed after visits to that state.

Suppose we wish to estimate $$v_{\pi}(s)$$, the value of a state $$s$$ under policy $$\pi$$.
- *visit* is an occurence of state $$s$$ in an episode
- $$s$$ may be visited multiple times in the same episode
  - *first visite* to $$s$$ is the first time it is visited in an episode
- The *first-vist MC method*
  - it estimates $$v_{\pi}$$ as the average of the returns following first visits to $$s$$
- The *every-visit MC method*
  - is the average of the returns following all visits to $$s$$
- Both methods converge to $$v_{\pi}$$ as the number of visits (or first visits) to $$s$$ goes to infinity

![figure 5.1](images/figure5_1.png)

For DP methods, you must have complete knowledge of the environment and sometime it is hard
to find some quantities needed for computation (like $$p(s',r|s,a)$$).
In some game (like black jack for exemple), it is really hard to determine thoses quantities.
MC methods does not have this issue because we are working with sample.
Generating the sample you will work with is often easier than to determine the one needed for
DP.

Backup diagrams are pertinent and we can generalize the idea to MC algorithms.
- backup diagram idea is
  - to show at the top *the root node* to be updated
  - to show below all the transitions and leaf nodes whose rewards and estimated values contribute to the update
- In MC estimation of $$v_{\pi}$$
  - the root is a state node
  - below is the entire trajectory of transitions along a particular single episode
    - ending at the terminal state

- Difference between DP diagram (Figure 3.4) and MC diagram (Figure 5.3)
  - DP diagram shows all possible transitions
  - DP diagram shows only one-step transitions
  - MC diagram shows only those sampled on the one episode
  - MC diagram goes all the way to the end of the episode

![figure 5.3](images/figure5_3.png)

- each estimation for each state are independant in MC methods
- MC methods do not *bootstrap*
- the computational expense of estimating the value of a single state is independent of the number of states
  - this can be attractive when one requires the value of only one subset of states
  - one can generate many sample episodes starting from the states of interest
    - averaging returns from only tese states ignoring all others

###5.2 Monte Carlo Estimation of Action Values

If a model is not available
- value functions is not enough to determine a policy
- we have to estimate *action values*

Let's consider the policy evaluation problem for action values.
- $$q_{\pi}(s,a)$$ is the exepected return when
  - starting in state $$s$$
  - taking action $$a$$
  - following policy $$\pi$$
- MC methods for this are almost the same as the one presented for state values
  - except we talk about visits to a state-action pair
- a state-action $$(s,a)$$ pair is visited 
  - if a state $$s$$ is visited and an action $$a$$ is taken in it
- The *every-vist* MC methods and *first-visit* MC method have the same behavior
  - but instead of state, it is a state-action pair
  - they converge quadratically (as before) to the true expected values

The only complication is that many state-action pairs may never be visited.
$$\pi$$ must be a non-deterministic because otherwise we will only observe return for one of the action from each state.
To compare alternatives, we need to estimate the value of *all* the actions from each state.

This is the general problem of *maintaining exploration*.
- For policy evaluation to work for action values, we must assure continual exploration
  - one solution is 
    - start in a *stat-action pair*
    - every pair has a nonzero probability of bein selected as the start
    - this guarantees that all stat-action pair will be visited an infinite number of times
    - we call this **the assumption of exploring starts**

- The assumption of exploring starts can be useful
- but cannot be relied upon in general
  - particularly when learning directly from actual interaction with an environment

Another solution is to consider only stochastic policy.
It means all state-action paris can be encoutered with a nonzero probability.

###5.3 Monte Carlo Control

We will consider how Monte Carlo estimation can be used in control to approximate 
optimal policies.
The overall idea is to proceed the same way we saw in DP methods.

![figure eval-improv](images/eval-improv.png)

$${\pi}_0 \xrightarrow{\mathbb{E}} q_{\pi_0} \xrightarrow{\mathbb{I}} \pi_1 \xrightarrow{\mathbb{E}} \dotso \xrightarrow{\mathbb{I}} \pi_* \xrightarrow{\mathbb{E}} q_*$$

- Policy evaluation is done exactly as described in the preceding section
- Many episodes are experienced, with the approximate action-value approching the true function asymptotically
- We are using *action-value* function instead of value function
  - no model is needed to construct the greedy policy
    - we only choose the action with the maximal action-value

$$       \pi(s) = arg\max_a q(s,a)$$

Policy improvement can be done by constructing each $$\pi_{k+1}$$ as the greedy policy with
respect to $$q_{\pi_k}$$.
The policy improvement theoremthen applies to $$\pi_k$$ and $$\pi_{k+1}$$ because,
forall $$s \in \mathcal{S}$$

$$\begin{align}
q_{\pi_k} (s, \pi_{k+1}(s)) &= q_{\pi_k}(s, arg\max_a q_{\pi_k}(s,a))\\
                            &= \max_a q_{\pi_k}(s,a)\\
                            &\geq q_{\pi_k}(s, \pi_k(s))\\
                            &= v_{\pi_k}(s)
\end{align}$$

Monte Carlo methods can be used to find optimal policies given only sample episodes and 
no other knowledge of the environment's dynamics.

There are 2 assumption that have to be remove for a practical use of MC methods
- episodes have exploring starts
- policy evaluation could be done with an infinite number of episode
  - 2 solutions
    - same methods used in DP, IE stop the algorithm when we obtain a too small variation
    - not trying to complete policy evaluation before returning to policy improvement
- We introduce *Monte Carlo ES* for Monte Carlo with Exploring Starts
  - the observed returns are used for policy evaluation after each episodes
  - then the policy is improved at all the state visited in the episode

![figure 5.4](images/figures5_4.png)
