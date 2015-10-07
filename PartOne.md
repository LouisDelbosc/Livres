#PART ONE
#Tabular Solution Methods

##Chapter 2
##Multi-arm Bandits

The n-armed bandits is a classical problem were we can experiment and explain some of classicals reinforcement issues.
You have n one-armed bandit (casino machines) and you try to know with which you'll get more gain.

In this chapter we study the evaluative aspect of reinforcement learning in simplified setting, one that does not involve learning to act in more than one situation. We call this *nonassociative* setting.

###2.1 An n-Armed Bandit Problem

Quick problem presentation:
- You have n different options or actions possible
  - each choice give you a numerical reward chosen from a stationary probability distribution
- You objectif is to maximize the expected total reward over some time period
  - for example 1000 selections or a time steps

We assume we do not have access to the value of each action.
We are confronted to the **exploration-exploitation dilemma**.
- *Exploitation*: Taking the best reward known
- *Exploration*: Try to explore the unknow space of action to find better reward

It is essential to know when to explore and when to exploit.

###2.2 Action-Value Methods

**Notation**:
- a: action
- q(a): The value of action
- Qt(a): the estimated value on the *t*-th time step of a
- Rt: the reward the *t*-th time
- Nt(a): the time a has been chosen at time t

We can estimate Qt(a) as :
```
Qt(a) = Sum(1,Nt(a)) Ri / Nt(a)
```
Or the mean of the Reward

If Nt(a) = 0, then we define Qt(a) instead as some default value such as Q1(a) = 0

The simplest action selection rule is to select at *t* the highest estimated action value.
It is the *greedy* action.

```
At = argmax(a) Qt(a)
```
*argmax(a)* denotes the value of a at which the expression that follows is maximized
Greedy action selection always exploits current knowledge to maximize immediate reward.

A simple alternative is to behave greedily most of the time but every once in a while, say with small probability &epsilon;, instead to select randomly from amongst all the actions with equal probability
We call it the *&epsilon;-greedy* methods

*Advantage*:
- On a long run, all Qt(a) will converge to q(a)

Chosing the &epsilon; (ie probability to not play the estimated maximum value action) is  important
For example if we take a very low &epsilon; then we will explore slowly, and we will converge to the real maximum later.
But if we chose a high &epsilon; then we might find it faster but after we will only play the best action 1-&epsilon; (because &epsilon; time we will play a random move wich is not the estimated max).
A fix at this issue is to change the &epsilon; over the time. High in the beginning but decreasing over time so we explore a lot early and we play the optimal move at the end.

Of course this is only available if we have a static problem which is not the case is many reinforcement learning problems.

###2.3 Incremental Implementation

Since each action as to keep a records of the *rewards*

```
Qt(a) = [ R1 + R2 + ... + R(Nt(a)) ] / Nt(a)
```

You can't implement this equation like this because the more reward you'll get, the more memory you'll need.
Hopefully there is a trick to calculate easily Qt(a).

```
Qk+1 = 1/k * Sum (Ri)
     = Qk + 1/k [ Rk - Qk ]
```

We can have Qt(a) with the previous estimation of q(a).
We just need to store the iteration (k), and the previous reward.
We also need an arbitrary Q1

The general form is :
**NewEstimate <-- OldEstimate + StepSize [ Target - OldEstimate ]**

```quote
The expression *[Target - OldEstimate]* is an *error* in the estimate. It is reduced by Taking a step toward the "Target". The target is presumed to indicatea desirable direction in which to move, though it may be noisy. In the case above, for example, the target is the k-th reward
```
The *StepSize* parameter is 1/k in our example but can be otherwise. It was a meaningful impact on the system.

###2.4 Tracking NonStionary Problem

If we want our Qk(a) not to be too influenced by the past, we can change our StepSize by a static value.
Let's define *StepSize* as &alpha;

Qk+1 = Qk + &alpha;[Rk - Qk]
     = &alpha;Rk + (1 - &alpha;)Qk

We call this a weighted average because the sum of the weights is :
(1 - &alpha;)^k + Sum ( &alpha;(1 - &alpha;)^(k-i) ) = 1

This is called and *exponential, recency-weighted average.*
Sometimes it is convenient to vart the step-size parameter from ste to step.

A well-known result in stochastic approximation theory gives us the conditions required to assure convergence with probability 1

InfiniteSum ( &alpha;k(a) ) = &infin;
and
InfiniteSum ( &alpha;k(a)² ) < &infin;

The first condition is required to guarantees that the steps are large enough to eventually overcome any initial conditions or random fluctuations.
The seoncd condition guarantees that eventually the steps become small enough to assure convergence

###2.5 Optimistic Initial Values

All the methods from above are biased by their initial estimates (Q1(a) or &epsilon; for example).
The bias disappears once all actions have been selected at least once but for methods with constant &alpha;, the bias is permanent though decreasing over time.
In practice this kind of bias can sometimes be very helpful because it is an easy way to supply some prior knowledge about what level of rewards can be expected.

We can force the system to explore by fixing high initial value (for example Q1(a) = 5) and if the reward is less than the initial value, then it will continue to explore (since max Q(a) is still within the actions not chosen).
It is called *optimistic initial values*.

It is not well suited to nonstationary problems because its drive for exploration is inherently temporary. If the task changes, creating a renewed need for exploration, this method cannot help.
Anyway, the beginning of time occurs only once so we should not focus on it too much.

###2.6 Upper-Confidence-Bound Action Selection
