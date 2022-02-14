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
 ![image](https://user-images.githubusercontent.com/98556827/153905889-b156ea03-92ab-42fa-a7fd-24eefffbde21.png)
                             
                             Stickers we need to track in the cube are marked with white
        
  


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

