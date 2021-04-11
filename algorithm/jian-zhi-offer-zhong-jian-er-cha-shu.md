# 剑指offer-重建二叉树

### 1 题目

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

### 2 示例

#### 输入

```text
[1,2,3,4,5,6,7],[3,2,4,1,6,5,7]
```

#### 输出

```text
{1,2,5,3,4,6,7} 
```



### 3 思路

前序遍历：根、左、右；中序遍历：左、根、右

1. 从前序数组获取根的值（rootNodeVal）
2. 寻找对应根值在中序数组中的位置（rootNodeIndex）
3. 根据rootNodeIndex将前序数组和中序数组划分成左、右子树两部分
4. 根据前序、中序所获得的左右子树，采用递归的方式重复上述步骤构建当前rootNode的左、右子节点

### 4 题解

```text
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
import java.util.*;
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        // 判空校验
        if (pre == null || in == null || pre.length == 0 || in.length ==0) {
            return null;
        }
        
        if (in.length == 1) {
            return new TreeNode(in[0]);
        }
        
        // 构建根节点并确定更节点在中序遍历数组中的下表索引
        TreeNode rootNode = new TreeNode(pre[0]);
        Integer rootIndex = getInRootNodeIndex(rootNode, in);
        
        // 取出前序遍历数组中根节点左子树的值
        int[] restLeftPre = Arrays.copyOfRange(pre, 1, rootIndex + 1);
        // 取出前序遍历数组中根节点右子树的值
        int[] restRightPre = Arrays.copyOfRange(pre, rootIndex + 1, pre.length);
        // 取出中序遍历数组中根节点左子树的值
        int[] restLeftIn = Arrays.copyOfRange(in, 0, rootIndex);
        // 取出中序遍历数组中根节点右子树的值
        int[] restRightIn = Arrays.copyOfRange(in, rootIndex + 1, in.length);
        
        // 先递归重建根节点左子树
        rootNode.left = reConstructBinaryTree(restLeftPre, restLeftIn);
        // 再递归重建根节点右子树
        rootNode.right = reConstructBinaryTree(restRightPre, restRightIn);
        
        return rootNode;
    }
    
    private Integer getInRootNodeIndex(TreeNode rootNode, int [] in) {
        Integer rootIndex = null;
        for(int i = 0; i < in.length; i++) {
            if (in[i] == rootNode.val) {
                rootIndex = i;
                break;
            }
        }
        return rootIndex;
    }
}
```



