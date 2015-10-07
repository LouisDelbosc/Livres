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

A simple alternative is to behave greedily most of the time but every once in a while, say with small probability *e*, instead to select randomly from amongst all the actions with equal probability
We call it the *e-greedy* methods

*Advantage*:
- On a long run, all Qt(a) will converge to q(a)

Chosing the *e* (ie probability to not play the estimated maximum value action) is  important
For example if we take a very low *e* then we will explore slowly, and we will converge to the real maximum later.
But if we chose a high *e* then we might find it faster but after we will only play the best action 1-*e* (because *e* time we will play a random move wich is not the estimated max).
A fix at this issue is to change the *e* over the time. High in the beginning but decreasing over time so we explore a lot early and we play the optimal move at the end.

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

###2.4 Tracking NonStionary Problem
