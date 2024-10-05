# Connect-4-Monte-Carlo-Tree-Search
Creating a connect 4 bot using the MCTS algorithm.

This project contains all the python code files you need to make a strong connect 4 bot, which you can play against.
Below I explain the algorithm in detail.

### MCTS Explanation
MCTS consists of the following three main functions:

1.	Selection function: with the UCT formula (upper confidence bound) we find 1 node, with connect 4, we choose from 7 nodes (because 7 possible moves). Part 1 of the UCT is Q/N. Q is the number of times won with this node and N is the number of times played with this node. This is exploitation. Part 2 is c*sqrt((log(N_parent))/N). Thus, new nodes that have not yet been tried are also chosen. This is exploration. This formula is applied to all 7 possible nodes and the one with the highest score is chosen.

2.	Simulation/roll-out function: After choosing a node, the game is simulated, this is completely random. The 7 child nodes (less if a column is already full so actually the possible moves) are found with the expansion function. One is simply chosen randomly. The following information about that node is stored: parent = the current node (the first node has no parent), N, Q and the 7 child nodes. Simulation continues until the game is finished (win, or if there are no more moves: tie).

3.	Back propagation function: N and Q are updated for each node from the tree. Q is of course different for player A than for player B. The main purpose of this is to propagate the results back to the first child nodes of the current game state. In addition, the accuracy of subsequent moves only gets better because if MCTS played a move and then player B also played a move, you don't need to create a completely new tree. You already have most of the structure, including the statistics of the new first child nodes of the new current game state that the back propagation function has already updated. However, the node corresponding to the new game state must be found in the tree and initialised without a parent, because its parent is of course no longer relevant. Little is done with the nodes of player B and is more for administration.

Functions 1, 2 and 3 are played over and over again until a certain end condition is reached, say after 5 seconds or after 10,000 roll-outs. Then the best move is chosen. Often this is the child node with the highest N because less time is spent on worse options during execution so the best option will then naturally have many Ns. However, you can also choose the highest Q/N but then you can get a node that has had few simulations and thus does not give a reliable result (maybe a bit weird). A combination of both ways can be a good technique though. 
After choosing and playing a move, the tree is not discarded but reused as explained in function 3 in the last sentences.

### Is it unbeatable?
You can set the number of simulations yourself (self.stop). The higher you set this number, the better the bot will play, but the thinking time also increases. In addition, progress decreases exponentially when increasing the simulations. I set it to 5000 and then played for a few hours. At first I lost everything, but I got better and better and the bot plays a bit predictably. Also, MCTS occasionally missed the best move. That's why I won most of the games at one point. This encouraged me to test another algorithm: Minimax. Check out this repository too!

### Other useful information
The code folder contains 2 files:
  1. Connect4: the game. Start this file if you want to play against a friend;
  2. MCTS: the controller with the algorithm. Start this file if you want to play against the algorithm.

The only library you need to install for this project is Numpy.
