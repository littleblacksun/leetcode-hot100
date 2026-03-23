<img width="695" height="666" alt="image" src="https://github.com/user-attachments/assets/ea6c55fc-547b-49e4-b88d-2ad8994dec6f" />

使用双指针，创建滑动窗口来判断是否有重复的字符

<img width="692" height="97" alt="image" src="https://github.com/user-attachments/assets/cbcdf850-18b6-4ac7-b6a1-74f950ae01aa" />

当right指针向右移动时：判断到

> cnt[c]++;
>     while(cnt[c]>1)
> 
说明此时，窗口里面出现了重复的字符，

> cnt[s[left]]--; // 剔除左边的字符
> left ++ ; // 左指针右移 直到窗口里面无重复字符

最后比较窗口大小，找最小的

```java
class Solution {
    public int lengthOfLongestSubstring(String S) {
        char[] s = S.toCharArray();
        int n = s.length;
        int ans = 0; 
        int left = 0;
        int[] cnt = new int[128];
        for (int right = 0; right<n;right++){
            char c = s[right];
            cnt[c]++;
            while(cnt[c]>1){
                cnt[s[left]]--;
                left ++ ;
            }
            ans = Math.max(ans,right-left+1);
        }
        return ans;
    }
}
```
