[//]: # (Image References)

[image1]: https://github.com/caasted/drlnd-project-2/blob/master/scores_plot.png "Scores Plot"

# Report for Udacity DRLND Project 2 - Continuous Control

## Learning Algorithm

To solve this project I utilized the Deep Deterministic Policy Gradients algorithm from the instructor's solution for the OpenAI Gym's Pendulum environment (https://github.com/udacity/deep-reinforcement-learning/blob/master/ddpg-pendulum/). I modified the definition of the function `ddpg` in `DDPG.ipynb` to interact with the Reacher environment (v2, multiple arms), modified `ddpg_agent.py` to work with multiple reachers simulateously, and increased the depth of the neural network in `model.py`. After those adjustments, I began tuning the hyperparameters until the agent was able to reach the desired performance.

### DDPG

The DDPG algorithm is an actor-critic method that utilizes four neural networks. The first neural network, the actor, learns to select the optimal action for a given set of state variables, while the second neural network, the critic, learns to estimate the value of the current state of the environment. The third and fourth networks are copies of the first two and lag behind the primary networks through the use of a soft update process. These copies are used for the target values in the reinforcement learning update step, which helps make the learning process more stable.

The DDPG algorithm takes advantage of an experience replay buffer. Storing the last N experiences and training the neural networks using a random sampling from them, avoids training on sequences of highly correlated data that could lead to settling at local maxima. This particular implementation of DDPG also uses the Ornstein-Uhlenbeck noise process to add some randomization to the state variables to aid the training process.

### Hyperparameters

 - Episodes and number of steps: 500 at 300 steps, 500 at 500 steps, 500 at 700 steps, up to 1000 at 1000 steps
 - Replay buffer size: 2,000,000
 - Minibatch size: 128
 - Discount factor (gamma): 0.99
 - Soft update factor (tau): 0.001
 - Learning rate actor: 5e-5
 - Learning rate critic: 5e-4
 - Weight decay: 0 (not used)

### Network Architecture (Actor)

 - Fully Connected Layer 1:
   - Inputs: 33
   - Outputs: 768
   - Activation: Rectified Linear Unit
 - Fully Connected Layer 2:
   - Inputs: 768
   - Outputs: 512
   - Activation: Rectified Linear Unit
 - Fully Connected Layer 3:
   - Inputs: 512
   - Outputs: 384
   - Activation: Rectified Linear Unit
 - Fully Connected Layer 4:
   - Inputs: 384
   - Outputs: 256
   - Activation: Rectified Linear Unit
 - Fully Connected Layer 5:
   - Inputs: 256
   - Outputs: 4
   - Acitvation: Hyperbolic Tangent

### Network Architecture (Critic)

Same as the Actor, but with a single output node with a linear activation function.

## Plot of Rewards

![Rewards Plot][image1]

The average score, across 100 episodes and 20 reachers, exceeded 30 after 1575 episodes. This is slightly misleading, because the performance goal had not actually been achieved at 1475 episodes, as indicated in the Jupyter Notebook, but was actually accomplished at the transition from 700-step runs to 1000-step runs after 1500 episodes, at which time the average score increased to well over 30. The single final demo run of the 20 Reachers actually scored an average over 39.

## Ideas for Future Work

Since the instructor's solution was able to reach >30 in far fewer episodes, there is very likely opportunity to improve the agent's performance by experimenting with different model architectures and hyperparameter values. Alternative training algorithms, such as A3C, could also potentially lead to a better agent or faster training time. The saved agent here was also stopped short of reaching optimal performance, since the goal was to report the number of epochs to reach an average score higher than 30. By letting the current agent train for longer, the agent's test performance is likely to continue to improve.
