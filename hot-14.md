<img width="676" height="875" alt="image" src="https://github.com/user-attachments/assets/9e3c78b1-b57c-4952-9494-b3174598fa36" />

# 解法一

## 核心思路

### 1.方向数组

用一个二维数组管理四个方向

```java
// 右、下、左、上
int[][] DIRS = {{0,1},{1,0},{0,-1},{-1,0}};
```

### 2.访问标记

直接修改原矩阵，将访问过的数字设为 Integer.MAX_VALUE，无需额外创建布尔数组，节省空间。

### 3.方向切换

用 di = (di + 1) % 4 实现循环切换方向，保证永远顺时针转向。使用di来确定DIRS的位置，以方便实现螺旋访问

## 算法分析
> 1.初始化行列数、结果集合、起始坐标、初始方向；
> 2.遍历总元素次数：
>  · 记录当前位置数字；
>  · 标记当前位置为已访问；
>  · 预判下一步位置是否有效；
>  · 无效则切换方向；
>  · 移动到有效位置；
> 3.返回结果集合。


```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    // 定义四个方向：右、下、左、上（顺时针螺旋的核心方向顺序）
    // DIRS[0] = 向右 → 行不变，列+1
    // DIRS[1] = 向下 → 行+1，列不变
    // DIRS[2] = 向左 → 行不变，列-1
    // DIRS[3] = 向上 → 行-1，列不变
    private static final int[][] DIRS = {{0,1},{1,0},{0,-1},{-1,0}};

    public List<Integer> spiralOrder(int[][] matrix) {
        // m = 矩阵的行数，n = 矩阵的列数
        int m = matrix.length;
        int n = matrix[0].length;
        // 初始化结果集合，预分配空间（矩阵总元素数 = m*n）
        List<Integer> ans = new ArrayList<>(m*n);

        // i：当前遍历的 行坐标，j：当前遍历的 列坐标（初始位置：左上角 (0,0)）
        int i = 0;
        int j = 0;
        // di：当前使用的方向索引（初始为0 → 向右）
        int di = 0;

        // 循环遍历：矩阵总共有 m*n 个元素，需要走 m*n 步
        for(int k = 0; k < m*n; k++) {
            // 1. 把当前位置的数字加入结果集
            ans.add(matrix[i][j]);
            // 2. 标记当前位置为已访问（用Integer.MAX_VALUE表示已访问，避免重复遍历）
            matrix[i][j] = Integer.MAX_VALUE;

            // 3. 计算【下一个位置】的坐标（按照当前方向走一步）
            int x = i + DIRS[di][0];
            int y = j + DIRS[di][1];

            // 4. 判断：下一个位置是否 越界 或 已经访问过
            // 越界：x/y 小于0 或 大于等于行列数
            // 已访问：matrix[x][y] == Integer.MAX_VALUE
            if(x < 0 || x >= m || y < 0 || y >= n || matrix[x][y] == Integer.MAX_VALUE){
                // 满足条件 → 需要改变方向：顺时针转90°（右→下→左→上 循环）
                di = (di + 1) % 4;
            }

            // 5. 更新坐标：走到【新的有效位置】
            i += DIRS[di][0];
            j += DIRS[di][1];
        }
        // 返回螺旋遍历结果
        return ans;
    }
}
```



# 解法二

## 解题思路（分层边界收缩法）

**核心思想
按层顺时针遍历，每走完一行 / 一列，就收缩对应边界，不标记、不判断越界：**

> 1.把矩阵看成一层一层的矩形边框，从外到内顺时针遍历每一层；
> 2.遍历顺序固定：右 → 下 → 左 → 上，走完一个方向立刻切换；
> 3.每走完一行，下次需要走的列数 -1；
> 4.每走完一列，下次需要走的行数 -1；
> 5.用 m/n 交换 + 减1 实现边界自动向内收缩；
> 6.遍历完所有元素（结果集长度 = 总元素数）时结束

### 关键技巧（最核心）

1、初始坐标偏移

j = -1：让起点在矩阵左侧外面，第一步向右走刚好进入矩阵 (0,0)。

2、方向循环

di = (di + 1) % 4 实现 右→下→左→上 自动切换。

3、边界收缩公式

```java
int temp = n;
n = m - 1;
m = temp;
```

走完一行（向右 / 左），下次走列时长度 -1

走完一列（向上 / 下），下次走行时长度 -1


```java

import java.util.ArrayList;
import java.util.List;

class Solution {
    // 方向数组：右下左上（顺时针螺旋的4个方向）
    // DIRS[0] 右 → 行不变，列+1
    // DIRS[1] 下 → 行+1，列不变
    // DIRS[2] 左 → 行不变，列-1
    // DIRS[3] 上 → 行-1，列不变
    private static final int[][] DIRS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

    public List<Integer> spiralOrder(int[][] matrix) {
        // m = 矩阵行数，n = 矩阵列数
        int m = matrix.length;
        int n = matrix[0].length;
        // 矩阵总元素个数（遍历结束的判断条件）
        int size = m * n;
        // 结果集合，预分配空间
        List<Integer> ans = new ArrayList<>(m * n);

        // 初始坐标：i=0（第0行），j=-1（列初始在最左侧外面，方便第一步向右走直接进入矩阵）
        int i = 0;
        int j = -1;

        // di = 当前方向，循环规则：右→下→左→上 不断切换
        // 循环终止条件：结果集大小 = 总元素数（遍历完成）
        for (int di = 0; ans.size() < size; di = (di + 1) % 4) {
            // 沿着当前方向，一次性走完 一行 或 一列
            // 第一次：向右走 n 步（走完一行）
            // 第二次：向下走 m-1 步（走完一列）
            // 第三次：向左走 n-1 步（走完一行）
            // 第四次：向上走 m-2 步（走完一列）
            // …… 以此类推
            for (int k = 0; k < n; k++) {
                // 按照当前方向移动一步
                i += DIRS[di][0];
                j += DIRS[di][1];
                // 把当前位置数字加入结果
                ans.add(matrix[i][j]);
            }

            // 核心技巧：交换 m 和 n，并且 m 减 1，实现「边界向内收缩」
            // 走完一行 → 下次走列时长度-1
            // 走完一列 → 下次走行时长度-1
            int temp = n;
            n = m - 1;
            m = temp;
        }

        // 返回螺旋遍历结果
        return ans;
    }
}

```
