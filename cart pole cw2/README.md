# Overview

# Problem Description

Your goal is to train an agent to balance a pole attached (by a frictionless joint) to a moving (frictionless) cart by applying a fixed force to the cart in either the 
left or right direction. Please see Fig.1 for an illustration. The aim is to train the DQN to keep the pole balanced (upright) for as many steps as possible. We do not 
control the magnitude of force we apply to the cart, only the direction. The optimal policy will account for deviations from the upright position and push the cartpole 
such that it remains balanced.

Figure 1: Illustration of the OpenAI Gym CartPole environment.

Our action space is discrete and of size 2. We have action 0, apply a force on the cart to the left, or action 1, apply a force on the cart to the right. The 
observation state we obtain from the environment is of size 4 of the following positions and velocities: cart position, cart velocity, pole angle and pole angular 
velocity. All four observations of the environment are initially assigned a random value between −0.05 and 0.05. So the cart starts close to the origin with the pole 
almost upright and a low initial angular velocity. The aim is to keep the pole upright for as many steps as possible. This makes the reward quite simple to define: for
each step taken (including final step) a reward of +1 is returned. The environment will only terminate when it reaches any of the following states:

- pole angle greater than ±12◦ (or equivalently 0.2094 radians)
- cart distance from centre greater than ±2.4,
- or the number of steps exceeds 500.

## DQN Implementation

Recall from the lectures, a DQN is a neural network designed to predict the Q function (for all the possible actions) of the environment given a state vector. The DQN
provided works on the Gym “CartPole” environment. Please see Fig. 2 for an example. The first layer takes as input the observed state. The number of outputs in the 
final layer of the network must be the same number of actions the agent can perform. This is how the state-action values are encoded in the neural network: the DQN
takes a state as input, and its nth output neuron’s value is the learned Q-value at the input state for the nth action.
The code provided alongside this assignment contains a PyTorch implementation of a simple DQN architecture, along with sample plotting and visualisation code, and trains 
the model to predict the action the agent should take to balance the cart pole. However, the model is not optimised and therefore does not converge to consistently 
balance the pole. Below, you can find a description of the functions and classes included in utils.py. You are strongly suggested to understand how these are implemented,
and you may modify these as you wish.

Figure 2: Diagram of a DQN with input layers and output layers matching those required for this
environment. Please note, you decide the parameters for the hidden layers (i.e., number of layers and
number of parameters per layer).

## Replay buffer

A replay buffer is implemented in the ReplayBuffer class. At initialisation it takes an integer as
argument which is the maximum number of transitions it can hold. It includes the following methods:

- push() method: Adds the object that is passed as input to the replay buffer’s memory. If an item is added when the replay buffer is at full capacity, it discards 
the oldest item it has in memory and replaces it with the newest. This method returns the updated replay buffer’s memory (an iterable object that contains the objects
pushed to it as elements).
- sample() method: It returns an iterable object of items uniformly sampled (without replacement) from the replay buffer’s memory. It takes as parameter an integer 
defining the number of samples to be returned.

## Defining a DQN

The DQN class (inherited from nn.Module) that will be your multi-layer DQN perceptron. It includes the following __init__() and forward() methods:

- __init__(): At initialisation, it takes a list of integers defining the number of neurons in each layer of the DQN. For example, a network with 2-d input, 1-d output
and 2 hidden layers of size 50 will take the list [2,50, 50, 1] as initialisation parameter.
- forward(): Implements a batched forward pass through the neural network, using a ReLU activation function. Carrying forward the example from above, for an input 
batch tensor of shape (N, 2) the output should have shape (N, 1). The DQN also handles non-batched states: so for an input of size (2,) the output is of size (1,).

## Action selection

The function greedy_action() takes two parameters as input: a DQN object and a (non-batched) state tensor and returns the integer corresponding to the greedy action 
at the given state according to the DQN.

## ε-greedy policy

The function epsilon_greedy() takes three parameters as input: an epsilon float, a DQN object and a (non-batched) state tensor. epsilon_greedy() returns an integer 
representing a stochastic sample of the epsilon greedy action at that state according to the DQN.

## Target network

The function update_target() takes two DQN objects as arguments, and updates the parameters of the first one copying the weights and biases from the second.

## Loss calculation

The function loss() computes the Bellman error for a batch of transitions. loss takes 7 arguments: two DQN objects (a policy and target network) and batched (i.e. 
two-dimensional) tensors with states, actions, rewards, next states and ‘dones’ in that order. For reference, the loss for a batch B of size N is computed according 
to the formula:

![equation](https://github.com/gaia2510/reinforcement-learning/blob/main/cart%20pole%20cw2/loss%20function%20latex.png)

with Q the first (policy) DQN, Qˆ the second (target) DQN. This implementation carries out the sum in parallel (as a batch) rather than looping through each 
transition, as this enables training to be much faster.

Hint: Understanding PyTorch’s gather() function will be useful to follow the given implementation.
