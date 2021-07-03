### 20210629

My solution

    class Solution {
        public String intToRoman(int num) {
            String result = "";
            
            while (num >= 1000) {
                result += "M";
                num = num - 1000;
            }
            
            result += getRoman(num / 100, "C", "D", "M");
            num = num - (num / 100) * 100;
            
            result += getRoman(num / 10, "X", "L", "C");
            num = num - (num / 10) * 10;
            
            result += getRoman(num, "I", "V", "X");
            
            return result;
        }
        
        private String getRoman(int firstDigit, String step, String middle, String upperStep) {
            String result = "";
            
            if (firstDigit == 9) {
                result += step + upperStep;
            } else if (firstDigit >= 5) {
                result += middle;
                for (int i = 0; i < firstDigit - 5; i++) {
                    result += step;
                }
            } else if (firstDigit == 4) {
                result += step + middle;
            } else {
                for (int i = 0; i < firstDigit; i++) {
                    result += step;
                }
            }
            
            return result;
        }
    }

Solution 1

    class Solution {
        public String intToRoman(int num) {
            String[] literals = new String[]{"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
            int[] values = new int[]{1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
            
            String result = "";
            for (int i = 0; i < literals.length && num > 0; i++) {
                while (num >= values[i]) {
                    result += literals[i];
                    num = num - values[i];
                }
            }
            
            return result;
        }
    }
