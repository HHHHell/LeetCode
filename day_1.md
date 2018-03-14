## LeetCode 2018/3/13

1. Hamming Distance

   xor

   ```java
   class Solution {
       public int hammingDistance(int x, int y) {
           int num = x^y, res = 0;
           //return Integer.bitCount(res);
           String str = Integer.toBinaryString(num);
           for(int i = 0; i < str.length(); i++)
           {
               if(str.charAt(i) == '1')
                   res++;
           }
           return res;
       }
   }
   ```

2. Merge Two Binary Trees

   recursion

   ```java
   /**
    * Definition for a binary tree node.
    * struct TreeNode {
    *     int val;
    *     TreeNode *left;
    *     TreeNode *right;
    *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
    * };
    */
   class Solution {
   public:
       TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
           if(t1 == NULL)
               return t2;
           if(t2 == NULL)
               return t1;
           
           TreeNode* root = new TreeNode(t1->val + t2->val);
           root->left = mergeTrees(t1->left, t2->left);
           root->right = mergeTrees(t1->right, t2->right);
           
           return root;
       }
   };
   ```

3. Single Number

   brute force: O(n) time, O(n) space

   xor: O(n)time, O(1) space

   ```java
   class Solution {
       public int singleNumber(int[] nums) {
           int res = 0;
           for(int i =0; i < nums.length; i++) 
               res = res^nums[i];
           return res;
       }
   }
   ```

4. Two Sum

   brute force method: O(n*n) time, O(1) space

   hash map: O(n) time, O(n) space

   ```java
   public class Solution {
       public int[] twoSum(int[] nums, int target) {
           int res[] = new int[2];
           Map<Integer, Integer> map = new HashMap<Integer, Integer>();
           for(int i = 0; i < nums.length; i++)
           {
               if(map.containsKey(target-nums[i]))
               {
                   res[1] = i;
                   res[0] = map.get(target-nums[i]);
                   break;
               }
               map.put(nums[i], i);
           }
           return res;
       }
   }
   ```

5. Linked List Cycle

   Flyod Finding Cycle: two pointers, one moves faster than the other, if there is loop in singly-linked list, they must meet at sometime.

   ```java
   /**
    * Definition for singly-linked list.
    * struct ListNode {
    *     int val;
    *     ListNode *next;
    *     ListNode(int x) : val(x), next(NULL) {}
    * };
    */
   class Solution {
   public:
       bool hasCycle(ListNode *head) {
           ListNode *tortoise = head;
           ListNode *hare = head;
           
           if(head == NULL)
               return false;
           while(hare->next != NULL && hare->next->next != NULL)
           {
               hare = hare->next->next;
               tortoise = tortoise->next;
               if(hare == tortoise)
                   return true;
           }
           return false;
       }
   };
   ```

6. Shortest Unsorted Continuous Subarray

   selection sort: O(n*n) time, O(1) space

   ​

   two pointers method:  O(n) time, O(1) space, awesome!

   ```java
   class Solution {
       public int findUnsortedSubarray(int[] nums) {
           int length = nums.length;
           int begin = -1, end = -2;
           int max = Integer.MIN_VALUE, min = Integer.MAX_VALUE;
           
           for(int i = 0; i < length; i++)
           {
               max = Math.max(max, nums[i]);
               min = Math.min(min, nums[length-i-1]);
               
               if(nums[i] < max)
                   end = i;
               if(nums[length-i-1] > min)
                   begin = length-i-1;
           }
           
           if(begin > end)
               return 0;
           return end-begin+1;
       }
   }
   ```

   end_index = The most right element having greater elements on the left side.
   begin_Index = The most left element having smaller elements on the right side.

   all elements on the right side of end_Index do not have greater elements on the left side and all elements on the left side of the begin_Index do not have greater elements on the right side

   ​

7. Product of Array Except Self

   nums[i]'s product except self = left elements' product * right elements' product

   O(n) time, O(1) space

   ```java
   class Solution {
       public int[] productExceptSelf(int[] nums) {
           int[] res = new int[nums.length];
           int left = 1, right = 1;
           res[0] = 1;
           
           for(int i = 1; i < nums.length; i++)
           {
               left = left*nums[i-1];
               res[i] = left;
           }
           
           for(int j = nums.length - 2; j > -1; j--)
           {
               right = right*nums[j+1];
               res[j] = res[j]*right;
           }
           return res;
       }
   }
   ```

8.  3Sum

   brute force: O(n* n * n) time, or O(n*n) time O(n) space with hash map

   two pointers: O(n*logn) time, quick sort,  then binary search

   ```java
   class Solution {
       public List<List<Integer>> threeSum(int[] nums) {
           Arrays.sort(nums);
           List <List<Integer>> res = new ArrayList();
           for(int i = 0; i < nums.length - 2; i++)
           {
               if(i != 0 && nums[i] == nums[i-1])
                   continue;
               if(nums[i] > 0)
                   break;                
               int hi = nums.length - 1;
               int lo = i + 1;
               int sum = 0 - nums[i];            
               while(lo < hi)
               {
                   if(nums[hi] + nums[lo] == sum)
                   {
                       res.add(Arrays.asList(nums[i], nums[lo], nums[hi]));
                   
                       while(lo < hi && nums[lo + 1] == nums[lo])
                           lo++;
                       lo++;
                       while(lo < hi && nums[hi - 1] == nums[hi])
                           hi--;
                       hi--;
                   }
                   else if(nums[lo] + nums[hi] > sum)
                   {
                       while(lo < hi && nums[hi - 1] == nums[hi])
                           hi--;
                       hi--;
                   }
                   else
                   {
                       while(lo < hi && nums[lo + 1] == nums[lo])
                           lo++;
                       lo++;
                   }
               }
           }
           return res;
       }    
   }
   ```

   ​

   ​
