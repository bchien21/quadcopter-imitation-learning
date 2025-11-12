# Building Neural Networks trained with Imitation Learning on Quadcopter Obstacle Avoidance using PyTorch

This repository contains Jupyter Notebooks and JSON file datasets for training neural networks on quadcopter obstacle avoidance. The JSON file dataset is generated from a (global quadcopter motion planner)[https://github.com/bchien21/starling2-motion_planner]
that acts as the "teacher" of the neural network. 

## Architecture Overview 
The neural network architecture is built with PyTorch. Currently, it is an encoder-decoder model, where the input to the encoder is a goal (x,y,z ) configuration, and the encoder is a straightforward MLP. The decoder is an LSTM, that is trained 
to output a pose waypoint (translation + quaternion) and a probability that this pose waypoint is the final waypoint of the collision-free trajectory. Essentially, the model takes in a goal translation, and outputs a collision-free trajectory 
of varying length for a quadcopter to follow, which necessitates the use of a RNN like the LSTM. 

During training, teacher forcing is used to give the LSTM the ground-truth input at the current time-step, and to tell the LSTM when to stop unrolling. 
During inference, the LSTM is told to keep on unrolling and adding predicted waypoints to the current trajectory it is trying to build, until it itself predicts that the current waypoint is the last waypoint in the trajectory based on the probability it outputs.
Instead of having ground-truth waypoint data as its input at the current time-step, it will use the waypoint it predicted from the previous timestep as the input. 
