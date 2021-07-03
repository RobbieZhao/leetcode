### 20210702

My solution

    class Solution {
        public int findKthLargest(int[] nums, int k) {
            PriorityQueue<Integer> pq = new PriorityQueue<>();
            
            for (int num : nums) {
                pq.add(num);
                if (pq.size() > k) {
                    pq.poll();
                }
            }
            
            return pq.peek();
        }
    }

QuickSelect (recursive)

    class Solution {
        private Random ran = new Random();
        
        public int findKthLargest(int[] nums, int k) {
            return quickSelect(nums, 0, nums.length - 1, k);
        }
        
        private int quickSelect(int[] nums, int left, int right, int k) {
            int pivotIndex = ran.nextInt(right + 1 - left) + left;
            int pivot = nums[pivotIndex];
            swap(nums, pivotIndex, right);
            
            int p = left;
            for (int i = left; i < right; i++) {
                if (nums[i] > pivot) {
                    swap(nums, i, p);
                    p++;
                }
            }
            
            swap(nums, p, right);
            
            if (p == k - 1) {
                return nums[p];
            } else if (p > k - 1) {
                return quickSelect(nums, left, p - 1, k);
            } else {
                return quickSelect(nums, p + 1, right, k);
            }
        }
        
        private void swap(int[] nums, int i, int j) {
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
    }

QuickSelect(iterative)

    class Solution {
        private Random ran = new Random();
        
        public int findKthLargest(int[] nums, int k) {
            int left = 0;
            int right = nums.length - 1;
            
            while (left < right) {
                int p = partition(nums, left, right, k);
                if (p == k - 1) {
                    break;
                } else if (p > k - 1) {
                    right = p - 1;
                } else {
                    left = p + 1;
                }
            }
            
            return nums[k - 1];
        }
        
        private int partition(int[] nums, int left, int right, int k) {
            int pivotIndex = ran.nextInt(right + 1 - left) + left;
            int pivot = nums[pivotIndex];
            swap(nums, pivotIndex, right);
            
            int p = left;
            for (int i = left; i < right; i++) {
                if (nums[i] > pivot) {
                    swap(nums, i, p);
                    p++;
                }
            }
            
            swap(nums, p, right);
            
            return p;
        }
        
        private void swap(int[] nums, int i, int j) {
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
    }