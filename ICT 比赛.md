# ICT 比赛

## 每日总结

### 6/1/2023

1. 层序遍历

   下面的题都看一看

   [按住ctrl点击这里](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.md)

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

2. 