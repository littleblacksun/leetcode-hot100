<img width="659" height="723" alt="image" src="https://github.com/user-attachments/assets/84aaa3bb-e8ac-454e-81c1-97cb7b26bb9e" />

## 题意解读

针对于字母异位词，如果重新排序之后，字符串相同，就可以视作为字母异位词

## 如下所示

eat tea ate -> aet
tan nat -> ant
bat -> bat
所以只需要把字符串排序之后相同的原字符串，放在同一个hash列表中即可 



``` java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        // 1. 定义哈希表：key是排序后的字符串，value是该key对应的所有字母异位词列表
        Map<String,List<String>> m = new HashMap<>();
        
        // 2. 遍历字符串数组中的每一个字符串
        for(String s : strs){
            // 3. 将字符串s转为字符数组（因为String是不可变的，排序需要操作字符数组）
            char[] sortedS = s.toCharArray();
            /* 
               4. Arrays.sort()：对字符数组进行升序排序
               - 作用：把字母异位词转换成完全相同的字符序列（比如 "eat" → ['a','e','t']，"tea" → ['a','e','t']）
               - 入参：char[] 类型的数组
               - 特性：原地排序（直接修改原数组），默认按Unicode编码升序（字母a-z对应编码升序，正好符合需求）
            */
            Arrays.sort(sortedS);
            
            // 5. new String(char[])：将排序后的字符数组转回字符串（作为哈希表的key）
            //    比如 sortedS是['a','e','t']，转回后就是 "aet"
            String key = new String(sortedS);
            
            /* 
               6. Map.computeIfAbsent()：核心API，分两步执行
               - 第一步：检查哈希表m中是否存在key（排序后的字符串）；
               - 第二步：
                 ✅ 如果不存在：执行第二个参数（Lambda表达式）创建新的ArrayList，插入到哈希表中，然后返回这个ArrayList；
                 ✅ 如果存在：直接返回已有的ArrayList；
               - 入参1：要检查的key（String类型）；
               - 入参2：Function接口（这里用Lambda简化：_ -> new ArrayList<>()），_表示忽略入参（因为创建ArrayList不需要额外参数）；
               - 后续的.add(s)：将原字符串s添加到上述返回的ArrayList中；
               - 对比传统写法（等价于下面这段代码）：
                 if (!m.containsKey(key)) {
                     m.put(key, new ArrayList<>());
                 }
                 m.get(key).add(s);
               - 优势：一行完成“检查-创建-获取”，比传统写法更简洁、高效（减少一次get操作）
            */
            m.computeIfAbsent(key, _ -> new ArrayList<>()).add(s);
        }
        
        /* 
           7. Map.values()：获取哈希表中所有的value集合（即所有字母异位词分组的列表）
              - 返回值类型：Collection<List<String>>
           8. new ArrayList<>(Collection)：将Collection类型的values转为List<List<String>>（符合方法返回值要求）
              - 入参：Collection类型的集合
              - 作用：因为方法返回值是List<List<String>>，而m.values()是Collection，需要转换为ArrayList
        */
        return new ArrayList<>(m.values());
    }
}

```
