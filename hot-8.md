<img width="770" height="653" alt="image" src="https://github.com/user-attachments/assets/523bada4-9ff8-49b1-bdcb-8b78d357aece" />

## 核心思路:定长滑窗
1、字母计数统计

由于字符串仅由小写字母组成，使用长度为 26 的数组统计 p 中各字母出现次数，同时用另一个数组统计滑动窗口内 s 子串的字母次数。

2、固定窗口滑动

窗口右边界不断向右扩展，将对应字母加入计数；当窗口长度达到 p 的长度时，判断窗口内字母计数与 p 的字母计数是否一致：

- 一致则为字母异位词，记录窗口左边界下标
- 
-无论是否匹配，窗口左边界都向右移动，移除对应字母的计数

3、判断逻辑

直接比较两个字母计数数组是否相等，相等即说明子串是 p 的字母异位词。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        // 统计 p 的每种字母出现的次数
        int[] cntP = new int[26];
        for (char c : p.toCharArray()) {
            cntP[c - 'a']++;
        }

        List<Integer> ans = new ArrayList<>();
        int[] cntS = new int[26]; // 统计 s 中长度为 p.length() 的窗口内字母次数
        for (int right = 0; right < s.length(); right++) {
            // 窗口右边界字母加入计数
            cntS[s.charAt(right) - 'a']++;
            int left = right - p.length() + 1;
            // 窗口长度不足时跳过
            if (left < 0) {
                continue;
            }

            // 字母计数相同则为异位词，记录左端点
            if (Arrays.equals(cntP, cntS)) {
                ans.add(left);
            }
            // 窗口左边界字母移出计数
            cntS[s.charAt(left) - 'a']--;
        }
        return ans;
    }
}
```


## 不定长滑窗

枚举子串t的右端点，如果发现t其中一种字母的出现次数大于p的这种字母出现次数，则右移t的左端点（确保窗口中，不会比p多出字母个数）

如果发现t的长度等于p的长度，则说明t的每种字母出现次数，等于p的每种字母出现次数，即t是p的异位词

### 思路

首先、统计p中字母出现的个数

然后、每出现一个字母c = s.charAt(right) - 'a'；就把对应cnt[c]--，就相当于窗口中加入了s.charAt(right)

当cnt[c]<0时，说明窗口中的c字母个数超出了p中字母个数，此时应该左移动left指针，并且让cnt[c]++；

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        // 统计 p 的每种字母的出现次数
        int[] cnt = new int[26]; 
        for (char c : p.toCharArray()) {
            cnt[c - 'a']++;
        }

        List<Integer> ans = new ArrayList<>();
        int left = 0;
        for (int right = 0; right < s.length(); right++) {
            int c = s.charAt(right) - 'a';
            cnt[c]--; // 右端点字母进入窗口
            while (cnt[c] < 0) { // 字母 c 太多了
                cnt[s.charAt(left) - 'a']++; // 左端点字母离开窗口
                left++;
            }
            if (right - left + 1 == p.length()) { // t 和 p 的每种字母的出现次数都相同（证明见上）
                ans.add(left); // t 左端点下标加入答案
            }
        }
        return ans;
    }
}

```

