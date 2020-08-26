# Binary Tree

## Recursion

use helper



## Stack

1. create stack and result list
2. ref to current \(root\)
3. while current not null and stack not empty, "push" the stack to the left of current
   1. stops when current is empty
   2. now, need to pop from the stack
      1. Note, for post order traversal, pop is replaced by peek
4. Add the val to the result when the node is popped
5. Go right
6. Check for any return condition in the while loop
   1. For post order traversal, check whether the last visited node is the right node of current
   2. For post order traversal, may check when the stack is empty in the peek
7. Return the result at the end

