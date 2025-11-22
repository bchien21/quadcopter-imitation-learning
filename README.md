# Building Neural Networks trained with Imitation Learning on Quadcopter Obstacle Avoidance using PyTorch

This repository contains Jupyter Notebooks and JSON file datasets for training neural networks on quadcopter obstacle avoidance using imitation learning. A [global planner](https://github.com/bchien21/starling2-motion_planner) generates collision-free trajectories using the following information: start state, goal state, obstacle locations, and obstacle dimensions. It is used to generate a labeled dataset (Where the label is the states and obstacle information, and the data is the trajectory) that is then used for supervising a model. 

## Architecture Overview 

The model is currently a vanilla LSTM (An RNN is needed because output trajectories are of variable-length). As an input, the LSTM takes in the goal state and the state it predicted the quadcopter to be in at the previous timestep. And then it outputs the state it thinks the quadcopter should be at at this current timestep. 

During training, the length of the ground truth trajectory is used to force the LSTM to stop unrolling, regardless of what its prediction is currently and what they've been in the past. But during inference. the LSTM continues to unroll until it outputs a state that is within a threshold to the goal. 
