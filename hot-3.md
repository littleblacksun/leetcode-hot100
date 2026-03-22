<img width="681" height="499" alt="image" src="https://github.com/user-attachments/assets/45bec7e2-ed90-4313-98ad-000b025101b7" />

由于排序的时间负责度是o(nlgn)，不符合题目o(n)的要求，

# 核心思路

对于nums中的元素x，以x为起点，不断查找下一个数，x+1,x+2,....是否在nums中，并统计序列的长度。

为了O(n)时间复杂度的要求，有两个**优化关键**

1、把nums的数，都放在hash集合中，这样子可以O(1)判断数字是否在nums中。

2、如果 x−1 在哈希集合中，则不以 x 为起点。为什么？因为以 x−1 为起点计算出的序列长度，一定比以 x 为起点计算出的序列长度要长！这样可以避免大量重复计算。比如 nums=[3,2,4,5]，从 3 开始，我们可以找到 3,4,5 这个连续序列；而从 2 开始，我们可以找到 2,3,4,5 这个连续序列，一定比从 3 开始的序列更长。

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> st = new HashSet<>();
        for (int num : nums){
            st.add(num);//把nums转成hash集合
        }

        int ans = 0;

        for (int x : st){//遍历hash集合
            if(st.contains(x-1)){//如果 x 不算序列的起点，直接跳过
                continue;
            }

            int y=x+1;
            while(st.contains(y))//不断查找下一个数是否在hash集合中
                {
                    y++;
                }
            //循环结束后，y-1是最后一个在hash集合中的数
            ans = Math.max(ans,y-x);
        }

        return ans;
    }
}

```
