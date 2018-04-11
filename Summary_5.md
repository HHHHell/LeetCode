1. 16: 3Sum Closest

   Almost same to 3Sum, sort, choose on number then binary search the other 2 numbers

   ```java
   class Solution {
       public int threeSumClosest(int[] nums, int target) {
           Arrays.sort(nums);
           int res = Integer.MAX_VALUE;
           int resSum = 0;
           
           if(nums.length <= 3){
               int s = 0;
               for(int i = 0; i < nums.length; i++)
                   s += nums[i];
               return s;
           }
           
               
           for(int i = 0; i < nums.length - 2; i++)
           {            
               int hi = nums.length - 1;
               int lo = i + 1;
               int min = Integer.MAX_VALUE;
               int minSum = 0;
               
               while(lo < hi)
               {
                   if(min > Math.abs(target - nums[i] - nums[lo] - nums[hi])){
                       min = Math.abs(target - nums[i] - nums[lo] - nums[hi]);
                       minSum = nums[i] + nums[lo] + nums[hi];
                   }
                   
                   if(nums[lo] + nums[hi] + nums[i] > target)
                       hi--;
                   else
                       lo++;
               }
               if(res > min){
                   res = min;
                   resSum = minSum;
               }
           }
           return resSum;
       }    
   }
   ```

2. 717: 1-bit and 2-bit Characters

   2-bit characters must begin with 1.

   ```java
   class Solution {
       public boolean isOneBitCharacter(int[] bits) {
           int lastOne = bits.length - 1, beforeZero = lastOne;
           boolean flag = false, zeroFlag = false;
           
           for(int j = bits.length - 1; j > -1; j--){
               if(bits[j] == 1 && !flag){
                   flag = true;
                   lastOne = j;
               }
               else if(bits[j] == 0 && flag){
                   beforeZero = j;
                   zeroFlag = true;
                   break;
               }
           }
           if(!flag)
               return true;
           
           if(!zeroFlag){
               beforeZero = -1;
           }
           return (lastOne-beforeZero)%2 == 0 || ((lastOne-beforeZero)%2 != 0 && (bits.length - 1 - lastOne > 1));
       }
   }
   ```

   another method, more simple and concise.

   ```java
   class Solution{
       public boolean isOneBitCharacter(int[] bits){
           int i = 0;
           while(i < bits.length-1){
               if(bits[i] == 1)
               	i+=2;
               else
               	i++;
           }
           return i==bits.length-1;
       }
   } 
   ```

3. 747: Largest Number At Least Twice of Others

   ```java
   class Solution {
       public int dominantIndex(int[] nums) {
           int max = Integer.MIN_VALUE;
           int secondMax = Integer.MIN_VALUE;
           int res = 0;
           
           for(int i = 0; i < nums.length; i++){
               int num = nums[i];
               if(num > max){
                   secondMax = max;
                   max = num;
                   res = i;
               }
               else if(num > secondMax)
                   secondMax = num;
           }
           
           if(max >= 2*secondMax)
               return res;
           return -1;
       }
   }
   ```

4. 746: Min Cost Climbing Stairs 

   use DP

   ```java
   class Solution {
       public int minCostClimbingStairs(int[] cost) {
           int[] stepCost = new int[cost.length];
           for(int i = cost.length-1; i > -1; i--){
               if(i == cost.length-1)
                   stepCost[i] = cost[i];
               else if(i == cost.length-2)
                   stepCost[i] = cost[i];
               else
                   stepCost[i] = cost[i] + Math.min(stepCost[i+1], stepCost[i+2]);
           }
           return Math.min(stepCost[0], stepCost[1]);
       }
   }
   ```

5. 167: Two Sum II - Input array is sorted

   Binary search

   ```java
   class Solution {
       public int[] twoSum(int[] numbers, int target) {
           int lo = 0, hi = numbers.length-1;
           while(lo < hi){
               if(numbers[lo] + numbers[hi] == target)
                   return new int[]{lo+1, hi+1};
               else if(numbers[lo] + numbers[hi] < target)
                   lo++;
               else
                   hi--;
           }
           return null;
       }
   }
   ```

6. 729: My Calendar I

   use TreeMap

   ```java
   class MyCalendar {
       TreeMap<Integer, Integer> book;
       
       public MyCalendar() {
           book = new TreeMap<Integer, Integer>();
       }
       
       public boolean book(int start, int end) {
           Map.Entry<Integer, Integer> floor = book.floorEntry(start);
           Map.Entry<Integer, Integer> ceil = book.ceilingEntry(start);
           
           if(floor == null || (floor != null && floor.getValue() <= start)){
               if(ceil == null || (ceil != null && ceil.getKey() >= end)){
                   book.put(start, end);
                   return true;
               }
           }
           return false;
       }
   }
   ```

7. 731: My Calendar II

   use Mycalendar I to save overlaps

   ```java
   class MyCalendarTwo {
       ArrayList<int[]> events;
       public MyCalendarTwo() {
           events = new ArrayList<int[]>();
       }
       
       public boolean book(int start, int end) {
           MyCalendar myCalendar = new MyCalendar();
           for(int[] event : events){
               int maxS = Math.max(start, event[0]), minE = Math.min(end, event[1]);
               if(maxS < minE && !myCalendar.book(maxS, minE)){
                   return false;
               }
           }
           events.add(new int[]{start, end});
           return true;
       }
       
       class MyCalendar{
           TreeMap<Integer, Integer> book = new TreeMap<Integer, Integer>();
           
           public boolean book(int start, int end){
               Map.Entry<Integer, Integer> floor = book.floorEntry(start);
               Map.Entry<Integer, Integer> ceil = book.ceilingEntry(start);
           
               if(floor == null || (floor != null && floor.getValue() <= start)){
                   if(ceil == null || (ceil != null && ceil.getKey() >= end)){
                       book.put(start, end);
                       return true;
                   }
               }
               return false;        
           }
       }
   }
   ```

8. 697: Degree of An Array

   use 2 maps

   ```java
   public int findShortestSubArray(int[] nums) {
       int degree = 0, n = nums.length, minSize = n;
       Map<Integer, Integer> map = new HashMap<>();
       Map<Integer, Integer[]> map2 = new HashMap<>();
       
       for (int i=0;i<n;i++) {
           map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
           degree = Math.max(degree, map.get(nums[i]));

           if (map2.get(nums[i]) == null) map2.put(nums[i], new Integer[2]);
           Integer[] numRange = map2.get(nums[i]);
           if (numRange[0] == null) numRange[0] = i;
           numRange[1] = i;
       }

       for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
           if (entry.getValue() != degree) continue;
           Integer[] range = map2.get(entry.getKey());
           minSize = Math.min(minSize, range[1] - range[0] + 1);  
       }
       return minSize;
   }
   ```

9. 695: Max Area of Island

   ```java
   class Solution {
       public int maxAreaOfIsland(int[][] grid) {
           if(grid.length == 0)
               return 0;
           int max = 0;
           for(int i = 0; i < grid.length; i++){
               for(int j = 0; j < grid[i].length; j++){
                   if(grid[i][j] == 1)
                       max = Math.max(max, countArea(grid, i, j));
               }
           }
           return max;
       }
       
       public int countArea(int[][] grid, int x, int y){
           int ylen = grid[0].length, xlen = grid.length;
           if(x >= xlen || y >= ylen || x < 0 || y < 0)
               return 0;
           if(grid[x][y] == 0)
               return 0;
           grid[x][y] = 0;
           int area = 1+countArea(grid, x, y+1)+countArea(grid, x+1, y)+countArea(grid, x-1, y)+countArea(grid, x, y-1);
           return area;
       }
   }
   ```

10. 162: Find Peak Element

    use binary search to achieve O(logn) time

    ```java
    class Solution {
        public int findPeakElement(int[] nums) {
            int mid = nums.length/2, lo = 0, hi = nums.length - 1;
            
            while(true){
                if(hi == lo)
                    return lo;
                if(hi == lo + 1)
                    return nums[lo] > nums[hi] ? lo : hi;
                
                if(nums[mid] > nums[mid-1] && nums[mid] > nums[mid+1])
                    return mid;
                
                if(nums[mid] > nums[mid-1] && nums[mid] < nums[mid+1]){
                    lo = mid;
                    mid = (mid + hi)/2;
                }
                else{
                    hi = mid;
                    mid = (mid + lo)/2;
                }
            }
        }
    }
    ```

11. Maximum Swap

    O(n) time

    ```java
    class Solution {
        public int maximumSwap(int num) {
            char[] digit = Integer.toString(num).toCharArray();
            int[] bucket = new int[10];
            
            for(int i = 0; i < digit.length; i++){
                bucket[digit[i] - '0'] = i;
            }
            
            for(int i = 0; i < digit.length; i++){
                for(int j = 9; j > digit[i] - '0'; j--){
                    if(bucket[j] > i){
                        char tmp = digit[i];
                        digit[i] = (char)('0' + j);
                        digit[bucket[j]] = tmp;
                        
                        return Integer.valueOf(new String(digit));
                    }
                }
            }
            return num;
        }
    }
    ```

12. Longest Continuous Increasing Subsequence

    ```java
    class Solution {
        public int findLengthOfLCIS(int[] nums) {
            int max = 0, j = 0;
            for(int i = 0; i < nums.length; i++){
                if((i != 0 && nums[i] > nums[i-1]) || j == 0)
                    j++;
                else{
                    max = Math.max(max, j);
                    j = 1;
                }
            }
            max = Math.max(max, j);
            return max;
        }
    }
    ```

    ​