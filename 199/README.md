### 20210702

My solution

    class Solution {
        public List<Integer> rightSideView(TreeNode root) {
            List<Integer> result = new ArrayList<>();
            
            if (root == null) {
                return result;
            }
            
            Queue<TreeNode> queue = new LinkedList<>();
            queue.add(root);
            
            while(!queue.isEmpty()) {
                int numOfNodes = queue.size();
                for (int i = 0; i < numOfNodes; i++) {
                    TreeNode curr = queue.poll();
                    if (i == numOfNodes - 1) {
                        result.add(curr.val);
                    }
                    
                    if (curr.left != null) {
                        queue.add(curr.left);
                    }
                    
                    if (curr.right != null) {
                        queue.add(curr.right);
                    }
                }
            }
            
            return result;
        }
    }

DFS 

    class Solution {
        public List<Integer> rightSideView(TreeNode root) {
            List<Integer> output = new ArrayList<>();
            
            if (root == null) {
                return output;
            }
            
            helper(root, 0, output);
            
            return output;
        }
        
        private void helper(TreeNode root, int level, List<Integer> output) {
            if (level == output.size()) {
                output.add(root.val);
            }
            
            if (root.right != null) {
                helper(root.right, level + 1, output);
            }
            
            if (root.left != null) {
                helper(root.left, level + 1, output);
            }
        }
    }