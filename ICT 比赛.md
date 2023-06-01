# ICT 比赛

copyright

[TOC]



## 每日总结



### 6/1/2023

1. **层序遍历**

   下面的题都看一看

   [点击这里](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.md)

   ```c++
   //层序遍历模板
   class Solution {
   public:
       vector<vector<int>> levelOrder(TreeNode* root) {
           queue<TreeNode*> que;
           if (root != NULL) que.push(root);
           vector<vector<int>> result;
           while (!que.empty()) {
               int size = que.size();
               vector<int> vec;
               // 这里一定要使用固定大小size，不要使用que.size()，因为que.size是不断变化的
               for (int i = 0; i < size; i++) {
                   TreeNode* node = que.front();
                   que.pop();
                   vec.push_back(node->val);
                   if (node->left) que.push(node->left);
                   if (node->right) que.push(node->right);
               }
               result.push_back(vec);
           }
           return result;
       }
   };
   ```

   * 掌握上面的层序遍历模板，所有类似题通吃
   * 注意掌握队列的使用
   * 注意这里空结点是否入队`if (node->left) que.push(node->left);`

   * `size`一定要提前规定
   * 注意求二叉树最小深度，要判断**左右节点都为空**

2. **回溯——组合问题**

   [点击这里](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0077.%E7%BB%84%E5%90%88.md)

   模板如下：

   ```c++
   void backtracking(参数) {
       if (终止条件) {
           存放结果;
           return;
       }
   
       for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
           处理节点;
           backtracking(路径，选择列表); // 递归
           回溯，撤销处理结果
       }
   }
   ```

   ```c++
   class Solution {
   private:
       vector<vector<int>> result;//模板
       vector<int> path;//也是模板
       void backtracking(int n, int k, int startIndex) {
           if (path.size() == k) {	//加入结果集
               result.push_back(path);
               return;
           }
           for (int i = startIndex; i <= n - (k - path.size()) + 1; i++) { // 如果i > n - (k - path.size()) + 1,肯定取不到K个数
               //上面的剪枝等价于
               //if(i > n - (k - path.size()) + 1) break;
               path.push_back(i); // 处理节点
               backtracking(n, k, i + 1);
               path.pop_back(); // 回溯，撤销处理的节点
           }
       }
   public:
   
       vector<vector<int>> combine(int n, int k) {
           backtracking(n, k, 1);//注意这里最后一个参数是1
           return result;
       }
   };
   ```

   

   * 其实回溯套路比较固定的，都是用这个模板

   * 然后就是回溯的每道题，心里都要有下面这颗树，后面所有回溯题，都要根据这棵树写代码

     因为是组合问题，数不能重复取，并且给你的数组已经默认有序，所以树长这样

     <img src="ICT 比赛.assets/image-20230601102810447.png" alt="image-20230601102810447" style="zoom:67%;" />

   

   * 回溯相比暴力循环，就是用空间换时间，用递归栈替代一部分循环
   * 剪枝优化可以不掌握，但是对锻炼思维有帮助






### 6/2/2023


   1. **贪心——用最少数量的箭引爆气球**

      [点击这里](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0452.%E7%94%A8%E6%9C%80%E5%B0%91%E6%95%B0%E9%87%8F%E7%9A%84%E7%AE%AD%E5%BC%95%E7%88%86%E6%B0%94%E7%90%83.md)

   * 先按照左边界排序

   * 如果当前气球左边界>前一个气球右边界，那就肯定要多一支箭了

   * 否则，更新右边界=最小右边界

   * 这题不难，但是需要注意`sort(points.begin(), points.end(), cmp);`这在算法中非常常见

     上面的代码可以替换成下面的代码，就不用写cmp函数了，建议都按下面的写法

     `sort(points.begin(), points.end(), [](int[] a, int[] b)->bool{return a[0] > b[0];})`

     这里用到了[lambda表达式](https://blog.csdn.net/qq_37085158/article/details/124626913)

LAMBDA表达式非常重要！！！！！！！！！！！！！！！！！！！！！！！！！！！！c++的`sort`，`for_each`这些函数都要用到lambda表达式，刷算法题快

大家顺便熟悉下`for_each`, `sort`这些函数

虽然今天只有一道题，但是大家主要还是熟悉lambda表达式，会用就行了





### 6/3/2023

明天是动态规划题目，所以今天先回顾下基础

[点击这里](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.md)

[还有这里](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-2.md)

我们做题基本用滚动数组做，代码简单

我总结一波：

1. 动态规划中，背包问题占很大一部分，所以背包问题要掌握
2. dp初始化很有讲究
3. 根据dp递推公式可以确定遍历顺序
4. 内层循环遍历背包容量：适用于组合问题，因为物品是按顺序考虑的；反过来，内层循环遍历物品，适用于排序问题，因为每一次容量都考虑了所有物品

基础掌握了，有时间看看相关题目，看就行，留个印象，不至于做不出

### 6/4/2023

1. **动态规划——零钱兑换**

   [点击这里](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0322.%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2.md)

   动态规划例题，算法课学过的

   动态规划注意：

   * dp数组初始化
   * 遍历顺序 + 先遍历物品还是先遍历背包
   * dp表达式

   

   这题我拿Java分析，Java和C++差不多的

   ```java
   class Solution {
       public int coinChange(int[] coins, int amount) {
           int max = Integer.MAX_VALUE;
           int[] dp = new int[amount + 1];
           //初始化dp数组为最大值
           for (int j = 0; j < dp.length; j++) {
               dp[j] = max;
           }
           //当金额为0时需要的硬币数目为0
           dp[0] = 0;
           //考虑物品（硬币）i
           for (int i = 0; i < coins.length; i++) {
               //必须正序遍历：完全背包每个硬币可以选择多次
               for (int j = coins[i]; j <= amount; j++) 
                   //j - coins[i]这个容量必须可以被装满，否则就循环下一次
                   if (dp[j - coins[i]] != max) {
                       //选择硬币数目最小的情况
                       dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);//dp[j]每次必须取最小的
                   }
               }
           }
           return dp[amount] == max ? -1 : dp[amount];
       }
   }
   ```

   

   * 动态规划的题目都用到了vector，大家要掌握vector的初始化和成员函数

     这题大家注意：

     1. 为什么dp要初始化成`Integer.MAX_VALUE`？因为`dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);`每次是取最小值
     2. 为什么从前往后遍历背包容量？因为一个硬币可以取多次
     3. 先遍历物品还是先遍历背包？这题没影响
     4. dp递推公式：每次都取最小值

### 6/5/2023

动态规划——判断子序列

这题不是背包问题，简单一（亿）点

[点击这里](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0392.%E5%88%A4%E6%96%AD%E5%AD%90%E5%BA%8F%E5%88%97.md)

1. 注意dp的意义
2. 这种题的切入口就是`s.charAt(i-1) == t.charAt(j-1)`,也就是判断s[i]和t[j]是否相同
3. `dp[i][j] = dp[i][j-1];`而不是`dp[i][j] = dp[i-1][j];`
4. 注意初始化`vector<vector<int>> dp(s.size() + 1, vector<int>(t.size() + 1, 0));`注意+1



### 6/6/2023

下面的题目也都不是背包问题，大家可以做一做

为什么需要强调动态规划，因为好多题都可以用动态规划做

[买卖股票](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0122.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAII%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89.md)

[打家劫舍](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0198.%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D.md)

### 6/7/2023



### 6/8/2023

介绍单调栈，单调栈没啥知识点，就

[点击这里](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0042.%E6%8E%A5%E9%9B%A8%E6%B0%B4.md)

### 6/9/2023











