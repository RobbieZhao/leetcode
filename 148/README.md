### 20210701

Solution 1 merge sort

    class Solution {
        public ListNode sortList(ListNode head) {
            if (head == null || head.next == null) {
                return head;
            }
            
            ListNode middle = getMiddle(head);
            
            ListNode second = sortList(middle.next);
            middle.next = null;
            ListNode first = sortList(head);
            
            return merge(first, second);
        }
        
        private ListNode merge(ListNode a, ListNode b) {
            ListNode dummy = new ListNode(-1);
            ListNode p = dummy;
            
            ListNode p1 = a;
            ListNode p2 = b;
            
            while (p1 != null && p2 != null) {
                if (p1.val < p2.val) {
                    p.next = p1;
                    p1 = p1.next;
                } else {
                    p.next = p2;
                    p2 = p2.next;
                }
                p = p.next;
            }
            
            p.next = p1 == null ? p2 : p1;
            
            return dummy.next;
        }
        
        private ListNode getMiddle(ListNode head) {
            ListNode slow = null;
            ListNode fast = head;
            
            while (fast != null && fast.next != null) {
                fast = fast.next.next;
                slow = slow == null ? head : slow.next;
            }
            
            return slow;
        }
    }

Solution 2 bottom up merge sort

    class Solution {
        private ListNode tail = null;
        private ListNode nextList = null;
        
        public ListNode sortList(ListNode head) {
            if (head == null || head.next == null) {
                return head;
            }
            
            ListNode dummy = new ListNode();
            
            int n = numNodes(head);
            ListNode start = head;
            for (int size = 1; size < n; size = size * 2) {
                tail = dummy;
                
                while (start != null) {
                    ListNode mid = split(start, size);
                    
                    merge(start, mid);
                    
                    start = nextList;
                }
                
                start = dummy.next;
            }
            
            return dummy.next;
        }
        
        private ListNode split(ListNode start, int size) {
            ListNode midPrev = start;        
            for (int i = 1; i < size && midPrev.next != null; i++) {
                midPrev = midPrev.next;
            }
            
            if (midPrev.next == null) {
                nextList = null;
                return null;
            }
            
            ListNode end = midPrev.next;
            for (int i = 1; i < size && end.next != null; i++) {
                end = end.next;
            }
            
            
            ListNode mid = midPrev.next;
            midPrev.next = null;
            
            nextList = end.next;
            end.next = null;
            
            return mid;
        }
        
        private void merge(ListNode a, ListNode b) {
            ListNode dummy = new ListNode();
            ListNode p = dummy;
            
            while (a != null && b != null) {
                if (a.val < b.val) {
                    p.next = a;
                    a = a.next;
                } else {
                    p.next = b;
                    b = b.next;
                }
                p = p.next;
            }
            
            p.next = a == null ? b : a;
            
            while (p.next != null) {
                p = p.next;
            }
            
            tail.next = dummy.next;
            tail = p;
        }
        
        private int numNodes(ListNode head) {
            int count = 0;
            
            ListNode p = head;
            while (p != null) {
                count++;
                p = p.next;
            }
            
            return count;
        }
    }