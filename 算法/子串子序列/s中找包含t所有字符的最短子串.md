如果 s 中存在这样的子串，我们保证它是唯一的答案
示例 1：  
输入：s = “ADOBECODEBANC”, t = “ABC” 输出：“BANC”  
示例 2：  
输入：s = “a”, t = “a” 输出：“a”  

滑动窗口适用于找连续子串

标准ASCII字符集共128个字符

```java
public String minWindow(String s, String t) {  
    int[] need = new int[128];  
    int lens = s.length();  
    int valid = 0, needCount = 0;  
    for(char c : t.toCharArray()) {  
        if(need[c] == 0) needCount++;  
        need[c]++;  
    }  
    // minlen 不用Integer.MAX_VALUE,容易爆栈
    int left = 0, right = 0, minlen = lens+1, minstart = 0;  
    int[] window = new int[128];  
    while(right < lens) {  
        char add = s.charAt(right);  
        right++;  
        // 这里的隐含信息是need[add] = 0时是不需要的,也就不用判断  
        if(need[add] > 0) {  
            window[add]++;  
            if(window[add] == need[add]) {  
                valid++;  
            }  
        }  
        while(valid == needCount) {  
            if(minlen > right-left) {  
                minstart = left;  
                minlen = right - left;  
            }  
            char remove = s.charAt(left);  
            left++;  
            if(need[remove] > 0) {  
                if(window[remove] == need[remove]) {  
                    valid--;  
                }  
            }  
            window[remove]--;  
        }  
    }  
    return minlen == s.length() + 1 ? "" : s.substring(minstart, minstart + minlen);  
}
```


