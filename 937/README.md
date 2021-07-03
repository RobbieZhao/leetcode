### 20210701

My Solution

    class Solution {
        class LetterLogComparator implements Comparator<String> {
            @Override
            public int compare(String log1, String log2) {
                int index1 = log1.indexOf(' ');
                String identifier1 = log1.substring(0, index1);
                String content1 = log1.substring(index1 + 1);
                
                int index2 = log2.indexOf(' ');
                String identifier2 = log2.substring(0, index2);
                String content2 = log2.substring(index2 + 1);
                
                int compare = content1.compareTo(content2);
                if (compare != 0) {
                    return compare;
                } else {
                    return identifier1.compareTo(identifier2);
                }
            }
        }
        
        public String[] reorderLogFiles(String[] logs) {
            int digitLogPointer = logs.length - 1;
            for (int i = logs.length - 1; i >= 0; i--) {
                if (isDigitLog(logs[i])) {
                    swap(logs, i, digitLogPointer);
                    digitLogPointer--;
                }
            }
            
            Arrays.sort(logs, 0, digitLogPointer + 1, new LetterLogComparator());
            
            return logs;
        }
        
        private void swap(String[] logs, int i, int j) {
            String tmp = logs[i];
            logs[i] = logs[j];
            logs[j] = tmp;
        }
        
        private boolean isLetterLog(String log) {
            char firstChar = log.split(" ")[1].charAt(0);
            
            return firstChar >= 'a' && firstChar <= 'z';
        }
        
        private boolean isDigitLog(String log) {
            return !isLetterLog(log);
        }
    }

Solution 1:

    class Solution {
        class LogComparator implements Comparator<String> {
            @Override
            public int compare(String log1, String log2) {
                String[] log1Strs = log1.split(" ", 2);
                String identifier1 = log1Strs[0];
                String content1 = log1Strs[1];
                boolean isLetter1 = Character.isAlphabetic(content1.charAt(0));
                    
                String[] log2Strs = log2.split(" ", 2);
                String identifier2 = log2Strs[0];
                String content2 = log2Strs[1];
                boolean isLetter2 = Character.isAlphabetic(content2.charAt(0));
                
                if (isLetter1 && isLetter2) {
                    int compare = content1.compareTo(content2);
                    
                    if (compare != 0) {
                        return compare;
                    }
                    
                    return identifier1.compareTo(identifier2);
                } else if (isLetter1) {
                    return -1;
                } else if (isLetter2) {
                    return 1;
                } else {
                    return 0;
                }
            }
        }
        
        public String[] reorderLogFiles(String[] logs) {
            Arrays.sort(logs, new LogComparator());
            
            return logs;
        }
    }
