# reinforcement-learning

# Description of the Maze environment:

The focus of this Coursework is the resolution of a Maze environment, illustrated in Figure 1 and model as a Markov Decision Process (MDP). In this illustration, the
black squares symbolise obstacles, and the dark-grey squares absorbing states, that correspond to specific rewards. Absorbing states are terminal states, there is no 
transition from an absorbing state to any other state.
This environment shares a lot of similarities with the Grid World environment studied in Tutorial 2, you are allowed to reuse any code you might have developed for 
this tutorial. However, unlike the Tutorial, this Coursework aims to help you implement RL techniques that do not require full knowledge of the dynamic of the 
environment, namely Monte-Carlo and Temporal-Difference learning. In the following, we will thus assume we don’t have access to the transition matrix T and reward
function R of the environment, and try to find the optimal policy π by sampling episodes in it. Consider an agent moving inside this maze, trying to get the best possible reward. To do so, it can
choose at any time-step between four actions:

- a0 = going north of its current state
- a1 = going east of its current state
- a2 = going south of its current state
- a3 = going west of its current state

The chosen action has a probability p to succeed and lead to the expected direction. If it fails, it has equal probability to lead to any other direction. For 
example, if the agent chooses the action a0 = going north, it is going to succeed and go north with probability p, and it has probability 1−p/3 to go east, probability
1−p/3 to go south and probability 1−p/3 to go west. p is given as a hyperparameter, defining the environment. If the outcome of an action leads the agent to a wall or
an obstacle, its effect is to keep the agent where it is. For example, if the agent chooses the action a1 = going east and there is a wall east but no wall in the 
other directions, it is going to stay in place in his current state with probability p and to go north, south or west with the same probability 1−p/3. Any action 
performs by the agent in the environment gives it a reward of −1. The episode ends when the agent reach one of the absorbing state (Ri)0≤i≤3 or if it performs more
than 500 steps in the environment. Finally, the starting state is chosen randomly at the beginning of each episode from the possible start states with equal 
probability 1/Nstart−states. The possible start states are indicated on the Maze illustration as ”S”.

This environment is personalised by your College ID (CID) number, specifically the last 2 digits (which we refer to as y and z), as follow:

• The probability p is set as p = 0.8 + 0.02 × (9 − y).
• The discount factor γ is set as γ = 0.8 + 0.02 × y.
• The reward state is Ri with i = z mod 4 (meaning that if z = 0 or z = 4 or z = 8, the reward state would be R0, if z = 1 or z = 5 or z = 9, it would be R1, etc).
• The reward state Ri gives a reward r = 500 and all the other absorbing states gives a reward r = −50. As stated before, any action performed by the agent in the 
environment also gives it a reward of −1.
