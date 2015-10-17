##Chapter 4
##Dynamique Programming

**Dynamique Programming**:
- it refers to a collection of algorithm that can be used to compute optimal policies
- it needs a perfect model of the environment (like a MDP)
- it has a great computation cost
- it has limited utility in reinforcement learning because of the cost and the model requirement
- it is important theoretically
- it is a base for many algorithm which aims at reproducing the effect of DP

They key idea of DP, and of reinforcement learning, is the use of value functions 
to organize and structure the search for good policies.

Let's rewrite the $$v_*$$ and $$q_*$$ which satisfy the Bellman optimality equations:
$$\begin{align}
v_*(s) &= \max_a \mathbb{E} [R_{t+1} + \gamma v_*(S_{t+1}) | S_t = s, A_t = a]\\
       &= \max_a \sum_{s', r} p(s', r | s, a) [r + \gamma v_*(s')]
\end{align}$$

or

$$\begin{align}
q_*(s,a) &= \mathbb{E} [R_{t+1} + \gamma \max_{a'} q_*(S_{t+1}) | S_t = s, A_t = a]\\
       &= \sum_{s', r} p(s', r | s, a) [r + \gamma \max_{a'} q_*(s')]
\end{align}$$

###4.1 Policy Evaluation

*Policy evaluation*:

$$\begin{align}
v_{\pi}(s) &= \mathbb{E}_{\pi} [ R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dotso | S_t = s ]\\
       &= \mathbb{E}_{\pi} [R_{t+1} + \gammav_{\pi}(S_{t+1}) | S_t = s]\\
       &= \sum_a \pi(a|s) \sum_{s', r}p(s', r|s, a) [r + \gamma v_{\pi)(s')}]
\end{align}$$

- $$\pi(a|s)$$ is the probability of taking action $$a$$ in state $$s$$ under policy $$\pi$$o- The existence and uniqueness of $$v_{\pi}$$ are guaranteed as long as either $$\gamma < 1$$ or eventual termination is guaranteed from all states under the policy $$\pi$$

To find the solution to this equation we will do an iterative methods.
We can considere a sequence of appromixate $$v_0, v_1, v_2, \dotso$$
The initial value is chosen arbitrarily.
**Each successive approximation is obtained by using the Bellman equation vor $$v_{\pi}$$ as an update rule.**

$$\begin{align}
v_{k+1}(s) &= \max_a \mathbb{E} [R_{t+1} + \gamma v_k(S_{t+1}) | S_t = s, A_t = a]\\
       &= \max_a \sum_{s', r} p(s', r | s, a) [r + \gamma v_{k}(s')]
\end{align}$$

$$v_k = v_{\pi}$$ is a fixed point for this update rule.
This algorithm is called *iterative policy evaluation*.

$$k \rightarrow \infty \Rightarrow v_k \rightarrow v_{\pi}$$

We replace the value of the old state by a new value which will also be replace later.

![Figure 4.1: Iterative policy evaluation](images/figure4_1.png)

To stop the algorithm (since it converges in the limit) we test the quantity
$$\max_{s_\mathcal{S}} |v_{k+1(s) - v_k(s)|$$. When it is sufficiently small we stop the loop.

