### 20210702

My solution

    class Solution {
        public int maxProduct(int[] nums) {
            int n = nums.length;
            if (n == 1) {
                return nums[0];
            }
            
            int[] positiveMemo = new int[n];
            int[] negativeMemo = new int[n];
            
            if (nums[0] >= 0) {
                positiveMemo[0] = nums[0];
            } else {
                negativeMemo[0] = nums[0];
            }
            
            for (int i = 1; i < n; i++) {
                if (nums[i] > 0) {
                    positiveMemo[i] = positiveMemo[i - 1] == 0 ? nums[i] : positiveMemo[i - 1] * nums[i];
                    
                    negativeMemo[i] = negativeMemo[i - 1] * nums[i];
                } else if (nums[i] < 0){
                    positiveMemo[i] = negativeMemo[i - 1] * nums[i];
                    
                    negativeMemo[i] = positiveMemo[i - 1] == 0 ? nums[i] : positiveMemo[i - 1] * nums[i];
                }
            }
            
            int max = positiveMemo[0];
            for (int i : positiveMemo) {
                if (i > max)
                    max = i;
            }
            
            return max;
        }
    }

看了答案后的一点改进，思路没有变化其实

    class Solution {
        public int maxProduct(int[] nums) {
            int n = nums.length;
            
            int maxSoFar = nums[0];
            int minSoFar = nums[0];
            
            int result = maxSoFar;
            for (int i = 1; i < n; i++) {
                int curr = nums[i];
                
                int tmpMax = Math.max(curr, Math.max(maxSoFar * curr, minSoFar * curr));
                minSoFar = Math.min(curr, Math.min(maxSoFar * curr, minSoFar * curr));
                maxSoFar = tmpMax;
                
                result = Math.max(result, maxSoFar);
            }
            
            return result;
        }
    }