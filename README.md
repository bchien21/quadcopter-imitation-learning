# Building Neural Networks trained with Imitation Learning on Quadcopter Obstacle Avoidance using PyTorch

This repository contains Jupyter Notebooks and JSON file datasets for training neural networks on quadcopter obstacle avoidance using imitation learning. A [global planner](https://github.com/bchien21/starling2-motion_planner) generates collision-free trajectories using the following information: start state, goal state, obstacle locations, and obstacle dimensions. It is used to generate a labeled dataset (Where the label is the states and obstacle information, and the data is the trajectory) that is then used for supervising a model. 

## Architectures Overview 

Currently, there are two notebooks to train two different models: An LSTM RNN and a 1-D Conditional U-Net Diffusion Model.

For the LSTM, it takes in the goal state and the state it predicted the quadcopter to be in at the previous timestep. And then it outputs the state it thinks the quadcopter should be at at this current timestep. During training, the length of the ground truth trajectory is used to force the LSTM to stop unrolling, regardless of what its prediction is currently and what they've been in the past. But during inference. the LSTM continues to unroll until it outputs a state that is within a threshold to the goal. 

For the diffusion model, during training, some random noise is added to the ground truth trajectory. The U-Net is trained to predict the noise that was added (MSE Loss on the actual noise and the predicted noise). During inference, a noisy trajectory is sampled from a Gaussian. This noisy trajectory, the quadcopter goal (which is used as a conditional state), and the current diffusion timestep are fed into the model to get the noise to be removed at that timestep. Then, a noise scheduler actually removes the noise at that timestep. This repeats until the maximum timestep is reached.  

It was discovered that the LSTM has trouble predicting trajectories when trained on data where one label could result in multiple outputs. For example, in a simple case of going around an obstacle, valid paths are to go to the left and right of the obstacle. If this behavior is present in training data, the LSTM will try to minimize the loss between these paths, and this results in it predicting a straight-line path through the obstacle. A diffusion model mitigates this problem because it is generative. 
