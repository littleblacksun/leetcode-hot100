<img width="1228" height="690" alt="image" src="https://github.com/user-attachments/assets/ea8188fc-4ec1-4de2-8aa4-364f88cef28b" />


# 思路：先按照左端点排序，再来判断右端点


以示例 1 为例，我们有 [1,3],[2,6],[8,10],[15,18] 这四个区间。

在左端点排好序之后，以[1,3]为例，我们需要判断后面的区间：

> 他的左端点小于3，并且他的右端点要>=3，那么这种情况就可以并入一个新区间

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals,(p,q) -> p[0] - q[0]);//按照左端点排序
        List<int[]> ans = new ArrayList<>();
        for ( int[] p : intervals){
            int m = ans.size();
            if( m>0 && p[0] <= ans.get(m-1)[1]){
                //如果 m=0（列表为空），直接把第一个区间加进去，不用判断合并
                //可以合并的情况
                //翻译：当前区间的左端点 ≤ 最后一个合并区间的右端点
                //因为区间已经按左端点排序，左端点不用改，只需要把右端点更新为两个区间中更大的那个。
                ans.get(m-1)[1] = Math.max(ans.get(m-1)[1],p[1]);
            }
            else{
                ans.add(p);
            }
        }
        return ans.toArray(new int [ans.size()][]);
    }
}
```
