<img width="878" height="883" alt="image" src="https://github.com/user-attachments/assets/5c6cbc56-b0e9-4018-a907-4d8447fd74c7" />

由于短板效应，那么容器里面可以接的水最大容量应该和两边之间最短的决定

设left=0 : 左指针 right=nums.length - 1 : 右指针

那么应该比较nums[left]  &  nums[right] 哪一个更小，谁小移动谁

**解析**

在移动两边的时候，首先，宽在变短，那么要想接雨水量变大，那么必须高变大，只有短边换成更长的边，才可以做到

因此，在移动短边时，只有遇见比自己更高的边时再判断即可。同长也不行

```java
class Solution {
    public int maxArea(int[] height) {
        int ans = 0;
        int left = 0;
        int right = height.length - 1;
        while(left<right)
        {
            int area = (right - left) * Math.min(height[left],height[right]);
            ans = Math.max(ans,area);
            if (height[left]<height[right]){
                left++;
            }
            else{
                right--;
            }
        }

        return ans;
    }
}
```
