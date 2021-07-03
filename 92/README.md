### 2021.6.28

第一次做

    class Solution {
        public ListNode reverseBetween(ListNode head, int left, int right) {
            ListNode dummy = new ListNode(-1, head);
            
            ListNode leftNodePrev = dummy;
            ListNode leftNode = head;
            for (int i = 1; i < left; i++) {
                leftNode = leftNode.next;
                leftNodePrev = leftNodePrev.next;
            }
            
            ListNode curr = leftNode;
            ListNode next = curr.next;
            for (int i = left; i < right && next != null; i++) {
                ListNode nextnext = next.next;
                next.next = curr;
                curr = next;
                next = nextnext;
            }
            
            leftNodePrev.next = curr;
            leftNode.next = next;
            
            return dummy.next;
        }
    }

看了答案后做的

    class Solution {
        private ListNode left;
        private boolean stop;
        
        public ListNode reverseBetween(ListNode head, int left, int right) {
            this.left = head;
            
            reverse(head, left, right);
            
            return head;
        }
        
        private void reverse(ListNode right, int m, int n) {
            if (n == 1) {
                return;
            }
            
            right = right.next;
            
            if (m > 1) {
                left = left.next;
            }
            
            reverse(right, m - 1, n - 1);
            
            if (left == right || right.next == left) {
                this.stop = true;
            }
            
            if (!this.stop) {
                int tmp = this.left.val;
                this.left.val = right.val;
                right.val = tmp;
                
                left = left.next;
            }
        }
    }