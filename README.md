# mcts-tictactoe
Learning how to play Tic Tac Toe through MonteCarlo Tree Search

***
## Design choices
Among the design choices, we can find:
* The __rollout policy__ is a randomised set of moves. Once an unvisited node is selected, we sample randomly among its children till a leaf with a terminal state is found. This has been designed like this because we have no prior statistics to base our moves on. 
* Similarly, the __opponent’s policy__ was also randomised since we can’t anticipate the opponent‘s moves. When doing the simulation, we assume that our opponent's moves are random.
* The UCT (argamax=Q(node)/N(node)+C·N(parent)/N(node)) as our __selection policy__ to find a balance between exploit and exploration. Q(node) is a score defined by +1 for wins, -1 for losses, and 0 for draws. N(node) is the number of times a node has been visited. N(parent) is the number of times the parent node has been visited. C is a constant which defines the relative importance given to uncertainty. In our implementation we set C = 0.2. Also, all nodes are initialised with values Q=0 and N=1 to handle the division by 0.
* After each rollout, the score of the end result (0 for draw, 1 for win, or -1 for loss) is added to the starting node of the rollout and propagated up the tree to the current root in the Q counter. Also, the number of visits is updated by +1 to the above mentioned nodes,

***
## Evaluation of the implemented algorithm

The search using the MCTS is performed with a time constraint which defaults to 5 seconds. Thanks to this, we limit the maximum runtime of the AIs simulations, while allowing for an extensive exploration of possible moves. At the start of the AI player’s turn it searches for the optimal move through the MCTS, then it chooses the move with the highest number of visits as this is considered the best move. 

To avoid memory bloat no information is saved between the states, as very few of the branches calculated at a previous turn would be relevant. For example, if the AI player starts the game first, it will explore 9 different possible starting moves. Then the opponent has 8 possible moves. Which means that 8*9 = 72 branches are explored, of which only 1 will be relevant the next time it’s the AI player’s turn. By only considering relevant branches, the sample efficiency is also kept high.
In order to evaluate the AI’s performance we created a simple ui to test the AI against human players and against a random player. 

The learned policy is competitive enough to be a reasonable opponent for a human player, but it can be beaten since the AI always looks for the winning moves rather than trying to minimise losses. This is because the selection policy we’ve implemented looks for the maximum of the player’s UTC instead of trying to minimise the maximum of the opponent’s UTC.

The implemented code is suitable to use in larger grids, meaning that the code is escalable. In the script, you can find an example when the AI system is playing against a random player with a 4x4 grid, only by providing a larger grid as an input to the node.

To facilitate tracking of the board states, we decided that instead of crosses and rings the players would place 1 and -1 respectively. If the sum of elements in a row, a column, or a diagonal is equal to the dimension of the board, player 1 wins. If the sum is equal to -dimension, player 2 wins. If none of these conditions are achieved when an end state is reached, it’s a draw.
