
<img width="889" height="734" alt="image" src="https://github.com/user-attachments/assets/15676e6e-312a-4edc-b450-c0775c23858b" />

> 对于无序的两数之后，最基本的解放有两种，一个是暴力解法，一个是利用哈希

## 暴力解法

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for(int i=0;i<nums.length;i++){
            for (int j=i+1;j<nums.length;j++){
                if(nums[i]+nums[j]==target){
                    return new int[] {i,j};
                }
            }
        }
        return new int[0];//防止出错
    }
}
```



## 哈希解法

相比于找**nums[i]+nums[j]==target**  可以把问题变形为=>  **nums[i]=target-nums[j]**

从一些数里面找一个数：适合hash表

> 从左边开始，把  nums[j]   :  j    放入到hash表中，那么我在后来指针往后走的时候，只需要去看，之前的hash表中是否有nums[i]=target-nums[j]

<img width="965" height="451" alt="image" src="https://github.com/user-attachments/assets/03f7d795-5593-4c67-b94b-b7709334668b" />


在hash表中的值，只用来保存nums[i]的索引index，如果有不符合条件且相同的健值，可以直接index覆盖

```bash	
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> idx = new HashMap<>(); // 创建一个空哈希表
        for (int j = 0; ; j++) { // 枚举 j
            int x = nums[j];
            // 在左边找 nums[i]，满足 nums[i]+x=target
            // 这里if条件，是判定有没有键值对应的索引，两者有其一，必有其一
            if (idx.containsKey(target - x)) { // 找到了
            	
                return new int[]{idx.get(target - x), j}; // 返回两个数的下标
            }
            // 放入到hash表中
            idx.put(x, j); // 保存 nums[j] 和 j
        }
    }
}

```

