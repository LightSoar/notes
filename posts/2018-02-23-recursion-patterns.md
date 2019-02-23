# Recursion patterns

## Accumulation
Accumulating the return value ('non-void' return type).

### Pattern
```python
def recur(...):
  if (termination_condition): return initial_value
  return current_value + recur(...)
```

### Example: Sum of an arithmetic progression
```python
def ap_sum(a0, d, n):
	if n==0: return 0
	return a0 + ap_sum(a0+d, d, n-1)
```

### Example: String reversal
```python
def reverse_string(s):
	if s=="": return ""
	return s[-1] + reverse_string(s[:-1])
```

## Simple transformation/traversal
### Pattern
```python
def recur(...):
  if (termination_condition): return
  pre_action()
  recur(...)
  post_action()
```

### Example: Print integer sequence
```python
def print_n_to_1_to_n(n, firstCall=True):

	# Termination condition
	if n < 1: return

	# Pre-action/Step-in (responsible for printing n->1)
	# This happens immediately before every recursive call
	print(n, end='\n' if n==1 else ' ')

	# Recursive call
	print_n_to_1_to_n(n-1, firstCall=False)

	# Post-action/Step-out (responsible for printing 1->n)
	# This happens only after the deepest recursive call returns
	print(n, end='\n' if firstCall else ' ')
```
Output of `print_n_to_1_to_n(9)`:
```
9 8 7 6 5 4 3 2 1
1 2 3 4 5 6 7 8 9
```

### Example: Binary tree traversal
```python
class Node:
	def __init__(self, value, left=None, right=None):
		self.value = value
		self.left = left
		self.right = right

def preOrder(node):
	if node is None: return
	print(node.value, end=' ')
	preOrder(node.left)
	preOrder(node.right)

def inOrder(node):
	if node is None: return
	inOrder(node.left)
	print(node.value, end=' ')
	inOrder(node.right)

def postOrder(node):
	if node is None: return
	postOrder(node.left)
	postOrder(node.right)
	print(node.value, end=' ')
```
#### Output
```python
tree = Node('a', Node('b', Node('d'), Node('e')),
                 Node('c', Node('f', None, Node('h')), Node('g', Node('i'))))

#       a
#     /   \
#   b       c
#  / \    /   \
# d   e  f     g
#         \   /
#          h i

print("Preorder:  ", end=''); preOrder(tree); print()
print("Inorder:   ", end=''); inOrder(tree); print()
print("Postorder: ", end=''); postOrder(tree); print()
```
Output:
```
Preorder:  a b d e c f h g i 
Inorder:   d b e a f h c i g 
Postorder: d e b h f i g c a
```

## Backtracking
The manipulated object is passed by-ref to minimize memory usage.

### Pattern
```python
def recur(...):
  if rejection_condition: return
  if acception_condition:
    print(...)
    return
  
  for b in valid_branchoffs():
    progress(b, ...)
    recur(...)
    regress(b,...)
```
### Example: Find all 77-digit numbers subject to X
```python
# Constraint: the sum of every 3 consecutive digits is equal to 7
def N77(longnum = []):
	if len(longnum) >= 3 and sum(longnum[-3:]) != 7:
		return
	if len(longnum) == 77:
		print(''.join(list(map(str, longnum))))
		return

	for i in range(0 if len(longnum)>0 else 1, 8):
		longnum.append(i)
		N77(longnum)
		longnum.pop()
```

### Example: Sudoku
```python
import numpy as np
from itertools import product

def solveSudoku(board: np.int16):
    if not validSudokuBoard(board):
        return

    N = len(board)
    if (board!=0).sum() == N**2:
        print(board)
        return

    indices = product(range(N), repeat=2)
    for (i,j) in indices:
        if board[i,j]==0:
            for k in [k for k in range(1,N+1)
                      if (k not in board[i,:]) and (k not in board[:,j])]:
                board[i,j] = k
                if validSudokuBox(board, i, j):
                    solveSudoku(board)
                board[i,j] = 0
            break
```
Helper functions:
```python
def validSudokuLine(arr):
    s = set(arr)
    if 0 in s: s.remove(0)
    return (len(s) == sum(arr!=0))

def validSudokuBox(board, i, j):
    lenBox = int(np.sqrt(len(board)))
    iBox = i - i%lenBox
    jBox = j - j%lenBox
    box = board[iBox:iBox+lenBox, jBox:jBox+lenBox]
    return validSudokuLine(box.ravel())

def validSudokuBoard(board: np.int16):
    if not((len(board.shape) == 2) and
           (board.shape[0]==board.shape[1]) and
           (np.sqrt(len(board)).is_integer()) and
           ((board>=0) * (board<=len(board))).all()):
        return False

    for i in range(len(board)):
        row = board[i,:]
        col = board[:,i]
        if not(validSudokuLine(row) and validSudokuLine(col)):
            return False

    return True
```
