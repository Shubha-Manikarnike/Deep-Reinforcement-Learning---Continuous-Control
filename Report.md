# Report
---
This project utilised the DDPG (Deep Deterministic Policy Gradient) architecture outlined in the [DDPG-Bipedal Udacity project repo](https://github.com/udacity/deep-reinforcement-learning/tree/master/ddpg-bipedal).

## State and Action Spaces
In this environment, a double-jointed arm can move to target locations. A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector must be a number between -1 and 1.

## Learning Algorithm

The agent training utilised the `ddpg` function in the DDPG_Continuous_Control notebook.

It continues episodical training via the the ddpg agent until `n_episoses` is reached or until the environment tis solved. The  environment is considered solved when the average reward (over the last 100 episodes) is at least +30.0. Note if the number of agents is >1 then the average reward of all agents at that step is used.

Each episode continues until `max_t` time-steps is reached or until the environment says it's done.

As above, a reward of +0.1 is provided for each step that the agent's hand is in the goal location.

The DDPG agent is contained in [`ddpg_agent.py`](https://github.com/Shubha-Manikarnike/Deep-Reinforcement-Learning---Continous-Control/blob/master/ddpg_agent.py)

For each time step and agent the Agent acts upon the state utilising a shared (at class level) `replay_buffer`, `actor_local`, `actor_target`, `actor_optimizer`, `critic_local`, `criticl_target` and `critic_optimizer` networks.

### DDPG Hyper Parameters
- n_episodes (int): maximum number of training episodes
- max_t (int): maximum number of timesteps per episode
- num_agents: number of agents in the environment

Where
`n_episodes=300`, `max_t=1000`

Upon running this numerous times it became apparent that the environment would return done at 1000 timesteps. Higher values were irrelevant.

### DDPG Agent Hyper Parameters

- BUFFER_SIZE (int): replay buffer size
- BATCH_SIZ (int): mini batch size
- GAMMA (float): discount factor
- TAU (float): for soft update of target parameters
- LR_ACTOR (float): learning rate for optimizer
- LR_CRITIC (float): learning rate for optimizer
- WEIGHT_DECAY (float): L2 weight decay
- N_LEARN_UPDATES (int): number of learning updates
- N_TIME_STEPS (int): every n time step do update


Where 
`BUFFER_SIZE = int(1e6)`, `BATCH_SIZE = 128`, `GAMMA = 0.99`, `TAU = 1e-3`, `LR_ACTOR = 1e-4`, `LR_CRITIC = 1e-4`, `WEIGHT_DECAY = 0.0`, `N_LEARN_UPDATES = 10` and `N_TIME_STEPS = 20`

Succesful training was also achieved with `N_LEARN_UPDATES = 8` in a shorter number of episodes but longer running due to no CUDA enabled GPUs on it. 

### Neural Networks

Actor and Critic network models were defined in [`ddpg_model.py`]https://github.com/Shubha-Manikarnike/Deep-Reinforcement-Learning---Continous-Control/blob/master/ddpg_model.py).

The Actor networks utilised two fully connected layers with 256 and 128 units with relu activation and tanh activation for the action space. The network has an initial dimension the same as the state size.

The Critic networks utilised two fully connected layers with 256 and 128 units with leaky_relu activation. The critic network has  an initial dimension the size of the state size plus action size.

## Plot of rewards
![Reward Plot](https://github.com/Shubha-Manikarnike/Deep-Reinforcement-Learning---Continuous-Control/blob/master/Output/Output.png?raw=true)

```
Episode 155	Score: 32.33	Average Score: 29.1617
Episode 156	Score: 33.03	Average Score: 29.2987
Episode 157	Score: 32.62	Average Score: 29.4370
Episode 158	Score: 32.34	Average Score: 29.5510
Episode 159	Score: 32.85	Average Score: 29.7084
Episode 160	Score: 33.55	Average Score: 29.8305
Episode 161	Score: 32.33	Average Score: 29.9425
Episode 162	Score: 31.93	Average Score: 30.0751

Environment solved in 62 episodes!	Average Score: 30.07
```

## Ideas for Future Work
Experimentation occurred on a few machines including a 6 core xeon Mac Pro with AMD GPUs, a 6th Gen I7 Intel with a GTX 1070 and an AMD 1700x with a RTX 2080. The later machine whilst having higher specifications ran significantly slower (needed to upgrade pytorch to work with the RTX 2080). However in all cases the training performance was slow and at times unstable. The GPUs were being under utilised. There is opportunity to move more code to being able to be GPU utilised without transfer of data back to the CPU.

In addition Proximal Policy Optimization (PPO) and Distributed Distributional Deterministic Policy Gradients (D4PG) methods could be explored.
