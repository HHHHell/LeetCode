## 2018/3/19-3/20

1. Majority Element

   ```java
   class Solution {
       public int majorityElement(int[] nums) {
           int major = nums[0];
           int cnt = 0;
            for(int i = 0; i < nums.length; i++)
            {
                if(nums[i] == major)
                    cnt++;
                else if(cnt == 0)
                    major = nums[i];
                else 
                    cnt--;
            }
           return major;
       }
   }
   ```

2. **Reverse Linked List**

   ```java
   Class Soution{
       public ListNode reverseList(ListNode head){
           ListNode newhead = null;
           while(head != null){
               ListNode tmp = head.next;
               head.next = newHead;
               newHead = head;
               head = tmp;
           }
           return newHead;
   	}
   }
   ```

3. Diameter of Binary Tree

   diameter = max (depth of left subtree + depth of right subtree) of all nodes

   ```java
   Class Solution{
       int max = 0;
       public int diameterOfBinaryTree(TreeNode root){
           depthOfTrr(root);
           return max;
       }
       
       public int depthOfTree(TreeNode root){
   		if(root == null)
               return 0;
           int left = depthOfTree(root.left);
           int right = depthOfTree(root.right);
           max = Math.max(max, left + right);
           return Math.max(left, right) + 1;
       }
   }
   ```

4. Best Time to Buy and Sell Stock

   - same as **maximum subarray problem**, solving with **Kadane' algorithm**: O(n) time, O(1) space.

     to begin with a productive question, if knowing maximum of subarray ending at index i (M[i], what is maximum of subarray ending at index i+1(M[i+1])? The answer turns out to be relatively straightforward: either the maximum subarray sum ending at position  i+1 includes the maximum subarray sum ending at position i as a prefix, or it doesn't.

   ```java
   Class Solution{
       public int maxProfit(int[] prices){
           if(prices.length < 1)
               return 0;
           int max = 0, maxSoFar = 0;
           for(int i = 1; i < prices.length; i++){
               max = Math.max(max+prices[i]-prices[i-1], prices[i]-prices[i-1]);
               maxSoFar = Math.max(max, maxSoFar);
           }
           return maxSoFar;
       }
   }
   ```

   - another solution: also O(n) time, O(1) space.

   ```java
   Class Solution{
       public int maxProfit(int[] prices){
           if(prices.length < 1)
               return 0;
           int min = prices[0], max = 0;
           for(int i = 0; i < prices.length; i++){
               min = Math.min(min, prices[i]);
               max = Math.max(max, prices[i]-min);
           }
           return max;
       }
   }
   ```

5. Climbing stairs

   **dynamic programming**:O(n) time, O(1) space

   number of ways to n stair = number of ways to (n-1) stair + number of ways to (n-2) stair

   ```java
   class Solution {
       public int climbStairs(int n) {
           if(n == 0)
               return 0;
           if(n == 1)
               return 1;
           if(n == 2)
               return 2;
           int nStep = 2, preStep = 2, ppreStep = 1, i = 2;
           while(i < n){
               nStep = preStep + ppreStep;
               ppreStep = preStep;
               preStep = nStep;
               i++;
           }
           return nStep;
       }
   }
   ```

6. Merge Two Sorted Lists

   recursion solution

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) { val = x; }
    * }
    */
   class Solution {
       public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
           if(l1 == null)
               return l2;
           else if(l2 == null)
               return l1;
           if(l1.val < l2.val){
               l1.next = mergeTwoLists(l1.next, l2);
               return l1;
           }
           else{
               l2.next = mergeTwoLists(l2.next, l1);   
               return l2;
           }
       }
   }
   ```

7. Symmetric Tree

   for each node in a symmetric tree, left == right && left.left ==right,right && left.right == right.left

   recursion solution

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
       public boolean isSymmetric(Treeode root){
           if(root == null)
               return true;
           return root.right == root.left && helper(root.right, root.left);
       }
       public boolean helper(TreeNode left, TreeNode right){
           if(left == null && right == null)
               return true;
           if(left == null || right == null)
               return false;
           return left.val == right.val && helper(right.right, left.left) && helper(right.left, left.right);
       }
   }
   ```

   iteration solution with **stack**

   ```java
   Class Solution{
           public boolean iterSolution(TreeNode root){
           if(root == null)
               return true;
           Stack<TreeNode> stack = new Stack<TreeNode>();
           stack.push(root.right);
           stack.push(root.left);
           while(!stack.empty()){
               TreeNode right = stack.pop();
               TreeNode left = stack.pop();
               if(right == null && left == null)
                   continue;
               if(right == null || left == null || right.val != left.val)
                   return false;
               
               stack.push(right.left);
               stack.push(left.right);
               stack.push(right.right);
               stack.push(left.left);
           }
           return true;
       }
   }
   ```

8. Maximum Subarray

   **Kadane's algorithm**, same with Best Time to Buy and Sell Stock

   ```java
   class Solution {
       public int maxSubArray(int[] nums) {
           if(nums.length < 1)
               return 0;
           
           int maxHere = nums[0], maxSoFar = nums[0];
           for(int i = 1; i < nums.length; i++)
           {
               maxHere = Math.max(nums[i], maxHere + nums[i]);
               maxSoFar = Math.max(maxHere, maxSoFar);
           }
           return maxSoFar;
       }
   }
   ```

9. Subtree of Another Tree

   recursion

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
       public boolean isSubtree(TreeNode s, TreeNode t) {
           // t is non-empty
           if(s == null)
               return false;
           
           if(s.val == t.val && isEquals(s, t))
               return true;
           return isSubtree(s.left, t) || isSubtree(s.right, t);
       }
       
       public boolean isEquals(TreeNode s, TreeNode t){
           if(s == null || t == null)
               return t == s;
           return t.val == s.val && isEquals(s.left, t.left) && isEquals(s.right, t.right);
       }
   }
   ```

10. Path Sum III

    almost same with Subtree of Another Tree

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
        public int pathSum(TreeNode root, int sum) {
            if(root == null)
                return 0;
            return isEquals(root, sum) + pathSum(root.right, sum) + pathSum(root.left, sum);
        }
        
        public int isEquals(TreeNode root, int tar){
            if(root == null)
                return 0;
            if(root.val == tar)
                return 1 + isEquals(root.left, tar - root.val) + isEquals(root.right, tar - root.val);
            return isEquals(root.left, tar - root.val) + isEquals(root.right, tar - root.val);
        }
    }
    ```

11. House Robber

    **dynamic programming**: 

    max_profit(i) = max(max_profit(i-2)+house(i), max_profit(i-1))

    ```java
    class Solution {
        public int rob(int[] nums) {
            if(nums.length == 0)
                return 0;
            if(nums.length == 1)
                return nums[0];
            if(nums.length == 2)
                return Math.max(nums[0], nums[1]);
            
            int now = 0;
            int pre = Math.max(nums[1], nums[0]);
            int ppre = nums[0];
            
            for(int i = 2; i < nums.length; i++){
                now = Math.max(ppre+nums[i], pre);
                ppre = pre;
                pre = now;
            }
            
            return now;
        }
    }
    ```

12. Valid Parentheses

    use stack

    ```java
    class Solution {
    public:
        bool isValid(string s) {
            stack<char> mystack;
            for(int i = 0; i < s.size(); i++){
                char t1 = s[i];
                if(t1 == '{' || t1 == '[' || t1 == '(' || mystack.empty())
                    mystack.push(t1);
                else{
                    char t2 = mystack.top();
                    mystack.pop();
                    if((t2 == '{' && t1 != '}') || (t2 == '[' && t1 != ']') || (t2 == '(' && t1 != ')'))
                        return false;
                }
            }
            if(mystack.empty())
                return true;
            return false;
        }
    };
    ```

13. **Find All Anagrams in a String**

    sliding window plate algorithm:https://leetcode.com/problems/find-all-anagrams-in-a-string/discuss/92007/Sliding-Window-algorithm-template-to-solve-all-the-Leetcode-substring-search-problem.

    ```java
    class Solution {
        public List<Integer> findAnagrams(String s, String p){
            ArrayList<Integer> res = new ArrayList<Integer>();
            int[] map = new int[256];
            for(int i = 0; i < p.length(); i++){
                map[p.charAt(i)]++;
            }
            
            int right = 0, left = 0, counter = p.length();
            while(right < s.length()){
                if(map[s.charAt(right++)]-- >= 1)
                    counter--;
                if(counter == 0 && right - left == p.length())
                    res.add(left);
                if(right - left == p.length() && map[s.charAt(left++)]++ >= 0)
                    counter++;
            }
            
            return res;
        }
    }
    ```

    solution two:

    ```java
    class Solution {
        public List<Integer> findAnagrams(String s, String p) {
            List<Integer> res = new ArrayList<Integer>();
            if(s == null)
                return res;
            HashMap<Character, Integer> map = new HashMap<Character, Integer>();
            char[] charList = p.toCharArray();
            for(int i = 0; i < charList.length; i++){
                char tmp = charList[i];
                if(map.containsKey(tmp))
                    map.put(tmp, map.get(tmp)+1);
                else
                    map.put(tmp, 1);
            }
            
            int right = 0, left = 0, counter = map.size();
            while(right < s.length()){
                char c = s.charAt(right);
                if(map.containsKey(c)){
                    map.put(c, map.get(c)-1);
                    if(map.get(c) == 0)
                        counter--;
                }
                right++;
                
                while(counter == 0){
                    if(right - left == p.length())
                        res.add(left);

                    char d = s.charAt(left);
                    if(map.containsKey(d)){
                        map.put(d, map.get(d)+1);
                        if(map.get(d) > 0)
                            counter++;
                    }
                    left++;
                }
            }
            
            return res;
        }
    }
    ```

14. ​


