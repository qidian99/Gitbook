# Algorithms

## Plan

1. Binary Tree Traversal
   1. In, Pre, Post
   2. Morris
   3. Iterative, Recursive
2. Backtracking
3. Dynamic Programming
4. OA 5

## Binary Search

1. findMedianSortedArrays \(4\): 2hr
   1. if the length of the array is even, the median is \(arr\[n/2\] + arr\[n/2 - 1\]\)/2.0
   2. if there are multiple if statement and you only want one of them to be executed, check for side effect so that a variable should not be modified in one if statement that will make another if condition true
   3. loop condition \(len + 1\)/2 or len/2+1 \(even/odd case\)
   4. To ensure a condition like m &lt; n in recursion, we can reverse it and pass it back to the recursion function

      ```text
      findMedianSortedArrays(int[] A, int[] B) {
        int m = A.length;
        int n = B.length;
        if (m > n) {
          return findMedianSortedArrays(B, A); // 保证 m <= n
        }
      }
      ```

   5. Boundary condition: min/max, +/- 1
2. 
## BFS

1. rightSideView \(199\): 0.5hr

   1. the level can be tracked by the size of result array

2. numIslands \(200\): 15min
   1. Can extract the terminating condition for recursion out from the if statements

      ```text
      if (r == -1 || c == -1 || r == rows || c == cols || grid[r][c] != '1') {
          return;
      }
      ```

   2. Can change the marks instead of creating a new array:

      ```text
      //当前 1 标记为 2
      grid[r][c] = '2';
      ```
3. s

## DP

1. maxProfit\(188, 121, 123, 53\)
   1. \(53: 30min\) 只是在定义的时候一个表示以 i 开头的子数组，一个表示以 i 结尾的子数组，却造成了时间复杂度的差异
   2. \(121: 15min\) one time: 当前 sell 指向的价格小于了我们买入的价格，所以我们要把 buy 指向当前 sell 才会获得更大的收益
      1. Two pointer
         1. When to update: when the current sell price is less than the buy in price
   3. \(123: 1hr\)
      1. 在第 j 天买入，收益就是 prices\[i\] - prices\[j\] + dp\[j-1\]\[k-1\]，多加了前一天的最大收益
      2. 我们多加了前一天的最大收益，我们也可以改成加当前天的最大收益。

         在第 `j` 天买入，收益就是 `prices[i] - prices[j] + dp[j][k-1]`

      3. Wrong: the partial result does not matter if it is GUARANTEED that there will be a value greater than the current result. If the difference of prices from day j-1 to day j is increasing, we can just skipped the transaction and gain delta more profile. If it is decreasing, then any transaction that sells at day j can have more profile if it instead sells at day j-1
   4. DP array meaning
      1. 2d, order to populate
      2. reduce 2d to 1d if it is traversed row by row or column by column
      3. reverse the order for for loop \(inner first and outer next\) may cause other initialization of temporary variable
2. 


## 

## Traversing Tree

