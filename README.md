### Leetcode 56 Medium

Merge intervals

#### 2021. 6. 15
这题毫无思路。。

这是第一次看了答案后的解法

	class Solution {
	    public int[][] merge(int[][] intervals) {
	        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
	        
	        ArrayList<int[]> result = new ArrayList<>();
	        
	        result.add(intervals[0]);
	        for (int i = 1; i < intervals.length; i++) {
	            int[] last = result.get(result.size() - 1);
	            int[] curr = intervals[i];
	            if (curr[0] <= last[1]) {
	                last[1] = Math.max(last[1], curr[1]);
	            } else {
	                result.add(curr);
	            }
	        }
	        
	        int[][] ret = new int[result.size()][];
	        return result.toArray(ret);
	    }
	}

 - 值得关注的是 `last[1] = Math.max(last[1], curr[1]);` 这个很容易忘记
 - 最后两行可以优化成 `return result.toArray(new int[result.size()][])`
 - 解答视频里面有一句话很关键：Whenever you have a solution that's `O(n^2)`, consider sorting. 


### Leetcode 17 Medium

#### 2021.6.15

这是还没看答案做的。直接想到的就是递归，容易错的是`digits.length == 0`的时候。

	class Solution {
	    public List<String> letterCombinations(String digits) {
	        Map<Character, char[]> map = new HashMap<>();
	        map.put('2', new char[] {'a', 'b', 'c'});
	        map.put('3', new char[] {'d', 'e', 'f'});
	        map.put('4', new char[] {'g', 'h', 'i'});
	        map.put('5', new char[] {'j', 'k', 'l'});
	        map.put('6', new char[] {'m', 'n', 'o'});
	        map.put('7', new char[] {'p', 'q', 'r', 's'});
	        map.put('8', new char[] {'t', 'u', 'v'});
	        map.put('9', new char[] {'w', 'x', 'y', 'z'});
	        
	        return letterCombinations(digits, map);
	    }
	    
	    public List<String> letterCombinations(String digits, Map<Character, char[]> map) {
	        
	        List<String> result = new ArrayList<>();
	        
	        if (digits.length() == 0) {
	            return result;
	        }
	        
	        char head = digits.charAt(0);
	        
	        for (char c : map.get(head)) {
	            List<String> list = letterCombinations(digits.substring(1), map);
	            
	            if (list.size() == 0) {
	                result.add(Character.toString(c));
	            } else {
	                for (String str : list) {
	                    result.add(c + str);
	                }
	            }
	        }
	        
	        return result;
	    }
	}

 - 可以改进的是map，可以改成char->string的mapping. A lot less typing!
 - 以及string改成stringbuilder会好些。

看了答案后，发现这个是backtracking的问题，重做如下：

	class Solution {
	    public List<String> letterCombinations(String digits) {
	        Map<Character, String> map = new HashMap<>();
	        
	        map.put('2', "abc");
	        map.put('3', "def");
	        map.put('4', "ghi");
	        map.put('5', "jkl");
	        map.put('6', "mno");
	        map.put('7', "pqrs");
	        map.put('8', "tuv");
	        map.put('9', "wxyz");
	        
	        List<String> output = new ArrayList<>();
	        
	        if (digits.length() != 0) {  
	            backtrack(output, digits, new StringBuilder(), 0, map);
	        }
	        
	        return output;
	    }
	    
	    private void backtrack(List<String> output, String digits, StringBuilder sb, int start, Map<Character, String> map) {
	        if (start == digits.length()) {
	            output.add(sb.toString());
	            return;
	        }
	        
	        char head = digits.charAt(start);
	        for (char c : map.get(head).toCharArray()) {
	            sb.append(c);
	            backtrack(output, digits, sb, start + 1, map);
	            sb.deleteCharAt(sb.length() - 1);
	        }
	    }
	}

看了排名第一的回答后用queue了一遍。。

	class Solution {
	    public List<String> letterCombinations(String digits) {
	        if (digits.length() == 0) {
	            return new ArrayList<>();
	        }
	        
	        Map<Character, String> map = new HashMap<>();
	        
	        map.put('2', "abc");
	        map.put('3', "def");
	        map.put('4', "ghi");
	        map.put('5', "jkl");
	        map.put('6', "mno");
	        map.put('7', "pqrs");
	        map.put('8', "tuv");
	        map.put('9', "wxyz");
	        
	        List<String> output = new ArrayList<>();
	        
	        output.add("");
	        for (int i = 0; i < digits.length(); i++) {
	            char curr = digits.charAt(i);
	            char[] possibleChars = map.get(curr).toCharArray();
	            
	            while (output.get(0).length() == i) {
	                String s = output.remove(0);
	                for (char c : possibleChars) {
	                    output.add(s + c);
	                }
	            }
	        }
	        
	        return output;
	    }
	}

 - 可以把递归 （backtrack）理解为DFS， Queue这个方法就是BFS

### lc 19 Medium

#### 2021.6.15 

只想到了two pass的思路

	class Solution {
	    public ListNode removeNthFromEnd(ListNode head, int n) {
	        ListNode dummy = new ListNode(-1, head);
	        ListNode p = dummy;
	        
	        int size = 0;
	        while (p.next != null) {
	            p = p.next;
	            size++;
	        }
	        
	        p = dummy;
	        int count = 0;
	        while (count < size - n) {
	            p = p.next;
	            count++;
	        }
	        p.next = p.next.next;
	        
	        return dummy.next;
	    }
	}

看了答案后重做

	class Solution {
	    public ListNode removeNthFromEnd(ListNode head, int n) {
	        ListNode dummy = new ListNode(-1, head);
	        ListNode front = dummy;
	        ListNode back = dummy;
	        
	        while(n >= 0) {
	            front = front.next;
	            n--;
	        }
	        
	        while (front != null) {
	            front = front.next;
	            back = back.next;
	        }
	        
	        back.next = back.next.next;
	        
	        return dummy.next;
	    }
	}

 - 这种单链表和一维数组的题经常用到double pointers

### lc 283 easy

#### 2021.6.15

The first solution that came to mind:

	class Solution {
	    public void moveZeroes(int[] nums) {
	        int nonZeroPointer = 0;
	        for (int i = 0; i < nums.length; i++) {
	            if (nums[i] != 0) {
	                nums[nonZeroPointer] = nums[i];
	                nonZeroPointer++;
	            }
	        }
	        
	        while (nonZeroPointer < nums.length) {
	            nums[nonZeroPointer] = 0;
	            nonZeroPointer++;
	        }
	    }
	}

The problem is that if the array is filled with zeros, then we have to loop throught the array twice.

The next solution will be to also change the number pointed by the fast pointer. Because in the end, the number pointed by the fast pointer will either be 0, or will be another number if the slow pointer could pass it. So we set it to be 0, if the slow pointer never reaches it.

The next solution:

	class Solution {
	    public void moveZeroes(int[] nums) {
	        int nonZeroPointer = 0;
	        for (int i = 0; i < nums.length; i++) {
	            if (nums[i] != 0) {
	                int tmp = nums[i];
	                nums[i] = nums[nonZeroPointer];
	                nums[nonZeroPointer] = tmp;
	                nonZeroPointer++;
	            }
	        }
	    }
	}

It's like snowballing. We gather all the zeros along the way, so that the numbers between non-zero pointer and fast pointer are all zeros.

### Lc 4 Hard

#### 2021.6.15

看解答看到吐血。。

	class Solution {
	    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
	        if (nums1.length > nums2.length) {
	            int[] tmp = nums1;
	            nums1 = nums2;
	            nums2 = tmp;
	        }
	        
	        int m = nums1.length;
	        int n = nums2.length;
	        
	        int start = 0;
	        int end = m;  
	        while (start <= end) {
	            int i = (start + end) / 2;
	            int j = (m + n + 1) / 2 - i;
	            
	            if (i < m && nums1[i] < nums2[j - 1]) {
	                start = i + 1;
	            } else if (i > 0 && nums1[i - 1] > nums2[j]) {
	                end = i - 1;
	            } else {
	                
	                int maxOfLeft;
	                if (i == 0) {
	                    maxOfLeft = nums2[j - 1];
	                } else if (j == 0) {
	                    maxOfLeft = nums1[i - 1];
	                } else {
	                    maxOfLeft = Math.max(nums1[i - 1], nums2[j - 1]);
	                }
	                
	                if ((m + n) % 2 == 1) {
	                    return maxOfLeft;
	                }
	                
	                int minOfRight;
	                if (i == m) {
	                    minOfRight = nums2[j];
	                } else if (j == n) {
	                    minOfRight = nums1[i];
	                } else {
	                    minOfRight = Math.min(nums1[i], nums2[j]);
	                }
	                      
	                return (maxOfLeft + minOfRight) / 2.0;
	            }
	        }
	        
	        throw new IllegalArgumentException();
	    }
	}

### lc 22 medium

#### 2021.6.16 

这是自己做出来的。一个典型的backtracking的问题

	class Solution {
	    public List<String> generateParenthesis(int n) {
	        List<String> output = new ArrayList<>();
	        
	        backtrack(output, new StringBuilder(), 0, 0, n);
	        
	        return output;
	    }
	    
	    public void backtrack(List<String> output, StringBuilder sb, int numOfLeftParen, int numOfUnclosed, int n) {
	        if (numOfLeftParen == n && numOfUnclosed == 0) {
	            output.add(sb.toString());
	            return;
	        }
	        
	        char[] options;
	        if (numOfLeftParen == n) {
	            options = new char[]{')'};
	        } else if (numOfUnclosed == 0) {
	            options = new char[]{'('};
	        } else {
	            options = new char[]{'(', ')'};
	        }
	        
	        for (char c : options) {
	            sb.append(c);
	            if (c == '(') {
	                backtrack(output, sb, numOfLeftParen + 1, numOfUnclosed + 1, n);
	            } else {
	                backtrack(output, sb, numOfLeftParen, numOfUnclosed - 1, n);
	            }
	            sb.deleteCharAt(sb.length() - 1);
	        }
	    }
	}

其中backtrack的代码可以稍微优化一下，不过复杂度不变

这是优化后的代码：

	class Solution {
	    public List<String> generateParenthesis(int n) {
	        List<String> output = new ArrayList<>();
	        
	        backtrack(output, new StringBuilder(), 0, 0, n);
	        
	        return output;
	    }
	    
	    public void backtrack(List<String> output, StringBuilder sb, int numOfLeftParen, int numOfUnclosed, int n) {
	        if (numOfLeftParen == n && numOfUnclosed == 0) {
	            output.add(sb.toString());
	            return;
	        }
	        
	        if (numOfLeftParen != n) {
	            sb.append('(');
	            backtrack(output, sb, numOfLeftParen + 1, numOfUnclosed + 1, n);
	            sb.deleteCharAt(sb.length() - 1);
	        }
	        
	        if (numOfUnclosed != 0) {
	            sb.append(')');
	            backtrack(output, sb, numOfLeftParen, numOfUnclosed - 1, n);
	            sb.deleteCharAt(sb.length() - 1);
	        }
	    }
	}

### lc 150

#### 2021.6.16

典型的stack解法

	class Solution {
	    public int evalRPN(String[] tokens) {
	        Stack<Integer> stack = new Stack<>();
	        
	        for (String token : tokens) {
	            
	            if ("+-*/".contains(token)) {
	                int second = stack.pop();
	                int first = stack.pop();
	                
	                if (token.equals("+")) {
	                    stack.push(first + second);
	                } else if (token.equals("-")) {
	                    stack.push(first - second);
	                } else if (token.equals("*")) {
	                    stack.push(first * second);
	                } else {
	                    stack.push(first / second);
	                }
	            } else {
	                stack.push(Integer.valueOf(token));
	            }
	        }
	        
	        return stack.pop();
	    }
	}

### lc 1328 Break a palindrome medium

#### 2021.6.16

这是自己做的。本来是想把中间的字符去掉 （如果是奇数个字符），然后再做，但是调整了很多次都觉得代码太复杂了，所以不如直接loop

	class Solution {
	    public String breakPalindrome(String palindrome) {
	        if (palindrome.length() == 1) {
	            return "";
	        }
	        
	        for (int i = 0; i < palindrome.length(); i++) {
	            if (i == palindrome.length() / 2 && palindrome.length() % 2 == 1) {
	                continue;
	            } 
	            
	            if (palindrome.charAt(i) != 'a') {
	                return palindrome.substring(0, i) + 'a' + palindrome.substring(i + 1);
	            }
	        }
	        
	        return palindrome.substring(0, palindrome.length() - 1) + 'b';
	    }
	}

在看了解答后发现其实如果过半的时候，还没有return，这说明所有的字符都是`a`。那就不用继续看了，直接把最后一个字符换成`b`就行。所以进一步优化

	class Solution {
	    public String breakPalindrome(String palindrome) {
	        if (palindrome.length() == 1) {
	            return "";
	        }
	        
	        for (int i = 0; i < palindrome.length() / 2; i++) {
	            if (palindrome.charAt(i) != 'a') {
	                return palindrome.substring(0, i) + 'a' + palindrome.substring(i + 1);
	            }
	        }
	        
	        return palindrome.substring(0, palindrome.length() - 1) + 'b';
	    }
	}

### lc 338 Counting bits

#### 2021.6.16 

第一遍做的（做了点修缮）

	class Solution {
	    public int[] countBits(int n) {
	        int[] result = new int[n + 1];
	                
	        for (int i = 1; i <= n; ) {
	            int count = i;
	            for (int j = 0; j < count && i <= n; j++) {
	                result[i] = 1 + result[j];
	                i++;
	            }   
	        }
	        
	        return result;
	    }
	}
	
看了一遍答案重做

	class Solution {
	    public int[] countBits(int n) {
	        int[] result = new int[n + 1];
	                
	        for (int i = 1; i <= n; i++) {
	            result[i] = result[i >> 1] + i % 2;
	        }
	        
	        return result;
	    }
	}

### lc 239 Sliding Window Maximum Hard

#### 2021.6.16

又是毫无思路的一题，看了答案后：

	class Solution {
	    public int[] maxSlidingWindow(int[] nums, int k) {
	        Deque<Integer> deque = new ArrayDeque<>();
	        
	        int n = nums.length;
	        int[] maxs = new int[n - k + 1];
	        
	        for (int i = 0; i < n; i++) {
	            cleanDeque(i, k, deque, nums);
	            deque.addLast(i);
	            
	            int maxIndex = deque.getFirst();
	            if (i + 1 - k >= 0) {
	                maxs[i + 1 - k] = nums[maxIndex];
	            }
	        }
	        
	        return maxs;
	    }
	    
	    private void cleanDeque(int i, int k, Deque<Integer> deque, int[] nums) {
	        if (!deque.isEmpty() && deque.getFirst() == i - k) {
	            deque.removeFirst();
	        }
	        
	        while (!deque.isEmpty() && nums[deque.getLast()] < nums[i]) {
	            deque.removeLast();
	        }
	    }
	}

这是看了solution3 后做的

	class Solution {
	    public int[] maxSlidingWindow(int[] nums, int k) {
	        int n = nums.length;
	        
	        int[] left = new int[n];
	        int[] right = new int[n];
	        
	        for (int i = 0; i < n; i++) {
	            if (i % k == 0) {
	                left[i] = nums[i];
	            } else {
	                left[i] = Math.max(left[i - 1], nums[i]);
	            }
	        }
	        
	        for (int j = n - 1; j >= 0; j--) {
	            if (j == n - 1 || (j + 1) % k == 0) {
	                right[j] = nums[j];
	            } else {
	                right[j] = Math.max(right[j + 1], nums[j]);
	            }
	        }
	        
	        int[] maxs = new int[n - k + 1];
	        for (int i = 0; i < maxs.length; i++) {
	            maxs[i] = Math.max(right[i], left[i + k - 1]);
	        }
	        
	        return maxs;
	    }
	}

### lc 1481 Least Number of Unique Integers after K Removals Medium

#### 2021.6.17

这是自己做的

	class Solution {
	    public int findLeastNumOfUniqueInts(int[] arr, int k) {
	        Map<Integer, Integer> map = new HashMap<>();
	        
	        for (int i = 0; i < arr.length; i++) {
	            map.put(arr[i], map.getOrDefault(arr[i], 0) + 1);
	        }
	        
	        int[][] counts= new int[map.size()][2];
	        int count = 0;
	        for (Map.Entry<Integer,Integer> entry : map.entrySet()) {
	            counts[count][0] = entry.getKey();
	            counts[count][1] = entry.getValue();
	            count++;
	        }
	        
	        Arrays.sort(counts, (a, b) -> Integer.compare(a[1], b[1]));
	        
	        for (int i = 0; i < counts.length; i++) {
	            if (counts[i][1] <= k) {
	                k = k - counts[i][1];
	            } else {
	                return counts.length - i;
	            }
	        }
	        
	        return 0;
	    }
	}

看了答案之后用priorityqueue做了一遍：

	class Solution {
	    public int findLeastNumOfUniqueInts(int[] arr, int k) {
	        Map<Integer, Integer> map = new HashMap<>();
	        for (int a : arr) {
	            map.put(a, map.getOrDefault(a, 0) + 1);
	        }
	        
	        PriorityQueue<Integer> pq = new PriorityQueue<>(Comparator.comparing(map::get));
	        pq.addAll(map.keySet());
	        
	        while (k > 0) {
	            k = k - map.get(pq.poll());
	        }
	        
	        if (k < 0) {
	            return pq.size() + 1;
	        }
	        
	        return pq.size();
	    }
	}

答案的第二种做法，进一步用treemap把hashmap的结果压缩

	class Solution {
	    public int findLeastNumOfUniqueInts(int[] arr, int k) {
	        Map<Integer, Integer> map = new HashMap<>();
	        for (int a : arr) {
	            map.put(a, map.getOrDefault(a, 0) + 1);
	        }
	        
	        TreeMap<Integer, Integer> occurrenceCount = new TreeMap<>();
	        for (Integer v : map.values()) {
	            occurrenceCount.put(v, occurrenceCount.getOrDefault(v, 0) + 1);
	        }
	        
	        int distinctCount = map.size();
	        
	        while (k > 0) {
	            Map.Entry<Integer, Integer> entry = occurrenceCount.pollFirstEntry();
	            int key = entry.getKey();
	            int value = entry.getValue();
	            if (k > key * value) {
	                k = k - key * value;
	                distinctCount -= value;
	            } else {
	                return distinctCount - k / key;
	            }
	        }
	        
	        return distinctCount;
	    }
	}

答案的第三种解法，与其用一个treemap，不如直接放在array里

	class Solution {
	    public int findLeastNumOfUniqueInts(int[] arr, int k) {
	        Map<Integer, Integer> map = new HashMap<>();
	        for (int a : arr) {
	            map.put(a, map.getOrDefault(a, 0) + 1);
	        }
	        
	        int[] occurrenceCount = new int[arr.length + 1];
	        for (int v : map.values()) {
	            occurrenceCount[v]++;
	        }
	        
	        int distinctCount = map.size();
	        int occurrence = 1;
	        while (k > 0) {
	            if (k > occurrence * occurrenceCount[occurrence]) {
	                k = k - occurrence * occurrenceCount[occurrence];
	                distinctCount -= occurrenceCount[occurrence];
	                occurrence++;
	            } else {
	                return distinctCount  - k / occurrence;
	            }
	        }
	        
	        return distinctCount;
	    }
	}

### lc 1041 Robot Bounded In Circle Medium 

#### 2021.6.17

这是自己做的，如果方向变了，或者还停留在原地，那就迟早会绕回来

	class Solution {
	    public boolean isRobotBounded(String instructions) {
	        int x = 0, y = 0;
	        char direction = 'N';
	        
	        for (int i = 0; i < instructions.length(); i++) {
	            char c = instructions.charAt(i);
	            if (c == 'G') {                
	                if (direction == 'N') {
	                    y++;
	                } else if (direction == 'S') {
	                    y--;
	                } else if (direction == 'W') {
	                    x--;
	                } else {
	                    x++;
	                }
	            } else if (c == 'L') {
	                if (direction == 'N') {
	                    direction = 'W';
	                } else if (direction == 'S') {
	                    direction = 'E';
	                } else if (direction == 'W') {
	                    direction = 'S';
	                } else {
	                    direction = 'N';
	                }
	            } else {
	                if (direction == 'N') {
	                    direction = 'E';
	                } else if (direction == 'S') {
	                    direction = 'W';
	                } else if (direction == 'W') {
	                    direction = 'N';
	                } else {
	                    direction = 'S';
	                }
	            }
	        }
	                
	        return (x == 0 && y == 0) || direction != 'N';
	    }
	}

看了答案后做了一遍，复杂度没有变化，只不过变得精简了许多

	class Solution {
	    public boolean isRobotBounded(String instructions) {
	        int x = 0, y = 0;
	        int[][] steps = new int[][]{{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
	        int directionIndex = 0;
	        
	        for (int i = 0; i < instructions.length(); i++) {
	            char c = instructions.charAt(i);
	            if (c == 'G') {
	                x += steps[directionIndex][0];
	                y += steps[directionIndex][1];
	            } else if (c == 'L') {
	                directionIndex = (directionIndex + 3) % 4;
	            } else {
	                directionIndex = (directionIndex + 1) % 4;
	            }
	        }
	        
	        return (x == 0 && y == 0) || directionIndex != 0;
	    }
	}

### lc 973  K closest points to origin Medium

#### 2021.6.17

这是我第一遍做的。虽然用了priorityqueue，但是复杂度跟直接sort的复杂度一样的。。。

	class Solution {
	    public int[][] kClosest(int[][] points, int k) {
	        PriorityQueue<int[]> pq = new PriorityQueue<>(10, (point1, point2) -> (squareSum(point1) - squareSum(point2)));
	        
	        for (int i = 0; i < points.length; i++) {
	            pq.add(points[i]);
	        }
	        
	        int[][] result = new int[k][2];
	        for (int i = 0; i < k; i++) {
	            result[i] = pq.poll();
	        }
	        
	        return result;
	    }
	    
	    public int squareSum(int[] point) {
	        return point[0] * point[0] + point[1] * point[1];
	    }
	}

看了答案后发现这个priorityQueue的顺序应该反过来，这样就能保证里面永远只有k个

	class Solution {
	    public int[][] kClosest(int[][] points, int k) {
	        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> squareSum(b) - squareSum(a));
	        
	        for (int i = 0; i < points.length; i++) {
	            pq.add(points[i]);
	            if (pq.size() > k) {
	                pq.poll();
	            }
	        }
	        
	        int[][] result = new int[k][2];
	        for (int i = 0; i < k; i++) {
	            result[i] = pq.poll();
	        }
	        
	        return result;
	    }
	    
	    private int squareSum(int[] point) {
	        return point[0] * point[0] + point[1] * point[1];
	    }
	}
	
看了答案里的第三种解答后。这个partition的方法太巧秒了，我自己写的话肯定只能写出来create new array用两个pointer，然后再copy回去的方法。

	class Solution {
	    public int[][] kClosest(int[][] points, int k) {
	        int pivot = 0;
	        
	        int l = 0, r = points.length - 1;
	        while (l < r) {
	            int mid = partition(points, l, r);
	            if (mid == k) {
	                break;
	            } else if (mid < k) {
	                l = mid + 1;
	            } else {
	                r = mid - 1;
	            }
	        }
	        
	        return Arrays.copyOf(points, k);
	    }
	    
	    private int partition(int[][] points, int l, int r) {
	        int[] pivot = points[l];
	        
	        while (l < r) {
	            while (l < r && compare(points[r], pivot) >= 0) {
	                r--;
	            }
	            points[l] = points[r];
	            
	            while (l < r && compare(points[l], pivot) <= 0) {
	                l++;
	            }
	            points[r] = points[l];
	        }
	        
	        points[l] = pivot;
	        
	        return l;
	    }
	    
	    private int compare(int[] A, int[] B) {
	        return squareSum(A) - squareSum(B);
	    }
	    
	    private int squareSum(int[] point) {
	        return point[0] * point[0] + point[1] * point[1];
	    }
	}

### lc 448 Find all numbers disappeared in an array Easy

#### 2021.6.17

第一思路，`O(N)` 的space

	class Solution {
	    public List<Integer> findDisappearedNumbers(int[] nums) {
	        int[] count = new int[nums.length + 1];
	        
	        for (int i = 0; i < nums.length; i++) {
	            count[nums[i]]++;
	        }
	        
	        List<Integer> result = new ArrayList<>();
	        for (int i = 1; i <= nums.length; i++) {
	            if (count[i] == 0) {
	                result.add(i);
	            }
	        }
	        
	        return result;
	    }
	}

看了答案之后的思路

	class Solution {
	    public List<Integer> findDisappearedNumbers(int[] nums) {
	        for (int i = 0; i < nums.length; i++) {
	            int index = Math.abs(nums[i]) - 1;
	            
	            if (nums[index] > 0) {
	                nums[index] = nums[index] * -1;
	            }
	        }
	        
	        List<Integer> result = new ArrayList<>();
	        for (int i = 0; i < nums.length; i++) {
	            if (nums[i] > 0) {
	                result.add(i + 1);
	            }
	        }
	        
	        return result;
	    }
	}
	
 - 既然它要求不要extra space, 就想想它可不可以修改输入


### lc 1335 Minimum Difficulty of a job schedule Hard

holy smokes! 居然解出来了一道hard

	class Solution {
	    private int[] jobDifficulty;
	    private int[][] memo;
	    
	    public int minDifficulty(int[] jobDifficulty, int d) {
	        int N = jobDifficulty.length;
	        if (d > N) {
	            return -1;
	        }
	        
	        this.jobDifficulty = jobDifficulty;
	        memo = new int[N][d];
	        
	        for (int i = 0; i < N; i++) {
	            Arrays.fill(memo[i], -1);
	        }
	        
	        return calculate(N - 1, d);
	    }
	    
	    private int calculate(int end, int d) {
	        if (end == 0) {
	            memo[0][0] = jobDifficulty[0];
	            return memo[0][0];
	        }
	        
	        if (d == 1) {
	            int maxOfPrevious = memo[end - 1][d - 1] == -1 ? calculate(end - 1, d) : memo[end - 1][d - 1];
	            memo[end][d - 1] = Math.max(maxOfPrevious, jobDifficulty[end]);
	            return memo[end][d-1];
	        }
	        
	        int min = Integer.MAX_VALUE;
	        int maxOfLastDay = jobDifficulty[end];
	        for (int i = end - 1; i >= d - 2; i--) {
	            int minOfPrevious = memo[i][d - 2] == -1 ? calculate(i, d - 1) : memo[i][d - 2];
	            if (jobDifficulty[i + 1] > maxOfLastDay) {
	                maxOfLastDay = jobDifficulty[i + 1];
	            }
	            if (minOfPrevious + maxOfLastDay < min) {
	                min = minOfPrevious + maxOfLastDay;
	            }
	        }
	        
	        memo[end][d-1] = min;
	        
	        return min;
	    }
	}

### lc 31 Next Permutation Medium

#### 2021.6.18

毫无思路的一题，看了解答的思路做的

	class Solution {
	    public void nextPermutation(int[] nums) {
	        // get i
	        int i;
	        for (i = nums.length - 1; i >= 1; i--) {
	            if (nums[i - 1] < nums[i]) {
	                break;
	            }
	        }
	        
	        if (i == 0) {
	            reverse(nums, 0);
	            return;
	        }
	        
	        // get j
	        int j;
	        for (j = i; j < nums.length; j++) {     
	            if (nums[i - 1] >= nums[j]) {
	                j--;
	                break;
	            }
	            
	            if (j == nums.length - 1) {
	                break;
	            }
	        }
	        
	        swap(nums, i - 1, j);
	        
	        reverse(nums, i);
	    }
	    
	    private void swap(int[] nums, int i, int j) {
	        int tmp = nums[i];
	        nums[i] = nums[j];
	        nums[j] = tmp;
	    }
	    
	    private void reverse(int[] nums, int start) {
	        int end = nums.length - 1;
	        while (start < end) {
	            swap(nums, start, end);
	            start++;
	            end--;
	        }
	    }
	}

### lc 34. Find First and Last Position of Element in Sorted Array Medium

#### 2021.6.19

题目并不难，但是还是做不出来。。

	class Solution {
	    public int[] searchRange(int[] nums, int target) {
	        int left = findBound(nums, target, true);
	        int right = findBound(nums, target, false);
	        
	        return new int[]{left, right};
	    }
	    
	    private int findBound(int[] nums, int target, boolean isLeft) {
	        int start = 0;
	        int end = nums.length - 1;
	        
	        while (start <= end) {
	            int mid = (start + end) / 2;
	            
	            if (nums[mid] == target) {
	                if (isLeft) {
	                    if (mid == start || nums[mid - 1] != target) {
	                        return mid;
	                    } else {
	                        end = mid - 1;
	                    }
	                } else {
	                    if (mid == end || nums[mid + 1] != target) {
	                        return mid;
	                    } else {
	                        start = mid + 1;
	                    }
	                }
	            } else if (nums[mid] > target) {
	                end = mid - 1;
	            } else {
	                start = mid + 1;
	            }
	        }
	        
	        return -1;
	    }
	}

### lc Combination Sum 39 Medium

#### 2021.6.19

backtrack 问题

	class Solution {
	    private List<List<Integer>> output;
	    private int[] candidates;
	    private int target;
	    
	    public List<List<Integer>> combinationSum(int[] candidates, int target) {
	        Arrays.sort(candidates);
	        
	        this.candidates = candidates;
	        this.target = target;
	        this.output = new ArrayList<>();
	        
	        List<Integer> tmp = new ArrayList<>();
	        
	        backtrack(tmp, 0, 0);
	        
	        return output;
	    }
	    
	    private void backtrack(List<Integer> tmp, int sum, int start) {
	        if (start == candidates.length) {
	            return;
	        }
	        
	        if (sum == target) {
	            output.add(new ArrayList<>(tmp));
	        }
	        
	        for (int i = start; i < candidates.length; i++) {
	            if (candidates[i] > target - sum) {
	                break;
	            }
	            
	            tmp.add(candidates[i]);
	            sum = sum + candidates[i];
	            backtrack(tmp, sum, i);
	            tmp.remove(tmp.size() - 1);
	            sum = sum - candidates[i];
	        }
	    }
	}
	
### lc 48. Rotate Image Medium

#### 2021.6.19

自己做的！！cs61b有一节Q&A就讲了这题！！

	class Solution {
	    public void rotate(int[][] matrix) {
	        int n = matrix.length;
	        for (int row = 0; row < n / 2; row++) {
	            for (int col = row; col < n - 1 - row; col++) {
	                rotate(matrix, row, col);
	            }
	        }
	    }
	    
	    private void rotate(int[][] matrix, int i, int j) {
	        int n = matrix.length;
	        
	        int tmp = matrix[i][j];
	        matrix[i][j] = matrix[n-1-j][i];
	        matrix[n-1-j][i] = matrix[n-1-i][n-1-j];
	        matrix[n-1-i][n-1-j] = matrix[j][n-1-i];
	        matrix[j][n-1-i] = tmp;
	    }
	}

看了答案做的。如果是先reflect再transpose, 那就是逆时针旋转90度

	class Solution {
	    public void rotate(int[][] matrix) {
	        transpose(matrix);
	        reflect(matrix);
	    }
	    
	    private void transpose(int[][] matrix) {
	        for (int i = 0; i < matrix.length; i++) {
	            for (int j = i + 1; j < matrix.length; j++) {
	                int tmp = matrix[i][j];
	                matrix[i][j] = matrix[j][i];
	                matrix[j][i] = tmp;
	            }
	        }
	    }
	    
	    private void reflect(int[][] matrix) {
	        for (int i = 0; i < matrix.length; i++) {
	            for (int j = 0; j < matrix.length / 2; j++) {
	                int tmp = matrix[i][j];
	                matrix[i][j] = matrix[i][matrix.length - 1 - j];
	                matrix[i][matrix.length - 1 - j] = tmp;
	            }
	        }
	    }
	    
	}

### lc 543 Diameter of Binary Tree Easy

#### 2021.6.19

自己做的 recursive

	class Solution {
	    public int diameterOfBinaryTree(TreeNode root) {
	        if (root == null) {
	            return -1;
	        }
	        
	        int leftDiameter = diameterOfBinaryTree(root.left);
	        int rightDiameter = diameterOfBinaryTree(root.right);
	        
	        int sumHeight = 2 + height(root.left) + height(root.right);
	        
	        return Math.max(Math.max(leftDiameter, rightDiameter), sumHeight);
	    }
	    
	    private int height(TreeNode root) {
	        if (root == null) {
	            return -1;
	        } 
	        
	        int leftHeight = height(root.left);
	        int rightHeight = height(root.right);
	        
	        return 1 + Math.max(leftHeight, rightHeight);
	    }
	}

看了答案后做的

	class Solution {
	    private int diameter;
	    public int diameterOfBinaryTree(TreeNode root) {
	        diameter = 0;
	        longestPath(root);
	        return diameter;
	    }
	    
	    private int longestPath(TreeNode root) {
	        if (root == null) {
	            return -1;
	        }
	        
	        int leftLongest = longestPath(root.left) + 1;
	        int rightLongest = longestPath(root.right) + 1;
	        
	        diameter = Math.max(diameter, leftLongest + rightLongest);
	        
	        return Math.max(leftLongest, rightLongest);
	    }
	}

### lc 10 Regular Expression Matching Hard

#### 2021.6.19

看了recursive的答案做的。。

	class Solution {
	    public boolean isMatch(String s, String p) {
	        if (p.isEmpty()) {
	            return s.isEmpty();
	        }
	        
	        boolean first_match = !s.isEmpty() && (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.');
	        
	        if (p.length() == 1) {
	            return first_match && s.length() == 1;
	        }
	        
	        if (p.charAt(1) == '*') {
	            return isMatch(s, p.substring(2)) || (first_match && isMatch(s.substring(1), p));
	        }
	        
	        return first_match && isMatch(s.substring(1), p.substring(1));
	    }
	}

看了dp的解法重做了一遍，不过说实话，dp的解法和recursive的方法非常相似。

	enum Result {
	    TRUE, FALSE
	}
	
	class Solution {
	    private Result[][] memo;
	    private String s;
	    private String p;
	    public boolean isMatch(String s, String p) {
	        memo = new Result[s.length() + 1][p.length() + 1];
	        this.s = s;
	        this.p = p;
	        
	        return calculate(0, 0);
	    }
	    
	    private boolean calculate(int i, int j) {
	        if (memo[i][j] != null) {
	            return memo[i][j] == Result.TRUE;
	        }
	        
	        boolean result;
	        if (j == p.length()) {
	            result = i == s.length();
	        } else {
	        
	            boolean firstMatch = i != s.length() && (s.charAt(i) == p.charAt(j) || p.charAt(j) == '.');
	
	            if (j == p.length() - 1) {
	                result = i == s.length() - 1 && firstMatch;
	            } else {
	
	                if (p.charAt(j + 1) == '*') {
	                    result = (firstMatch && calculate(i + 1, j)) || calculate(i, j + 2);
	                } else {
	                    result = firstMatch && calculate(i + 1, j + 1);
	                }
	            }
	        }
	        
	        memo[i][j] = result ? Result.TRUE : Result.FALSE;
	        return result;
	    }
	}