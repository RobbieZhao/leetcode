### 20210630

My solution

    class Solution {
        public Node copyRandomList(Node head) {
            Map<Node, Node> oldToNew = new HashMap<>();
            
            Node dummy = new Node(-1);
            Node newP = dummy;
            
            Node p = head;
            while (p != null) {
                Node newNode = new Node(p.val);
                newNode.random = p.random;
                
                newP.next = newNode;
                oldToNew.put(p, newNode);
                
                p = p.next;
                newP = newP.next;
            }
            
            newP = dummy.next;
            while (newP != null) {
                if (newP.random != null) {
                    newP.random = oldToNew.get(newP.random);
                }
                newP = newP.next;
            }
            
            return dummy.next;
        }
    }

Solution 1:

    class Solution {
        public Node copyRandomList(Node head) {
            Map<Node, Node> oldToNew = new HashMap<>();
            
            return copyRandomList(head, oldToNew); 
        }
        
        private Node copyRandomList(Node head, Map<Node, Node> oldToNew) {
            if (head == null) {
                return null;
            }
            
            if (oldToNew.containsKey(head)) {
                return oldToNew.get(head);
            }
            
            Node newNode = new Node(head.val);
            
            oldToNew.put(head, newNode);
            
            newNode.next = copyRandomList(head.next, oldToNew);
            newNode.random = copyRandomList(head.random, oldToNew);
            
            return newNode;
        }
    }

Solution 2

    class Solution {
        public Node copyRandomList(Node head) {
            Map<Node, Node> oldToNew = new HashMap<>();
            
            Node dummy = new Node(-1);
            Node newP = dummy;
            
            Node p = head;
            while (p != null) {
                newP.next = getNode(oldToNew, p);
                newP.next.random = getNode(oldToNew, p.random);
                p = p.next;
                newP = newP.next;
            }
            
            return dummy.next;
        }
        
        private Node getNode(Map<Node, Node> oldToNew, Node node) {
            if (node == null) {
                return null;
            }
            
            if (oldToNew.containsKey(node)) {
                return oldToNew.get(node);
            } else {
                Node newNode = new Node(node.val, node.next, node.random);
                
                oldToNew.put(node, newNode);
                
                return newNode;
            }
        }
    }

Solution 3

    class Solution {
        public Node copyRandomList(Node head) {
            Node p = head;
            while (p != null) {
                Node newNode = new Node(p.val);
                
                newNode.next = p.next;
                p.next = newNode;
                
                p = p.next.next;
            }
            
            p = head;
            while (p != null) {
                p.next.random = p.random == null ? null : p.random.next;
                p = p.next.next;
            }
            
            Node dummy = new Node(-1, head);
            p = dummy;
            while (p.next != null) {
                Node original = p.next;
                Node copy = p.next.next;
                original.next = copy.next;
                p.next = copy;
                p = p.next;
            }
            
            return dummy.next;
        }
    }