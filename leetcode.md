402 84 单调栈

2017 机器人

#### 001.Two sum

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* twoSum(int* nums, int numsSize, int target, int* returnSize){
    *returnSize = 2;
    int *returnArr= (int*) malloc((*returnSize)*sizeof(int));
    //malloc: dynamic memory allocation, return a void *, here int*  is to cast to assign the int #
    for(int i=0; i<(numsSize-1); i++){
        for(int j=i+1; j<numsSize; j++){
            if((nums[i]+nums[j]) == target){
                returnArr[0] = i;
                returnArr[1] = j;
                return returnArr;
            }
        }
    }    
    return returnArr;
}
```

#### 114. Flatten Binary Tree to Linked List

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public void flatten(TreeNode root) {
        if(root == null)        return;

        TreeNode temp = root.left;
        root.left     = root.right;
        root.right    = temp;

        TreeNode cur = root;
        while(cur.right != null)
            cur = cur.right;
        
        cur.right = root.left;
        root.left = null;

        flatten(root.right);
    }
}
//前序遍历 根左右

    1
   / \
  2   5
 / \   \
3   4   6

//将 1 的左子树插入到右子树的地方
    1
  /   \
 5     2         
   \   / \         
    6  3   4                 
//将原来的右子树接到左子树的最右边节点
    1
     \
      2          
     / \          
    3   4  
         \
          5
           \
            6
            
 //将 2 的左子树插入到右子树的地方
    1
     \
      2          
       \          
        3       4  
                 \
                  5
                   \
                    6   
        
 //将原来的右子树接到左子树的最右边节点
    1
     \
      2          
       \          
        3      
         \
          4  
           \
            5
             \
              6         
```



#### 141. Linked List Cycle

```java
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
public boolean hasCycle(ListNode head) {
    if(head == null)    return false;
    ListNode fast = head;
    ListNode slow = head;
    while(fast != null && fast.next != null)
    {
        fast = fast.next.next;
        slow = slow.next;
        if(fast == slow)
            return true;
    }
    return false;
}
```



#### 94. Binary Tree Inorder Traversal

https://leetcode-cn.com/problems/binary-tree-inorder-traversal/

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

class Solution {
    List<Integer> list = new ArrayList<Integer>();//initialize一个list[]
  //List是type ArrayList是class list是object new了一个list 都属于ArrayList这个类
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null)  return list;
        inorderTraversal(root.left);//一直找最左边的node //其实它是一棵树，树以根节点命名
        list.add(root.val);//add是个method java内置
        inorderTraversal(root.right);
        return list;
    }
}
//中序遍历 左根右
```



//后序遍历 左右根

