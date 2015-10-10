##Chapter 3
##Finite Markov Decision Processes

###3.1 The Agent-Environment Interface

Some definition:
- **Agent**: the learner and decision-maker
- **Environment**: things the *agent* interact with
- **Action**: the way the *agent* interact with its *environment*
- **Reward**: a special numerical values send by the *environment* to the *agent* that it tried to maximize

![Figure 1: The agent-environment interaction in reinforcement learning](images/figure_1.png)

The agent interact with its environment at discrete time steps.
At each time step $$t$$ the agent receives some representation of the environment's *state*.

Let $$S_t \in \mathcal{S} A_t \in \mathcal{A(S_t)} R_{t+1} \in \mathcal{R} \subset \mathbb{R}$$ 

Where :
- $$\mathcal{S}$$ is the set of all possible states
- $$\mathcal{A(S_t)}$$ is the set of actions available in state S_t
- $$R_{t+1}$$ the reward associated to the previous action

At each time step, the agent implements a mapping from states to action.
This mapping is called a *policy*.
- $$\pi_t$$ is a policy at time step t
- $$\pi_t(a | s)$$ is the probability that $$A_t = a$$ if $$S_t = s$$

The framework is abstract and flexible so it can easily be extended to many reinforcement problems.

###3.2 Goals and Rewards

The reward is a simple number, $$R_t \in \mathbb{R}$$ passed from the environment to the agent.
The agent's goal is maximizing *cumulative* reward in the long run.
We can clearly state this informal idea as the *reward hypothesis*:

*That all of what we mean by goals and purposes can be well thought of as the maximization of the expected value of thecumulative sum of a received scalar signal (called reward).*

You must express your goal in terms of reward. Take care not to try to reward sub-goal.
For example in chess, your reward has to be :
- victory = +1
- draw = 0
- loss = -1

If you implement sub-goal by the means of capturing some pawn or something else,
your algorithm may try to achieve its sub-goal at the cost of its ultimate goal.

It is the environment which give the reward and not the agent itself.
We define the agent as everything we do not have full control over.
We can only interact with it with actions.
This does not preclude the agent from defining for itself a kind of internal reward
or a sequence of internal rewards.
Indeed, this is exactly what many reinforcement learning methods do.

###3.3 Returns

Let's defined our goal formally.
We want to maximize the *expected return*.
- Let $$G_t$$ a specific function of the reward sequence
- Let $$T$$ is a final time step

$$\begin{align}
G_t = R_{t+1} + R_{t+2} + R_{t+3} + ... + R_T
\end{align}$$

There is two kind of problems:
- episodic problems
  - there are episodes which ends in a special state called the *terminal state*
  - followed by a reset to a standard starting state.
- continiuing problems
  - the opposite of *episodic problems*
  - in this case the final step $$T = \infty$$

We will use an other mathematical definition of *returns* to simplify both problems

$$\begin{align}
G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + ... = \sum_{k=0}^{infty} \gamma^2 R_{t+k+1}

0 \leq \gamma \leq 1
\end{align}$$

$$\gamma$$ is called the *discount rate*.

The discrount rate determines the present value of future rewards.
The higher $$\gamma$$ is, the more strongly the objective takes futures rewards into account

###3.4 Unified Notation For Episodic and Continuing Tasks
