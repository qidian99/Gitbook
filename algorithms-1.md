# Algorithms

## Plan 1

1. Binary Tree Traversal
   1. In, Pre, Post
   2. Morris
   3. Iterative, Recursive
2. Backtracking
3. Dynamic Programming
4. ~~OA 5~~

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



## Category

### Sliding Window

1. Maximum sum subarray of size k \(53\)

   ```java
   public int maxSubArray(int[] nums) {
     int n = nums.length;
     int dp = -1;
     int max = Integer.MIN_VALUE;
     for (int i = 0; i < n; i++) {
       if (dp < 0) {
         dp = nums[i];
       } else {
         dp = nums[i] + dp;
       }
       max = Math.max(max, dp);
     }
     return max;
   }
   ```

2. Minimum Size Subarray Sum \(209\)

   Binary search &lt; or &lt;= \(if both are valid values, use &lt;=\)

   ```java
   while (minLen <= maxLen) {
       //取中间的长度
       midLen = (minLen + maxLen) >>> 1;
       //判断当前长度的最大和是否大于等于 s
       if (getMaxSum(midLen, nums) >= s) {
           maxLen = midLen - 1; //减小长度
           min = midLen; //更新最小值
       } else {
           minLen = midLen + 1; //增大长度
       }
   }     
   ```

3. longest-substring-with-at-most-k-distinct-characters

   ```java
   import java.util.*;
   import java.lang.*;
   import java.io.*;

   class GFG {
   	public static void main (String[] args) throws IOException{
   		//code
   		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
   		int t = Integer.parseInt(br.readLine());
   		while(t-->0){
   		    String str = br.readLine();
   		    int k = Integer.parseInt(br.readLine());
   		     int len = str.length();
		     
   		    int[] arr = new int[26];
   		    int dis=0;
   		    int start=0;
   		    int maxLen=-1;
   		    for(int i=0;i<len;i++){
   		        int index = str.charAt(i)-'a';
   		        if(arr[index]==0){
   		            dis++;
   		        }
   		        arr[index]++;
   		        while(dis>k){
   		            arr[str.charAt(start)-'a']--;
   		            if(arr[str.charAt(start)-'a']==0){
   		                dis--;
   		            }
   		            start++;
   		        }
   		        if(dis==k && maxLen<i-start+1){
   		            maxLen=i-start+1;
   		        }
   		    }
   		    System.out.println(maxLen);
		    
   		}
   	}
   }
   ```

4. \*\*\*\*[**Fruit Into Baskets**](https://leetcode.com/problems/fruit-into-baskets/description/) **\(904\)**

   ```java
   public int totalFruit(int[] tree) {
       int[] types = new int[40000];

       int start = 0, end = 0;
       int numTypes = 0;
       int count = 0;
       int max = 0;
       while (end < tree.length) {
         int t = tree[end++];
         count += 1;
         if (types[t]++ == 0) {
           numTypes += 1;
         }

         if (numTypes <= 2) {
           max = Math.max(count, max);
           continue;
         }

         while (start < end) {
           count--;
           if (--types[tree[start++]] == 0) {
             numTypes -= 1;
           }
           if (numTypes <= 2) {
             break;
           }
         }
       }
       return max;

     }
   ```

5. Longest Substring Without Repeating Characters \(3\)

   ```java
   public int lengthOfLongestSubstring(String s) {
       int n = s.length();

       int i = 0, j = 0, ans = 0;

       HashSet<Character> set = new HashSet<>();

       while (i < n && j < n) {
         if (!set.contains(s.charAt(j))) {
           set.add(s.charAt(j));
           j++;
           ans = Math.max(ans, j - i);
         } else {
           set.remove(s.charAt(i));
           i++;
         }
       }

       return ans;

     }
   ```

6. characterReplacement2 \(424\) 

   ```java
   public int characterReplacement(String s, int k)
   {
       int[] count = new int[128];
       int max=0;
       int start=0;
       for(int end=0; end<s.length(); end++)
       {
           max = Math.max(max, ++count[s.charAt(end)]);
           if(max+k<=end-start)
               count[s.charAt(start++)]--;
       }
       return s.length()-start;
   }
   ```

   Historical maximum matters

7. Max Consecutive Ones III \(1004\)

   \`\`\`jaav

   This is the real magic of sliding window: historical best

### Two pointers

1. Pair with target sum \(1\)

   1. Map: what is the key, what is the value? difference & index resp.

   ```java
   public int[] twoSum(int[] nums, int target) {
     // diff
     int n = nums.length;
     int[] res = new int[2];
     Map<Integer, Integer> map = new HashMap<>();
     for (int i = 0; i < n; i++) {
       if (map.containsKey(nums[i])) {
         int p = map.get(nums[i]);
         res[0] = p;
         res[1] = i;
         return res;
       }

       map.put(target - nums[i], i);

     }

     return res;
   }

   public int[] twoSum(int[] nums, int target) {
       Map<Integer,Integer> map=new HashMap<>();
       for(int i=0;i<nums.length;i++){
           int sub=target-nums[i];
           if(map.containsKey(sub)){
               return new int[]{i,map.get(sub)};
           }
           map.put(nums[i], i);
       }
       throw new IllegalArgumentException("No two sum solution");
   }
   ```

2. Remove duplicates \(26\)

   ```java
   public int removeDuplicates(int[] nums) {
       if (nums.length == 0) return 0;
       int i = 0;
       for (int j = 1; j < nums.length; j++) {
           if (nums[j] != nums[i]) {
               i++;
               nums[i] = nums[j];
           }
       }
       return i + 1;
   }

   ```

3. Squaring a Sorted Array \(977\)

   ```java
   public int[] sortedSquares(int[] A) {
       int n = A.length;
       int[] result = new int[n];
       int i = 0, j = n - 1;
       for (int p = n - 1; p >= 0; p--) {
           if (Math.abs(A[i]) > Math.abs(A[j])) {
               result[p] = A[i] * A[i];
               i++;
           } else {
               result[p] = A[j] * A[j];
               j--;
           }
       }
       return result;
   }
   ```

   Pointer can be start from middle or two ends

4. 3Sum \(15\), 4Sum \(18\)

   ```java
   public List<List<Integer>> threeSum(int[] num) {
       Arrays.sort(num); //排序
       List<List<Integer>> res = new LinkedList<>(); 
       for (int i = 0; i < num.length-2; i++) {
           //为了保证不加入重复的 list,因为是有序的，所以如果和前一个元素相同，只需要继续后移就可以
           if (i == 0 || (i > 0 && num[i] != num[i-1])) {
               //两个指针,并且头指针从i + 1开始，防止加入重复的元素
               int lo = i+1, hi = num.length-1, sum = 0 - num[i];
               while (lo < hi) {
                   if (num[lo] + num[hi] == sum) {
                       res.add(Arrays.asList(num[i], num[lo], num[hi]));
                       //元素相同要后移，防止加入重复的 list
                       while (lo < hi && num[lo] == num[lo+1]) lo++;
                       while (lo < hi && num[hi] == num[hi-1]) hi--;
                       lo++; hi--;
                   } else if (num[lo] + num[hi] < sum) lo++;
                   else hi--;
              }
           }
       }
       return res;
   }
   ```

5. 3Sum Closest \(16\)

   ```java
   public int threeSumClosest(int[] nums, int target) {
       Arrays.sort(nums);
       int sub=Integer.MAX_VALUE;
       int sum=0;
       for(int i=0;i<nums.length;i++){ 
           int lo=i+1,hi=nums.length-1;
           while(lo<hi){
               if(Math.abs((nums[lo]+nums[hi]+nums[i]-target))<sub){
                   sum=nums[lo]+nums[hi]+nums[i];
                   sub=Math.abs(sum-target);
               }
               if(nums[lo]+nums[hi]+nums[i]>target){
                   hi--;
               }else{
                   lo++;
               }
           }
       }
       return sum;
   }
   ```

   for absolute value comparison, can set a temp variable and update two values when condition meets

6. Triple with sum smaller than value

   ```java
   class Solution(object):
       def threeSumSmaller(self, nums, target):
           """
           :type nums: List[int]
           :type target: int
           :rtype: int
           """
           numbers = nums
           if len(numbers) < 3:
               return 0
           numbers.sort()
           result = 0
           for idx, num in enumerate(numbers):
               t = target - num
               #only start with afterward elements
               twoSumResults = self.twoSum(numbers[idx+1:], t)
               result += twoSumResults
           return result

       def twoSum(self, numbers, target):
           # need to find all combinations now
           if len(numbers) < 2:
               return 0
           result = 0
           start, end = 0, len(numbers) -1
           while start < end:
               while end > start and numbers[start] + numbers[end] >= target:
                   end -= 1
               if start < end:
                   result += end - start
                   start += 1
               else:
                   break

           return result
   ```

   Note: `result += end - start` means `result += (end - start + 1) - 1`

7. Subarray Product Less Than K \(713\)

   ```java
   public int numSubarrayProductLessThanK(int[] nums, int k) {

       int n = nums.length;

       if (n == 0) {
         return 0;
       }
       int res = 0;
       int left = 0, right = 0;
       int prod = 1;

       for (; right < n; right++) {
         prod *= nums[right];
         // if (right < left) {
         // right = left;
         // prod *= nums[left];
         // }

         while (left <= right && prod >= k) {
           prod /= nums[left++];
           // System.out.println(prod);
         }

         res += right - left + 1;
         // if (right <= left) {
         // right = left + 1;
         // }
         // for (; right < nums.length; right++) {
         // prod *= nums[right];
         // if (prod >= k) {
         // break;
         // }
         // }
         // // i, ... , j-1
         // System.out.println("j : " + right + " i : " + left);

         // res += (right - left) * (right - left + 1) / 2;
       }

       return res;
     }
   ```

   How to extend the pointers: expand right first + increment left or increment right + shrink left. It depends on the target.

8. Dutch national flag problem

   ```java
     public void sortColors(int[] nums) {
       int i = 0;
       int j = 0;
       int n = nums.length - 1;
       int p = 1;
       while( j <= n ){
           if(nums[j] < p){
               swap(nums, i, j);
               i++;
               j++;
           }else if (nums[j] > p){
               swap(nums, j, n);
               n--;
           }else{
               j++;
           }
       }
   }

   private void swap(int[] arr, int i, int j){
       int t = arr[i];
       arr[i] = arr[j];
       arr[j] = t;
   }
   ```

   Pointer `p` grows from left to right. when pointer does not change, we might need extra comparison .

### Fast and Slow pointers

1. LinkedList cycle \(141\)

   ```java
   public boolean hasCycle(ListNode head) {
       ListNode slow = head;
       ListNode fast = head;
       while (fast != null) {
           if (fast.next == null) {
               return false;
           }
           slow = slow.next;
           fast = fast.next.next;
           if (fast == slow) {
               return true;
           }
       }
       return false;
   }
   ```

2. Find start of loop:
   1. We have discussed [Floyd’s loop detection algorithm](https://www.geeksforgeeks.org/detect-loop-in-a-linked-list/). Below are steps to find the first node of the loop. 1. If a loop is found, initialize a slow pointer to head, let fast pointer be at its position.  2. Move both slow and fast pointers one node at a time.  3. The point at which they meet is the start of the loop.
   2. Distance traveled by fast pointer = 2 \* \(Distance traveled 

                                               by slow pointer\)



      \(m + n\*x + k\) = 2\*\(m + n\*y + k\)



      Note that before meeting the point shown above, fast

      was moving at twice speed.



      x --&gt;  Number of complete cyclic rounds made by 

             fast pointer before they meet first time



      y --&gt;  Number of complete cyclic rounds made by 

             slow pointer before they meet first time

       m + k = \(x-2y\)\*n



      Which means m+k is a multiple of n. 



      So if we start moving both pointers again at the same speed such that one pointer \(say slow\) begins from the head node of the linked list and other pointers \(say fast\) begins from the meeting point. When the slow pointer reaches the beginning of the loop \(has made m steps\), the fast pointer would have made also moved m steps as they are now moving the same pace. Since m+k is a multiple of n and fast starts from k, they would meet at the beginning. Can they meet before also? No, because the slow pointer enters the cycle first time after m steps.
3. Happy number \(202\)

   ```java
   // 计算各个位的平方和
     private int getNext(int n) {
       int next = 0;
       while (n > 0) {
         int t = n % 10;
         next += t * t;
         n /= 10;
       }
       return next;
     }

     public boolean isHappy(int n) {
       int slow = n;
       int fast = n;
       do {
         slow = getNext(slow);
         fast = getNext(getNext(fast));
       } while (slow != fast);
       return slow == 1;
     }
   ```

   HashSet to check if visited

4. Middle of the LinkedList \(876\)

   ```java
   public ListNode middleNode(ListNode head) {
       ListNode slow = head, fast = head;
       while (fast != null && fast.next != null) {
           slow = slow.next;
           fast = fast.next.next;
       }
       return slow;
   }
   ```

### Merged Intervals

1. Merge Interval \(56\)
   1. Tricky solution: [https://leetcode.com/problems/merge-intervals/discuss/351533/A-Java-sorting-solution](https://leetcode.com/problems/merge-intervals/discuss/351533/A-Java-sorting-solution)
   2. Normal solution

      ```java
      public int[][] merge(int[][] intervals) {
        // 先按照区间起始位置排序
        Arrays.sort(intervals, (v1, v2) -> v1[0] - v2[0]);
        // 遍历区间
        int[][] res = new int[intervals.length][2];
        int idx = -1;
        for (int[] interval : intervals) {
          // 如果结果数组是空的，或者当前区间的起始位置 > 结果数组中最后区间的终止位置，
          // 则不合并，直接将当前区间加入结果数组。
          if (idx == -1 || interval[0] > res[idx][1]) {
            res[++idx] = interval;
          } else {
            // 反之将当前区间合并至结果数组的最后区间
            res[idx][1] = Math.max(res[idx][1], interval[1]);
          }
        }
        return Arrays.copyOf(res, idx + 1);
      }
      ```
2. Insert Interval

   ```java
   public int[][] insert(int[][] intervals, int[] newInterval) {
       int start = newInterval[0];
       int end = newInterval[1];
       List<int[]> list = new ArrayList<>();
       for (int[] interval : intervals) {
         int curStart = interval[0];
         int curEnd = interval[1];
         if (curEnd < start) {
           list.add(new int[] { curStart, curEnd });
         } else if (curStart > end) {
           list.add(new int[] { start, end });
           start = curStart;
           end = curEnd;
         } else {
           start = Math.min(start, curStart);
           end = Math.max(end, curEnd);
         }
       }
       list.add(new int[] { start, end });
       int[][] res = new int[list.size()][2];
       for (int i = 0; i < list.size(); i++) {
         res[i][0] = list.get(i)[0];
         res[i][1] = list.get(i)[1];
       }
       return res;
     }

   ```

3. Intervals Intersection

   ```java
   public int[][] intervalIntersection(int[][] A, int[][] B) {
     if (A == null || A.length == 0 || B == null || B.length == 0)
       return new int[][] {};
     List<int[]> res = new ArrayList<>();

     int i = 0, j = 0;
     int startMax, endMin;
     while (i < A.length && j < B.length) {
       startMax = Math.max(A[i][0], B[j][0]);
       endMin = Math.min(A[i][1], B[j][1]);

       if (endMin >= startMax)
         res.add(new int[] { startMax, endMin });

       if (A[i][1] == endMin)
         i++;
       if (B[j][1] == endMin)
         j++;
     }

     return res.toArray(new int[res.size()][2]);
   }
   ```

4. Conflicting Appointments \(435\)

   ```java
   public int eraseOverlapIntervals(int[][] intervals) {
       if (intervals.length == 0) {
         return 0;
       }
       Arrays.sort(intervals, (a, b) -> (a[0] - b[0]) != 0 ? a[0] - b[0] : b[1] - a[1]);
       int res = 1;

       int prev = intervals.length - 1;

       for (int i = intervals.length - 2; i >= 0; i--) {
         int[] interval = intervals[i];

         // System.out.println("[" + interval[0] + "," + interval[1] + "]");
         // System.out.println(interval[1]);
         // if (prev == -1) {
         // prev = i;
         // continue;
         // }

         int[] prevInterval = intervals[prev];

         if (interval[1] <= prevInterval[0]) {
           res += 1;
           prev = i;
           // System.out.println(i);
           // System.out.println(res);
           // prev--;
         }
       }

       return intervals.length - res;

     }
  
     public int eraseOverlapIntervals(Interval[] intervals) {
       if (intervals.length == 0)  return 0;

       Arrays.sort(intervals, new myComparator());
       int end = intervals[0].end;
       int count = 1;        

       for (int i = 1; i < intervals.length; i++) {
           if (intervals[i].start >= end) {
               end = intervals[i].end;
               count++;
           }
       }
       return intervals.length - count;
   }

   class myComparator implements Comparator<Interval> {
       public int compare(Interval a, Interval b) {
           return a.end - b.end;
       }
   }
   ```

   1. The following [greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm) does find the optimal solution:
      1. Select the interval, x, with the earliest **finishing time**.
      2. Remove x, and all intervals intersecting x, from the set of candidate intervals.
      3. Repeat until the set of candidate intervals is empty.
   2. Whenever we select an interval at step 1, we may have to remove many intervals in step 2. However, all these intervals necessarily cross the finishing time of x, and thus they all cross each other \(see figure\). Hence, at most 1 of these intervals can be in the optimal solution. Hence, for every interval in the optimal solution, there is an interval in the greedy solution. This proves that the greedy algorithm indeed finds an optimal solution.
   3. [56 Merge Intervals](https://leetcode.com/problems/merge-intervals)  [252 Meeting Rooms](https://leetcode.com/problems/meeting-rooms/) [253 Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) [452 Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

### Cyclic Sort

1. Cyclic Sort
2. First Missing Positive \(41\)

   ```java
   public int firstMissingPositive(int[] nums) {
       int n = nums.length;
       //遍历每个数字
       for (int i = 0; i < n; i++) {
           //判断交换回来的数字
           while (nums[i] > 0 && nums[i] <= n && nums[i] != nums[nums[i] - 1]) {
               //第 nums[i] 个位置的下标是 nums[i] - 1
               swap(nums, i, nums[i] - 1);
           }
       }
       //找出第一个 nums[i] != i + 1 的位置
       for (int i = 0; i < n; i++) {
           if (nums[i] != i + 1) {
               return i + 1;
           }
       }
       //如果之前的数都满足就返回 n + 1
       return n + 1;
   }

   private void swap(int[] nums, int i, int j) {
       int temp = nums[i];
       nums[i] = nums[j];
       nums[j] = temp;
   }
   ```

   对于这种要求空间复杂度的，我们可以先考虑如果有一个等大的空间，我们可以怎么做。然后再考虑如果直接用原数组怎么做，主要是要保证数组的信息不要丢失。目前遇到的，主要有两种方法就是交换和取相反数。

3. Finding the Missing Number \(268\)

   ```java
   public int missingNumber(int[] nums) {

     int xor = 0, i = 0;
   	for (i = 0; i < nums.length; i++) {
   		xor = xor ^ i ^ nums[i];
   	}

   	return xor ^ i;
   }

   public static int missingNumber(int[] nums) {
       int sum = nums.length;
       for (int i = 0; i < nums.length; i++)
           sum += i - nums[i];
       return sum;
   }
   ```

4. Find all Missing Numbers \(448\)

   ```java
   public List<Integer> findDisappearedNumbers(int[] nums) {
     List<Integer> res = new ArrayList<>();
     int n = nums.length;
     for (int i = 0; i < n; i++) {
       int num = Math.abs(nums[i]);
       if (nums[num - 1] > 0) {
         nums[num - 1] = -nums[num - 1];
       }
     }

     for (int i = 0; i < n; i++) {
       if (nums[i] > 0) {
         res.add(i + 1);
       }
     }

     return res;
   }
   ```

5. Find the Duplicate Number \(287\)

   ```java
   public int findDuplicate1(int[] nums) {
     int res = -1;

     int i = 0;

     while (i < nums.length) {
       if (nums[i] == i + 1) {
         i++;
         continue;
       }

       if (nums[nums[i] - 1] == nums[i]) {
         return nums[i];
       }

       swap(nums, i, nums[i] - 1);

     }

     return res;

   }

   public void swap(int[] nums, int i, int j) {
     int temp = nums[i];
     nums[i] = nums[j];
     nums[j] = temp;
   }
   ```

   Other solutions: binary sort, slow fast pointers, binary representation;

6. Find all Duplicate Numbers

   ```java
   public List<Integer> findDuplicates(int[] nums) {
       List<Integer> res = new ArrayList<>();
       for (int i = 0; i < nums.length; ++i) {
           int index = Math.abs(nums[i])-1;
           if (nums[index] < 0)
               res.add(Math.abs(index+1));
           nums[index] = -nums[index];
       }
       return res;
   }
   ```

7. Kth missing positive number \(1539\)

   ```java
   public int findKthPositive(int[] A, int k) {
     int l = 0, r = A.length, m;
     while (l < r) {
       m = (l + r) / 2;
       if (A[m] - 1 - m < k)
         l = m + 1;
       else
         r = m;
     }
     return l + k;
   }
   ```

   l: index where when we add 5 will always cover the last l numbers in array. l+k shouldn't be in nums and that why we have the `<` sign in if condition

### In-Place Reversal of a LinkdedList

1. Reverse a LinkedList
2. Reverse a Sub-list
3. Reverse every K-element Sub-list







