## 2018/3/21

1. Min Stack

   If you want to push x to stack, save (x-min) in stack actually.

   ```java
   class MinStack {
       /** initialize your data structure here. */
       long min;
       Stack<Long> stack;
       
       public MinStack() {
           min = Integer.MAX_VALUE;
           stack = new Stack<Long>();
       }
       
       public void push(int x) {
           stack.push((long)x-min);
           min = Math.min(min, x);
       }
       
       public void pop() {
           long x = stack.pop();
           min = Math.max(min-x, min);
       }
       
       public int top() {
           return (int)Math.max(stack.peek() + min, min);
       }
       
       public int getMin() {
           return (int)min;
       }
   }
   ```

2. Intersection of Two Linked Lists

   steps of p1 = length of headA + lengh of headB = steps of p2, and last n steps in their paths are same.

   ```java
   /**
    * Definition for singly-linked list.
    * public class ListNode {
    *     int val;
    *     ListNode next;
    *     ListNode(int x) {
    *         val = x;
    *         next = null;
    *     }
    * }
    */
   public class Solution {
       public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
           if(headA == null || headB == null)
               return null;
           ListNode p1=headA, p2 = headB;
           
           while(p1 != p2){
               if(p1 == null)
                   p1 = headB;
               else
                   p1 = p1.next;
               if(p2 == null)
                   p2 = headA;
               else
                   p2 = p2.next;
           }
           return p1;
       }
   }
   ```

3. Counting Bits

   number of bit n = number of bit (n/2) + n%2

   ```java
   class Solution {
       public int[] countBits(int num) {
           int[] res = new int[num+1];
           for(int i = 1; i < num + 1; i++){
               res[i] = res[i/2] + i%2;
           }
           return res;
       }
   }
   ```

4. Queue Reconstruction by Height

   sort by height and k, then insert to list

   ```java
   class Solution {
       public int[][] reconstructQueue(int[][] people) {
           if(people.length == 0)
               return people;
           Arrays.sort(people, new Comparator<int[]>(){
               public int compare(int[] a, int[] b){
                   if(a[0] == b[0])
                       return a[1] - b[1];
                   else
                       return b[0] - a[0];
               }
           });
           
           ArrayList<int[]> tmp = new ArrayList<int[]>();
           for(int i = 0; i < people.length; i++){
               tmp.add(people[i][1], new int[]{people[i][0], people[i][1]});
           }
           
           int[][] res = new int[people.length][2];
           for(int i = 0; i < tmp.size(); i++){
               res[i][0] = tmp.get(i)[0];
               res[i][1] = tmp.get(i)[1];            
           }
           
           return res;
       }
   }
   ```

5. Palindromic Substrings

   extend every char

   ```java
   class Solution {
       int sum = 0;
       public int countSubstrings(String s) {
           if(s == null || s.length() == 0)
               return  0;
           
           for(int i = 0; i < s.length(); i++){
               extend(i, i, s);
               extend(i, i + 1, s);
           }
           
           return sum;
       }
       
       public void extend(int left, int right, String s){
           while(left > -1 && right < s.length() && s.charAt(right) == s.charAt(left)){
               sum++;
               left--;
               right++;
           }
       }
   }
   ```

6. Top K Frequent Elements

   hash map

   ```java
   class Solution {
       public List<Integer> topKFrequent(int[] nums, int k) {
           HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
           for(int i = 0; i < nums.length; i++){
               map.put(nums[i], map.getOrDefault(nums[i], 0)+1);
           }
           
           List<Integer>[] bucket = new List[nums.length + 1];
           
           for(int key : map.keySet()){
               int fre = map.get(key);
               if(bucket[fre] == null)
                   bucket[fre] = new ArrayList();
               bucket[fre].add(key);
           }
           
           List<Integer> res = new ArrayList<Integer>();
           for(int i = bucket.length -1; i > -1 && res.size() < k; i--){
               if(bucket[i] != null){
                   res.addAll(bucket[i]);
               }
           }
           return res;
       }
   }
   ```

7. Binary Tree Inorder traversal

   stack, push all left nodes firstly, then pop top element, visit it and it's right node

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
       List<Integer> res = new ArrayList();
       public List<Integer> inorderTraversal(TreeNode root) {
           //traversal(root);
           iterative(root);
           return res;
       }
       
       public void traversal(TreeNode root){
           if(root == null)
               return;
           traversal(root.left);
           res.add(root.val);
           traversal(root.right);
       }
       
       public void iterative(TreeNode root){
           Stack<TreeNode> stack = new Stack<TreeNode>();
           TreeNode p = root;
           while(!stack.empty() || p != null){
               while(p != null){
                   stack.push(p);
                   p = p.left;
               }
               p = stack.pop();
               res.add(p.val);
               p = p.right;
           }
       }
   }
   ```