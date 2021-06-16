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


### Leetcode 17

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

### lc 19

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