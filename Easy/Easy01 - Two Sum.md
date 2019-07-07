# Easy01 - Two Sum
title: Module Easy01 - Two Sum
date: 2016/03/03 20:46:25
categories:
- LeetCode
tags: LeetCode-Easy
---


# 1 题目

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

```java
Example:

Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

```

# 2 个人解法

## 2.1 个人解法一

最开始的想法是使用两个 for 循环来遍历数组，这是最简单的，也是时间复杂度最高的：O(n * n)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int[] tempSum = {-1, -1};
        int firstNum;
        int secordNum;
        if (nums.length == 1 && nums[0] == target) {
            return new int[]{0};
        }
        int i;
        for (i = 0;i < nums.length - 1; ++i) {
            firstNum = nums[i];
            int j = 0;
            for (j = i + 1; j < nums.length; ++j) {
                secordNum = nums[j];
                if (secordNum + firstNum == target) {
                    tempSum[0] = i;
                    tempSum[1] = j;
                }
            }
        }
        return tempSum;
    }
}
```

## 2.2 个人解法二

后来想到了一个去掉 2 * for 循环的方法，可以用集合来存储元素和下标的映射关系，然后便利数组，看 target - nums[i] 的元素是否在集合中即可；

时间复杂度；O(n)

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> containMap = new HashMap<Integer, Integer>();
        int i;
        for (i = 0; i < nums.length; ++i) {
            containMap.put(nums[i], i);
        }
        for (i = 0; i < nums.length; ++i) {
            int otherNum = target - nums[i];
            if (containMap.containsKey(otherNum) && i != containMap.get(otherNum)) {
                return new int[]{i, containMap.get(otherNum)};
            }
        }
        return new int[]{0};
    }
}
```

# 3 官方解法

> 作者：LeetCode
链接：https://leetcode-cn.com/problems/two-sum/solution/liang-shu-zhi-he-by-leetcode-2/
来源：力扣（LeetCode）

## 3.1 方法一：暴力法
暴力法很简单，遍历每个元素 x，并查找是否存在一个值与 target - x 相等的目标元素。

```java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[j] == target - nums[i]) {
                return new int[] { i, j };
            }
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
**复杂度分析**：

- 时间复杂度：O(n^2)， 对于每个元素，我们试图通过遍历数组的其余部分来寻找它所对应的目标元素，这将耗费 O(n) 的时间。因此时间复杂度为 O(n^2)。

- 空间复杂度：O(1)。

## 3.2 方法二：两遍哈希表
为了对运行时间复杂度进行优化，我们需要一种更有效的方法来检查数组中是否存在目标元素。如果存在，我们需要找出它的索引。保持数组中的每个元素与其索引相互对应的最好方法是什么？哈希表。

通过以空间换取速度的方式，我们可以将查找时间从 O(n) 降低到 O(1)。哈希表正是为此目的而构建的，它支持以 近似恒定的时间进行快速查找。我用“近似”来描述，是因为一旦出现冲突，查找用时可能会退化到 O(n)。但只要你仔细地挑选哈希函数，在哈希表中进行查找的用时应当被摊销为 O(1)。

一个简单的实现使用了两次迭代。在第一次迭代中，我们将每个元素的值和它的索引添加到表中。然后，在第二次迭代中，我们将检查每个元素所对应的目标元素（target - nums[i]）是否存在于表中。注意，该目标元素不能是 nums[i] 本身！

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[] { i, map.get(complement) };
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
**复杂度分析**：

- 时间复杂度：O(n)， 我们把包含有 n 个元素的列表遍历两次。由于哈希表将查找时间缩短到 O(1) ，所以时间复杂度为 O(n)。

- 空间复杂度：O(n)， 所需的额外空间取决于哈希表中存储的元素数量，该表中存储了 n 个元素。

## 3.3 方法三：一遍哈希表
事实证明，我们可以一次完成。在进行迭代并将元素插入到表中的同时，我们还会回过头来检查表中是否已经存在当前元素所对应的目标元素。如果它存在，那我们已经找到了对应解，并立即将其返回。

```Java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
**复杂度分析：**

- 时间复杂度：O(n)， 我们只遍历了包含有 nn 个元素的列表一次。在表中进行的每次查找只花费 O(1) 的时间。

- 空间复杂度：O(n)， 所需的额外空间取决于哈希表中存储的元素数量，该表最多需要存储 n 个元素。





