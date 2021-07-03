### 20210702

My solution 1:

    class Solution {
        public void rotate(int[] nums, int k) {
            int n = nums.length;
            k = k % n;
            
            int[] tmp = new int[n];
            System.arraycopy(nums, 0, tmp, k, n - k);
            System.arraycopy(nums, n - k, tmp, 0, k);
            
            System.arraycopy(tmp, 0, nums, 0, n);
        }
    }

Solution 4:

    class Solution {
        public void rotate(int[] nums, int k) {
            k = k % nums.length;
            
            reverse(nums, 0, nums.length - 1);
            reverse(nums, k, nums.length - 1);
            reverse(nums, 0, k - 1);
        }
        
        private void reverse(int[] nums, int start, int end) {
            while (start < end) {
                swap(nums, start, end);
                start++;
                end--;
            }
        }
        
        private void swap(int[] nums, int i, int j) {
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
    }