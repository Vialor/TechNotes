# Minimax Search
> Minimax search is optimal  
> Time complexity: O(b^m)  

AI: Max player, Opponent: Min player
```Python
def minimaxSearch(state):
	# set up game
	# game isTerminal, actions, utility, result
	value, move = maxValue(state)
	return move
	
def maxValue(state):
	if game.isTerminal(state):
		return game.utility(state, player), None
	value, move = float("-inf"), None
	for move in game.actions(state):
		vc, mc = minValue(game.result(state, move))
		if vc > value:
			value, move = vc, mc
	return v, move
	
def minValue(state):
	if game.isTerminal(state):
		return game.utility(state, player), None
	value, move = float("inf"), None
	for move in game.actions(state):
		vc, mc = maxValue(game.result(state, move))
		if vc < value:
			value, move = vc, mc
	return value, move
```
# α-β Search
> Prune branches for minimax search  
> α-β Search is optimal  
> Time complexity: O(b^m)  
  
α = best minimax value for MAX player so far, max(values of explored children)
β = best minimax value for MIN player so far, min(values of explored children)
Each node will has an α-β pair based on search history of post-order traversal; prune children branches when α ≥ β is spotted in children.
```Python
def minimaxSearch(state):
	# set up game
	# game: isTerminal, actions, utility, result
	alpha, beta= inf("-inf"), inf("inf")
	value, move = maxValue(state)
	return move
	
def maxValue(state):
	nonlocal alpha, beta
	if game.isTerminal(state):
		return game.utility(state, player), None
	value, move = float("-inf"), None
	for move in game.actions(state):
		vc, mc = minValue(game.result(state, move))
		if vc > value:
			value, move = vc, mc
			alpha = max(alpha, value)
			if alpha >= beta:
				return value, move
	return value, move
	
def minValue(state):
	nonlocal alpha, beta
	if game.isTerminal(state):
		return game.utility(state, player), None
	value, move = float("inf"), None
	for move in game.actions(state):
		vc, mc = maxValue(game.result(state, move))
		if vc < value:
			value, move = vc, mc
			beta = min(beta, value)
			if alpha >= beta:
				return value, move
	return value, move
```
## Cutting-off Search

> Cutoff when certain depth is reached; use an Evaluation Function to evaluate searched states
> Instead of `game.isTerminal`, use `game.isCutoff` which takes in an additional parameter `depth`