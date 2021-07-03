### 20210701

My solution

    public class Solution {
        public ListNode detectCycle(ListNode head) {
            Set<ListNode> set = new HashSet<>();
            
            ListNode p = head;
            while (p != null) {
                if (!set.add(p)) {
                    return p;
                }
                p = p.next;
            }
            
            return null;
        }
    }

Solution 2: 

    public class Solution {
        public ListNode detectCycle(ListNode head) {
            ListNode fast = head;
            ListNode slow = head;
            
            while (fast != null && fast.next != null) {
                fast = fast.next.next;
                slow = slow.next;
                
                if (fast == slow) {
                    break;
                }
            }
            
            if (fast == null || fast.next == null) {
                return null;
            }
            
            ListNode p = head;
            while (p != slow) {
                p = p.next;
                slow = slow.next;
            }
            
            return p;
        }
    }