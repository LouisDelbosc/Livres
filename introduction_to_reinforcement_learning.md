#INTRODUCTION TO REINFORCEMENT LEARNING

##Chapter 1
## The Reinforcement Learning Problem

Reinforcement learning is about understanding cause and effects. It is focus around goal.

###1.1 Reinforcement Learning

In an essential way they are *closed-loop* problems because the learning system's actions influence its later inputs

Reinforcement learning problems involve learning what to do -- how to map situations to actions -- so as to maximize a numerical reward signal.

Reinforcement_Learning(situation) = action which maximize Reward

The learner is not told which actions to take, as in many forms of machine learning,but instead must discover which actions yield the most reward by trying them out.

There are three most important distinguishing feature of reinforcement learn  problems.
- being closed-loop in an essential way
- not havin direct instructions as to what actions to take
- where the consequences of actions, including reward signals, play out over extended time periods


*How to define a reinforcement learning method:*
**Create a agent interacting with its environment to achieve a goal**
The goald must be related to the state of the environment
Three aspects :
- sensation
- action
- goal

Supervised and unsupervised learning achieve different goal and aim at different results.
Unsupervised aims at finding hidden structures.
Moreover there is exploitation-exploration dilemma. The agent has to choose but also keep exploring new possibilities.
This dilemma does not appear in supervised and unsupervised learning (at least in their purest forms).


###1.3 Elements of Reinforcement Learning

You can identify 4 main subelements of a reinforcement learning system:
- a *policy*
  - It defines the way our agent behave
  - It is a mapping from perceived state of the environment to actions to be taken when in those states
  - Policies may be stochastic
- a *reward signal*
  - A single number to let our agent know if he took the right decision
  - It can only be changed via the agent's actions
  - It depend on :
    - the current action
    - the current state of the agent's environment
  - The reward signal is the primary basis for altering the policy
    - If policy A give low reward => policy must be change to choose other action in this situation
  - It general, rewardsignals may be stochastic functions of the state of the environment and the actions taken
- a *value function*
  - Its goal is to evaluate a position to the long run
  - Value of state = total amount of reward an agent can expect to accumulate over the future, starting from that state
  - The only goal of value is for the agent to get more reward
  - But to evaluate a position we look at the value
  - Hard to determine and have to be change overtime to stick to the environment
  - Value estimation has a central role in reinforcement learning
- a *model* of the environment (optionnal)
  - It mimics the behavior of the environment
  - Used for planning
  - Example: (state, action) => (next state, reward)
  - Model-based methods are opposed to model-free methods that are explicitly *trial-and-error learners*

###1.4 Limitations and Scope

Value function is not mandatory. For example evolutionnary methods can work without it.
Evolutionnary methods (genetic algorithm) have advantages on problems in which the learning agent cannot accurately sense the state of its environment.

Our focus is on reinforcement learning that involve learning while interacting with the environment.
Evolutionnary methods do not act this way.

Other methods like *policy gradient methods* do not have any value function but still are reinforcement.
They estimate the directions the parameters should be adjusted in order to most rapidly improve a policy's performance.

Some comment on reinforcement learning and optimization methods because it is a source of misunderstanding.
Trying to maximized a quantity does not mean that that quantity is ever maximized.
Reinforcement learning always try to increase the amount of reward it receives but many factors can prevent it from achieving the maxium, even if one exists.
**Optimization is not the same as optimality.**

###1.5 An extended example: Tic-Tac-Toe

Better to read it, page 24

###1.6 History of Reinforcement Learning

look at Szita

###1.8 Bibliographical Remarks

Books from (sometime he gives only the author and the date):
- Coverage of reinforcement learning
  - Szepesvari (2010)
  - Bertsekas and Tsitsklis (1996)
  - Kaelbling (1993a)
- Control or operation research perspective
  - Si et al. (2004) (written like that in the book)
  - Powell (2011)
  - Lewis and Liu (2012)
- Machine Learning
  - Sutton (1992)
  - Kaebling (1996)
- Useful surveys
  - Barto (1995b)
  - Kaelbling, Littman, Moore (1996)
  - Keerthi and Ravindran (1997)
- Recent developments
  - Weiring and van Otterlo (2012)
