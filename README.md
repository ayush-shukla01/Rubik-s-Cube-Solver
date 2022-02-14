# Rubik-s-Cube-Solver

#Table of Content
1. General Info
2. Data Representation
3. Training
4. Model Application
5. Interpretation

# 1. General Info
 This project is an attempt to implement the following paper: https://arxiv.org/abs/1805.07470.
  
 A 3x3 Rubik's Cube has 4.33x10¹⁹ distinct state, with only one goal state. A brute froce search approach for solving the cube will be highly impractical. In this project, Autodidactic Iteration (ADI) algorithm was used to train the model. ADI is an iterative supervised learning which trains a deep neural network which takes in inpui state s and outputs a value, policy pair.  The policy output p is a vector containing the move probabilities for each of the 12 possible moves from that state. Once the network is trained, the policy is used to reduce breadth and the value is used to reduce depth in the MCTS.

# 2. Data Representation
 In order to solve 3x3 cube using model we have to take two entities into consideration: actions and states.
 
 **Actions**
 Actions are the possible rotations we can perform on any given cube state. We can perform 12 actiions in total, 6 clockwise and 6 anti-clockwise. The naiming of the actions is based on the cube side side we are rotating( left, right, top, bottom, front and back) and type of rotation( clockwise and anti-clockwise).
 
 **States**
 A state is a particular configuration of the colors(cube stickers) and the size of our state space is really huge( 4.33x10¹⁹ distinct state). While trying to solve the cube, the model needs to keep track of 20 x 24 tensors for every state of the cube. This 20 x 24 is derived based on the cubelets and their respective color combination that we need to keep track of. For every state of the cube, we only to keep track of the cube stickers marked with white in the figure below: 
 ![image](https://user-images.githubusercontent.com/98556827/153905889-b156ea03-92ab-42fa-a7fd-24eefffbde21.png)
                             
                             Stickers we need to track in the cube are marked with white
 We only keep track of the corner and side cuibelets as the central cubelets can only rotate and not change their relaticve positions. Each corner cubelet, has 3 color stickers on it. In order to precisely, indicate the position and orientation of the corner cube, we have 8x3 = 24 combinations. For the 12 side cubelets, we have only 2 color stickers. Which again gives us 12x2 = 24 combinations- Therefore, we have 20 cubelets with each having 24 possible positons.
 
 # 3. Training
 While training the model, we start from goal state and apply sequence of random transformations of some pre-defined length N. This gives us sequence of N states where for each state we perform the following procedure:
 - apply the 12 transformations to the state s
 - pass these 12 new states to the neural network and ask for the "value" of each state. This gives us 12 values for every sub-state of state s.
 - Then target value for state s is calulated as  vᵢ = maxₐ(v(sᵢ,a)+R(A(sᵢ,a)))  where A(sᵢ,a) is the state after action a is performed on state sᵢ and R(sᵢ) equals 1 for goal state and -1 otherwise
 - target policy for state s is calculated using the same formula, but instead of max we take argmax: pᵢ = argmaxₐ (v(sᵢ,a)+R(A(sᵢ,a))). Our target policy will have 1 at the position of maximum value for sub-state and 0 on all other positions.
 
 This process is illustrated in the following figure from paper:
 ![image](https://user-images.githubusercontent.com/98556827/153919187-4ca17588-05e1-43e3-bf39-f6989a6a7067.png)

 
  **Neural Network Architecture**
  The neural network architecture is taken from the paper.
  ![image](https://user-images.githubusercontent.com/98556827/153911088-92735e16-aa96-46ea-9085-6c2a4c66a90f.png)
  
# 4. Model Application
  
  



Project Organization
------------

    ├── LICENSE
    ├── CSVS               <- Contain the CSVs generated for series of tests with increasing complexity(scramble depth)
    │   ├── c3x3           <- Contains the CSVs generated for 3x3 cube
    │
    ├── cubes_tests        <- Contains Sample problem set files for 3x3 and 2x2 cubes
    │
    ├── ini                <- Contains the files with parameters that are used for training the model
    │
    ├── libcube            
    │   ├── cubes         
    │   │   ├── _common.py <- Methods for permutation, rotation and orientation of cubelets as per the environment and mapping
    │   │   ├── _env.py    <- class and method for setting up the cube environment, goal state,etc. 
    │   │   ├── cube2x2.py <- Defines the state, actions, transformation and encoding for 2x2 cube
    │   │   └── cube3x3.py <- Defines the state, actions, transformation and encoding for 3x3 cube
    │   │
    │   ├── conf.py        <- Congiguration file for training the model
    │   ├── mcts.py        <- Algorithm used for solving the cubes
    │   └── model.py       <- Classes & methods for setting up NN and training the model.
    │
    ├── docs               <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │   ├── cubes2x2       <- Contains the .dat files generated while training 2x2 cube
    │   └── cubes3x3       <- Contains the .dat files generated while training 3x3 cube
    │
    ├── nbs                <- Contains notebooks that provide insights into the model and results
    │                      
    ├── tests              <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └──Libcube         <- Generated graphics and figures to be used in reporting
    │   │   └──Cubes        
    │                      
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment
    │                         
    ├── gen_cubes.py       <- Tool to generate test set for solver.py
    │                       
    ├── solver.py          <- Source code for use in this project.
    │
    ├─ train.py            <- Trains the model
    │                      
    ├─ train_debug.py      <-  Used to analyze trained model and other training process details
    │
    └── run_tests.sh       <- tox file with settings for running tox; see tox.readthedocs.io 

