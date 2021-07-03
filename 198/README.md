### 20210702

First Solution (TLE)

    class Solution {
        public int rob(int[] nums) {
            return Math.max(rob(nums, 0), rob(nums, 1));
        }
        
        private int rob(int[] nums, int start) {
            if (start >= nums.length) {
                return 0;
            }
            
            int sum1 = nums[start] + rob(nums, start + 2);
            int sum2 = nums[start] + rob(nums, start + 3);
            
            return Math.max(sum1, sum2);
        }
    }

Adding in memoization (top down)

    class Solution {
        public int rob(int[] nums) {
            Integer[] sumCache = new Integer[nums.length];
            
            return Math.max(rob(nums, 0, sumCache), rob(nums, 1, sumCache));
        }
        
        private int rob(int[] nums, int start, Integer[] sumCache) {
            if (start >= nums.length) {
                return 0;
            }
            
            if (sumCache[start] != null) {
                return sumCache[start];
            }
            
            int sum1 = nums[start] + rob(nums, start + 2, sumCache);
            int sum2 = nums[start] + rob(nums, start + 3, sumCache);
            
            sumCache[start] = Math.max(sum1, sum2);
            
            return sumCache[start];
        }
    }

Replaceing recursion with iteration (bottom up)

    class Solution {
        public int rob(int[] nums) {
            if (nums.length == 1) {
                return nums[0];
            }
            
            int[] cache = new int[nums.length];
            
            for (int i = nums.length - 1; i >= 0; i--) {
                int sum1 = i + 2 >= nums.length ? 0 : cache[i + 2];
                int sum2 = i + 3 >= nums.length ? 0 : cache[i + 3];
                
                cache[i] = nums[i] + Math.max(sum1, sum2);
            }
            
            return Math.max(cache[0], cache[1]);
        }
    }
    



Optimizing space

    class Solution {
        public int rob(int[] nums) {
            int sum1 = 0;
            int sum2 = 0;
            
            int result = 0;
            for (int i = nums.length - 1; i >= 0; i--) {
                result = Math.max(nums[i] + sum2, sum1);
                sum2 = sum1;
                sum1 = result;
            }
            
            return result;
        }
    }