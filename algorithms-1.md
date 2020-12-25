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

1. maxProfit\(188, 121, 123, 53\): 2.5hr
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
2. rob\(198\): 15min
3. rob\(213\): 30min:
   1. 这道题通过分类的思想，成功将新问题化解到了已求解问题上，这个思想也经常遇到。
4. maximalSquare\(221\): 45min
   1. queue.size\(\) is dynamic. If want to loop by level, set a local variable first
5. Maximal Rectangle 85: 1hr
6. Largest Rectangle in Histogram 84: 1.5hr
   1. 最坏的情况，递推式代入，依旧是 O（n log（n））。而找最小柱子，就可以抽象成，在一个数组区间内找最小值问题，而这个问题前人已经提出了一个数据结构，可以使得时间复杂度是 log（n），完美！名字叫做线段树，可以参考[这里](https://zhuanlan.zhihu.com/p/34150142)和[线段树空间复杂度](https://blog.csdn.net/gl486546/article/details/78243098)，我就不重复讲了。主要思想就是利用二叉树，使得查找时间复杂度变成了 O（log（n））。

      我们以序列 { 5, 9, 7, 4 ,6, 1} 为例，线段树长下边的样子。节点的值代表当前区间内的最小值。

      ![](https://windliang.oss-cn-beijing.aliyuncs.com/84_3.jpg)

   2. 这样的话时间复杂度达到了O（n）。开始自己不理解，问了一下同学。其实证明的话，可以结合解法五，我们寻找 leftLessMin 其实可以看做压栈出栈的过程，每个元素只会被访问两次。

      ```text
      int[] leftLessMin = new int[heights.length];
      leftLessMin[0] = -1;
      for (int i = 1; i < heights.length; i++) {
          int l = i - 1;
          //当前柱子更小一些，进行左移
          while (l >= 0 && heights[l] >= heights[i]) {
              l = leftLessMin[l];
          }
          leftLessMin[i] = l;
      }
      ```

   3. Stack. 

      * 如果当前栈空，或者当前柱子大于栈顶柱子的高度，就将当前柱子的下标入栈
      * 当前柱子的高度小于栈顶柱子的高度。那么就把栈顶柱子出栈，当做解法四中的当前要求面积的柱子。而右边第一个小于当前柱子的下标就是当前在遍历的柱子，左边第一个小于当前柱子的下标就是当前新的栈顶。

      Update max value when pop. Update max value when iteration ends and stack is not empty.
7. Trapping Rain Water 42: 1hr
   1. Row by row
   2. Column by row
   3. DP: max left and right \(column by column\)
   4. Two pointer
   5. Stack \(stack.peek\(\) and current height\)









## Quick sort

## Merge sort





## 

## Traversing Tree

