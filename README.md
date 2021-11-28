# CS698R-Project-Penalty-Shot-Keeper-6

We create a platform to pit SOTA Deep Reinforcement Learning algorithms against each other on the Penalty Shot Kick task. The task involves two agents simulating an penalty shootout. This was part of a course project under Prof. Ashutosh Modi, CS698R, IITK - Deep Reinforcement Learning.

## Abstract

The penalty shot task involves two competing agents in a multi-agent environment with continuous state and action spaces. The kicker controls the puck, while the keeper controls the bar. The kicker's aim is to make the puck hit the goal line without the bar catching it, while the bar on the other hand tries to catch the puck. Through this environment, we study various RL algorithms and benchmark their performance. We also create heuristic based policies to model human play, which can be useful from the neuroscience perspective.

[Report](https://drive.google.com/file/d/1_xkMm-6JUh_VIgNew1a4vPjCpU2wdI5D/view?usp=sharing)

[Presentation](https://docs.google.com/presentation/d/1IQWI3DyXmwVcbSVk2tw5AGpj2htmcwmImcwmi7AX3QY/edit?usp=sharing)

[Demo](https://drive.google.com/drive/folders/1IuIw46Xb06kNobh9M8N60xGwGyFPbGIo?usp=sharing)

## Table of Contents
- [Features](#features)
- [How to get started](#how-to-get-started)
    - [Install packages](#install-packages)
    - [Train and test a model](#train-and-test-a-model)
    - [Play as bar](#to-play-as-bar)
- [Codebase](#codebase)
    - [Game Environment](#game-environment)
    - [Agents](#agents)
    - [Async Communication](#async-communication)
    - [Examples](#examples)
- [Rendered Videos](saved_renders)
- [Authors](#authors)
- [Want to contribute?](#want-to-contribute)
- [Literature References](#literature-references)
- [Acknowledgement](#acknowledgement)

## Features
- Rendering of the custom environment at each step
- Fully configurable environments and policies
- Async server to play with a policy manually

[Back to TOC](#table-of-contents)

## How to get started

### Install packages
>The `-e` flag is included to make the project package editable

>Login to `wandb.ai` to record your experimental runs 
```bash
pip install -e .
pip install -e ./gym-env
wandb login
```

[Back to TOC](#table-of-contents)

### Train and test a model
> Use files in `utils/config/` to control configuration of agent specific policy hyper-parameters and environment parameters

#### Example command to run that trains a puck and bar with PPO algorithm and uses a previously saved policy for each of the agents with 1 training environment and 2 test environment 
```bash
python ./utils/train.py  --wandb-name "Name for Wandb Run" --training-num 1 --test-num 2 --puck ppo --bar ppo --load-puck-id both_ppo --load-bar-id both_ppo 
```

[Back to TOC](#table-of-contents)

### To play as bar:
Open 3 terminals and run 
```bash
python ./examples/server/start_server.py
```
```bash
python ./examples/server/agent_puck.py
```
```bash
python ./examples/server/agent_bar.py
```
Click `Start` and use the mouse slider to control the direction of the bar.

[Back to TOC](#table-of-contents)

## Codebase
### Game Environment
It consists of a puck and a bar with puck moving towards bar at constant horizontal speed. Both of them are controlled by separate agents. The goal of puck is to move past bar and reach final line while the goal of bar is to catch puck before it can reach the final line.

The environment has been developed using OpenAI Gym library which accepts two action parameters corresponding to puck and bar, and moves the game by one time step giving output a tuple of state, reward, completion state and extra information object. [See code](gym-env) [Back to TOC](#table-of-contents)

### Agents
- `lib-agents`: It features trivial, value based and policy based algorithms including `smurve`, `DQN`, `TD3`, `PPO` and `DDPG`.
- `comm-agents`: It implements the hardcoded approach for finding a baseline and pure exploration strategy. It also implements the mouse slider.
- Also implements a `TwoAgentPolicyWrapper` to combine policies for the puck and the agent.
[See code](agents) [Back to TOC](#table-of-contents)

### Utils
- Contains training script and utility functions implementing wrappers
- Stores policy and environment configuration information. 
[See code](utils) [Back to TOC](#table-of-contents)

### Async Communication
To support asynchronous inputs from agents, we have created a main server which controls the environment. The agents using client class to connect to the server and use its step function to give their action and receive corresponding result tuple. The server takes actions from the agents as input and synchronizes them and updates the environment by one time step. [See code](communication) [Back to TOC](#table-of-contents)

### Examples
- Script for playing with the puck as a bar
- A notebook demonstrating smurves
[See Code](examples) [Back to TOC](#table-of-contents)


## Authors
- [Akshat Sharma](https://github.com/akshatsh49) (akshatsh at iitk dot ac dot in)
- [Devanshu Singla](https://github.com/devanshusingla) (dsingla at iitk dot ac dot in)
- [Naman Gupta](https://github.com/namangup) (namangup at iitk dot ac dot in)
- [Sarthak Rout](https://github.com/SarthakRout) (sarthakr at iitk dot ac dot in)

[Back to TOC](#table-of-contents)
## Want to Contribute?
We welcome everyone to report bugs, raise issues, add features etc. Drop a mail to the [authors](#authors) to discuss more.

[Back to TOC](#table-of-contents)

## Literature References
- McDonald, K.R., Broderick, W.F., Huettel, S.A. et al. Bayesian nonparametric models characterize instantaneous strategies in a competitive dynamic game. Nat Commun 10, 1808 (2019). https://doi.org/10.1038/s41467-019-09789-4
- Iqbal SN, Yin L, Drucker CB, Kuang Q, Gariépy JF, et al. (2019) Latent goal models for dynamic strategic interaction. PLOS Computational Biology 15(3): e1006895. https://doi.org/10.1371/journal.pcbi.1006895
- Kelsey R McDonald, John M Pearson, Scott A Huettel, Dorsolateral and dorsomedial prefrontal cortex track distinct properties of dynamic social behavior, Social Cognitive and Affective Neuroscience, Volume 15, Issue 4, April 2020, Pages 383–393, https://doi.org/10.1093/scan/nsaa053
- He He, Jordan Boyd-Graber, Kevin Kwok and Hal Daumé III. Opponent Modeling in Deep Reinforcement Learning. ArXiv:1609.05559

[Back to TOC](#table-of-contents)
## Acknowledgement
- We thank Prof. Ashutosh Modi for providing us with this opportunity to explore and learn more about SOTA algorithms through a project.
- We thank our mentor, Avisha Gaur for guiding us and clarifying our doubts related to the environment
- We thank our TA, Gagesh Madaan for providing valuable inputs regarding various solution approaches.
- We also thank the creators of Tianshou and OpenAI Gym library which forms a core part of our codebase
- We thank the open source community for wonderful libraries for everything under the sun! 

[Back to TOC](#table-of-contents)
