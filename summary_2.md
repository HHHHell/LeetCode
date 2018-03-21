2018/3/14 - 3/15

1. Maximum Depth of Binary Tree

   recursive function

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode(int x) { val = x; }
    * }
    */
   class Solution {
       public int maxDepth(TreeNode root) {
           if(root == null)
               return 0;
           return Math.max(maxDepth(root.right) + 1, maxDepth(root.left) + 1);
       }
   }
   ```

2. Invert Binary Tree

   recursive function

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode(int x) { val = x; }
    * }
    */
   class Solution {
       public TreeNode invertTree(TreeNode root) {
           if(root == null)
               return root;
           
           TreeNode node = root.left;
           
           root.left = invertTree(root.right);
           root.right = invertTree(node);
           
           return root;
       }
   }
   ```

3. Move Zeroes

   O(n) time

   all the number between cur and lastNoneZero is zero

   ```java
   class Solution {
       public void moveZeroes(int[] nums) {
           for(int cur = 0, lastNoneZero = 0; cur < nums.length; cur++)
           {
               if(nums[cur] != 0)
               {
                   int tmp = nums[cur];
                   nums[cur] = nums[lastNoneZero];
                   nums[lastNoneZero++] = tmp;
               }
           }
       }
   }
   ```

4. Find All Numbers Disappeared in an Array

   use index as flag

   O(n) time, O(1) space

   ```java
   class Solution {
       public List<Integer> findDisappearedNumbers(int[] nums) {
           ArrayList<Integer> res = new ArrayList<Integer>();
           for(int i = 0; i < nums.length; i++)
           {
               int val = Math.abs(nums[i]) -1;
               if(nums[val] > 0)
                   nums[val] = -nums[val];
           }
           
           for(int i = 0; i < nums.length; i++)
           {
               if(nums[i] > 0)
                   res.add(i+1);
           }
           return res;
       }
   }
   ```

5. Convert BST to Greater Tree

   in-order traversal,  calculate cumulative sum from right to lift, then update tree node 

   ```java
   /**
    * Definition for a binary tree node.
    * public class TreeNode {
    *     int val;
    *     TreeNode left;
    *     TreeNode right;
    *     TreeNode(int x) { val = x; }
    * }
    */
   class Solution {
       int sum = 0;
       
       public TreeNode convertBST(TreeNode root) {
           if(root == null)
               return root;
           root.right = convertBST(root.right);
           sum += root.val;
           root.val = sum;
           root.left = convertBST(root.left);
           return root;
       }
   }
   ```

   ​