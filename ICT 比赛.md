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

