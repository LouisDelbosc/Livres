#PART ONE
#Tabular Solution Methods

##Chapter 2
##Multi-arm Bandits

The n-armed bandits is a classical problem were we can experiment and explain some of classical reinforcement issues.
You have n one-armed bandit (casino machines) and you try to know with which you'll get more gain.

In this chapter we study the evaluative aspect of reinforcement learning in simplified setting, one that does not involve learning to act in more than one situation. We call this *non-associative* setting.

###2.1 An n-Armed Bandit Problem

Quick problem presentation:
- You have n different options or actions possible
  - each choice give you a numerical reward chosen from a stationary probability distribution
- Your goal is to maximize the expected total reward over some time period
  - for example 1000 selections or a time steps

We assume we do not have access to the value of each action.
We are confronted to the **exploration-exploitation dilemma**.
- *Exploitation*: Taking the best reward known
- *Exploration*: Try to explore the unknown space of action to find better reward

It is essential to know when to explore and when to exploit.

###2.2 Action-Value Methods

**Notation**:
- $$a$$: action
- $$q(a)$$: The value of action
- $$Q_t(a)$$: the estimated value on the *t*-th time step of a
- $$R_t$$: the reward the *t*-th time
- $$N_t(a)$$: the time a has been chosen at time t

We can estimate Qt(a) as :

$$Q_t(a) = \sum_{1}^{N_t(a)}\frac{R_i}{N_t(a)}$$

Or the mean of the Reward

If $$N_t(a)$$ = 0, then we define $$Q_t(a)$$ instead as some default value such as $$Q_1(a)$$ = 0

The simplest action selection rule is to select at *t* the highest estimated action value.
It is the *greedy* action.


$$A_t = argmax_a Qt(a)$$

$$argmax(a)$$ denotes the value of a at which the expression that follows is maximized
Greedy action selection always exploits current knowledge to maximize immediate reward.

A simple alternative is to behave greedily most of the time but every once in a while, say with small probability $$\epsilon$$, instead to select randomly from amongst all the actions with equal probability.
We call it the *$$\epsilon$$-greedy* methods

*Advantage*:
- On a long run, all $$Q_t(a)$$ will converge to $$q(a)$$

Choosing the $$\epsilon$$ (IE probability to not play the estimated maximum value action) is  important
For example if we take a very low $$\epsilon$$ then we will explore slowly, and we will converge to the real maximum later.
But if we chose a high $$\epsilon$$ then we might find it faster but after we will only play the best action 1-$$\epsilon$$ (because $$\epsilon$$ time we will play a random move which is not the estimated max).
A fix at this issue is to change the $$\epsilon$$ over the time. High in the beginning but decreasing over time so we explore a lot early and we play the optimal move at the end.

Of course this is only available if we have a static problem which is not the case is many reinforcement learning problems.

###2.3 Incremental Implementation

Since each action as to keep a records of the *rewards*

$$Q_t(a) = \frac{[R_1 + R_2 + \dotsm + R_{N_t(a)}]}{N_t(a)}$$


You can't implement this equation like this because the more reward you'll get, the more memory you'll need.
Hopefully there is a trick to calculate easily Qt(a).

$$\begin{align}
Q_{k+1} &= \frac{1}{k} \sum_{i=1}^{k+1} R_i\\
        &= Q_k + \frac{1}{k} [R_k - Q_k]\\
\end{align}$$


We can have $$Q_t(a)$$ with the previous estimation of $$q(a)$$.
We just need to store the iteration (k), and the previous reward.
We also need an arbitrary $$Q_1$$

The general form is :
**NewEstimate <-- OldEstimate + StepSize [ Target - OldEstimate ]**

The expression *[Target - OldEstimate]* is an *error* in the estimate.
It is reduced by Taking a step toward the "Target".
The target is presumed to indicatea desirable direction in which to move, though it may be noisy.
In the case above, for example, the target is the k-th reward

The *StepSize* parameter is 1/k in our example but can be otherwise. It was a meaningful impact on the system.

###2.4 Tracking NonStionary Problem

If we want our $$Q_k(a)$$ not to be too influenced by the past, we can change our StepSize by a static value.
Let's define *StepSize* as $$\alpha$$

$$\begin{align}
Q_{k+1} &= Q_k + \alpha[R_k - Q_k]\\
        &= \alpha R_k + (1 - \alpha)Q_k\\
        &= \dotsi\\
        &= (1 - \alpha)^k Q_1 + \sum_{i=1}^{k} \alpha (1 - \alpha)^{k-1} R_i
\end{align}$$

We call this a weighted average because the sum of the weights is :
$$(1 - \alpha)^k + \sum_{i=1}^{k} \alpha (1-\alpha)^{k-i} = 1$$

This is called and *exponential, recency-weighted average.*
Sometimes it is convenient to vart the step-size parameter from step to step.

A well-known result in stochastic approximation theory gives us the conditions required to assure convergence with probability 1.

$$\sum_{k=1}^{\infty} \alpha_k(a) = \infty$$
**and**
$$\sum_{k=1}^{\infty} \alpha^2_k(a) < \infty$$

The first condition is required to guarantees that the steps are large enough to eventually overcome any initial conditions or random fluctuations.
The second condition guarantees that eventually the steps become small enough to assure convergence

###2.5 Optimistic Initial Values

All the methods from above are biased by their initial estimates ($$Q_1(a)$$ or $$\epsilon$$ for example).
The bias disappears once all actions have been selected at least once but for methods with constant $$\alpha$$, the bias is permanent though decreasing over time.
In practice this kind of bias can sometimes be very helpful because it is an easy way to supply some prior knowledge about what level of rewards can be expected.

We can force the system to explore by fixing high initial value (for example $$Q_1(a) = 5$$) and if the reward is less than the initial value, then it will continue to explore (since $$max_a Q(a)$$ is still within the actions not chosen).
It is called *optimistic initial values*.

It is not well suited to non-stationary problems because its drive for exploration is inherently temporary. If the task changes, creating a renewed need for exploration, this method cannot help.
Anyway, the beginning of time occurs only once so we should not focus on it too much.

###2.6 Upper-Confidence-Bound Action Selection

The greedy actions are those that look best at present, but some of the other actions may actually be better.
We can select our action with an other equation than (E1).
$$
A_t = argmax_a \left[ Q_t(a) + c \sqrt{\frac{ln(t)}{N_t(a)}}\right]
$$
(see page 55, equation 2.8 from pdf for a more visible version of the equation)

This is called *upper confidence bound* action selection.

It is very effective on n-armed problems but it is hard to extend this methods to more general reinforcement learning settings.

###2.7 Gradient Bandits


So far in this chapter we have considered methods that estimate action values and use those estimates
to select actions. This is often a good approach, but it is not the only one possible. In this section
we consider learning a numerical preference Ht(a) for each action a. The larger the preference, the more
often that action is taken, but the preference has no interpretation in terms of reward.

*Preference* is a complicated subject and I invite you to look at the equation in page 57 to 60 in the book.


Once can gain a deeper insight into this algorithm by understanding it as a stochastic approximation
to gradient ascent


The idea is to create an other parameter ( *preference* ) to chose which action to do.

###2.8 Associative Search

To simplify our explanation, we only considered only *non-associative* tasks, in which there is no need to associate different actions with different situations.
In a general reinforcement learning task there is more than one situation, and the goal is to learn a policy: a mapping from situations to the actions that are best in those situations.

*Associative search* task involves both **trial-and-error** learning in the form of *search* for the best actions and *association* of these actions with the situations in which they are best.


They are like the full reinforcement learning problem in that they involve learning a policy,
but like our version of the n-armed bandit problem in that each action affect the next 
situation as well as the reward, then we have the full reinforcement learning problem.

