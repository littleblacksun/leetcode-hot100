<img width="620" height="388" alt="image" src="https://github.com/user-attachments/assets/99a522d5-db78-4e7a-904d-5ab83422987f" />

# 方法一：把nums当作栈

用栈来记录非零元素

<img width="450" height="365" alt="image" src="https://github.com/user-attachments/assets/94e046c6-ecec-4038-bccb-3eac6e4b5e8f" />

最后在栈的末尾加上两个0

为了做到O(1)空间复杂度，直接把nums当作栈，用一个变量stackSize表示栈的大小，初始值为0.

入栈就是把nums[stackSize]置为nums[i]，然后把stackSize加1

最后把nums中下标从stackSize到n-1的数都置为0。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int stackSize = 0;
        for (int x : nums)
        {
            if(x!=0){
                nums[stackSize++] = x;
            }
        }
        Arrays.fill(nums,stackSize,nums.length,0);
    }
}
```

# 方法二：双指针+交换元素

方法一在最坏情况下（nums 全为 0），需要遍历 nums 两次。能否做到一次遍历？

**核心思路**

把 0 视作空位。我们要把所有非零元素都移到数组左边的空位上，并保证非零元素的顺序不变。

例如 nums=[0,0,1,2]，把 1 放到最左边的空位上，数组变成 [ 1 ,0, 0 ,2]。注意 1 移动过去后，在原来 1 的位置又产生了一个新的空位。也就是说，我们交换了 nums[0]=0 和 nums[2]=1 这两个数。

为了保证非零元素的顺序不变，我们需要维护最左边的空位的位置（下标）。

**具体思路**

从左到右遍历 nums[i]。同时维护另外一个下标i0（初始值为0）,并其希望[i0,i-1]之间都是0,并且i0指向左边的0；

在i向后遍历nums[i]时，当遍历到nums[i]!=0时，交换nums[i0] 与 nums[i],然后把i0和i加一，因为这个时候，i0之前的都不为0了

<img width="565" height="430" alt="image" src="https://github.com/user-attachments/assets/add4cf51-cefc-42e3-943d-5988a886d4f2" />

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int i0=0;
        for(int i = 0;i<nums.length;i++){
            if(nums[i]!=0)
            {
                int tmp = nums[i];
                nums[i] = nums[i0];
                nums[i0] = tmp;
                i0++;
            }
        }
    }
}
```

