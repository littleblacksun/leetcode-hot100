<img width="725" height="502" alt="image" src="https://github.com/user-attachments/assets/3da5facd-7ba1-4551-bd03-882a89cb27d6" />

# 思路：数组翻转

该方法为数组的翻转：我们可以先将所有元素翻转，这样尾部的 kmodn 个元素就被移至数组头部，然后我们再翻转 [0,kmodn−1] 区间的元素和 [kmodn,n−1] 区间的元素即能得到最后的答案。

<img width="427" height="306" alt="image" src="https://github.com/user-attachments/assets/eadbc1dc-d1c9-4c9f-a8ec-aba2e2a2c71c" />

```java
public void rotate(int[] nums, int k) {
    k %= nums.length;       // 第1行：处理超大 k
    reverse(nums,0,nums.length-1);  // 第2行：反转整个数组
    reverse(nums,0,k-1);    // 第3行：反转前k个
    reverse(nums,k,nums.length-1);  // 第4行：反转后面所有
}

public void reverse(int[] nums, int start, int end){
    while(start < end){
        // 交换 start 和 end 位置的数
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] =  temp;
        
        start++;  // 左指针右移
        end--;    // 右指针左移
    }
}
```
