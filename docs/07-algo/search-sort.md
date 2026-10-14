# 7.2 查找与排序

## 一、查找：在数据里找一个目标

### 1. 顺序查找（线性查找）

**从头到尾一个一个比，找到就返回位置，找不到返回 -1。**

```python title="linear_search.py" linenums="1"
def linear_search(nums, target):
    """顺序查找：返回 target 的下标，找不到返回 -1"""
    for i in range(len(nums)):
        if nums[i] == target:
            return i
    return -1

data = [23, 5, 78, 12, 99, 45]
print(linear_search(data, 99))    # 4
print(linear_search(data, 100))   # -1
```

**运行结果：**

```text
4
-1
```

| 优点 | 缺点 |
|---|---|
| 实现简单，**不要求数据有序** | 数据量大时慢，最多要比 n 次 |

### 2. 二分查找（折半查找）

**前提：数据必须已经排好序。**

**思路**：每次看中间那个数——

- 如果正好是目标，找到了；
- 如果中间数比目标大，说明目标在**左半边**，去左边找；
- 如果中间数比目标小，说明目标在**右半边**，去右边找。

**每比较一次，搜索范围就缩小一半。**

```python title="binary_search.py" linenums="1"
def binary_search(nums, target):
    """二分查找（要求 nums 已升序排列），返回下标，找不到返回 -1"""
    left = 0
    right = len(nums) - 1

    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1          # 目标在右半边
        else:
            right = mid - 1         # 目标在左半边

    return -1

data = [3, 8, 15, 22, 37, 48, 56, 61, 79, 94]
print(binary_search(data, 37))     # 4
print(binary_search(data, 100))    # -1
```

**运行结果：**

```text
4
-1
```

!!! warning "三个最容易写错的地方"
    1. **循环条件是 `left <= right`**，写成 `<` 会漏掉只剩一个元素的情况；
    2. **更新时要 `mid + 1` 或 `mid - 1`**，写成 `left = mid` 会导致**死循环**；
    3. **前提是数据有序**。对无序数据用二分查找，结果是错的（不报错，但找不到）。

!!! tip "效率对比"
    在 100 万个有序数字里查找：

    | 方法 | 最多比较次数 | 数据翻倍时 |
    |---|---|---|
    | 顺序查找 | 1,000,000 次 | 次数翻倍 |
    | 二分查找 | 约 20 次 | 只多 1 次 |

    **这就是 O(n) 与 O(log n) 的差距。**

## 二、排序：把数据排成顺序

排序不只是为了"好看"——**有序数据能让后续的查找、统计、比较都变简单**。
下面是两种最简单的排序算法，理解它们的思路比记住代码更重要。

### 1. 冒泡排序

**思路**：反复比较**相邻**的两个元素，如果顺序不对就交换。
每一轮结束后，最大的元素会"冒"到末尾。

```python title="bubble_sort.py" linenums="1"
def bubble_sort(nums):
    """冒泡排序（升序），原地修改并返回列表"""
    n = len(nums)
    for i in range(n - 1):                 # 一共需要 n-1 轮
        swapped = False                    # 记录本轮是否发生过交换
        for j in range(n - 1 - i):         # 每轮结束后，末尾 i 个已排好
            if nums[j] > nums[j + 1]:
                nums[j], nums[j + 1] = nums[j + 1], nums[j]
                swapped = True
        if not swapped:                    # 本轮没有交换，说明已经有序
            break
    return nums

print(bubble_sort([5, 2, 9, 1, 5, 6]))
```

**运行结果：**

```text
[1, 2, 5, 5, 6, 9]
```

!!! success "`swapped` 这个小优化很关键"
    如果某一轮**一次交换都没发生**，说明数据已经有序，可以提前结束。
    对基本有序的数据，这让冒泡排序接近 O(n)。
    **"提前发现可以停止"是一种重要的优化思路。**

### 2. 选择排序

**思路**：每一轮从未排序部分里**选出最小的**，放到已排序部分的末尾。

```python title="selection_sort.py" linenums="1"
def selection_sort(nums):
    """选择排序（升序）"""
    n = len(nums)
    for i in range(n - 1):
        min_idx = i                        # 先假设 i 处最小
        for j in range(i + 1, n):
            if nums[j] < nums[min_idx]:
                min_idx = j                # 找到更小的就记下位置
        nums[i], nums[min_idx] = nums[min_idx], nums[i]   # 交换
    return nums

print(selection_sort([64, 25, 12, 22, 11]))
```

**运行结果：**

```text
[11, 12, 22, 25, 64]
```

### 3. 两种排序对比

| 对比项 | 冒泡排序 | 选择排序 |
|---|---|---|
| 比较次数 | O(n²) | O(n²) |
| 交换次数 | 较多（可能多次交换） | **每轮最多交换 1 次（较少）** |
| 是否稳定 | 稳定（相等元素相对顺序不变） | 不稳定 |
| 能否提前结束 | ✅ 可以（`swapped` 优化） | ❌ 不行 |

!!! note "实际比赛怎么办？"
    竞赛中**不要手写排序**——直接用 Python 内置的 `sorted()` 或 `list.sort()`，
    它们的效率远高于冒泡和选择排序（内部用的是 Timsort 算法）。

    学习手写排序的目的是：**理解"排序是怎么实现的"，为以后学更复杂的算法打基础。**
    ```python
    nums = [5, 2, 9, 1]
    nums.sort()                  # 原地升序
    nums.sort(reverse=True)      # 原地降序
    result = sorted(nums)        # 返回新列表
    ```

## 三、案例分析：用二分查找解决"猜数字"

> **问题**：在 1 到 100 之间猜一个数，用最少的次数必能猜中。

**策略**：每次猜中点，根据"大了/小了"砍掉一半。**这就是二分查找的现实版。**

```python title="guess_binary.py" linenums="1"
secret = 73
left, right = 1, 100
count = 0

while left <= right:
    mid = (left + right) // 2
    count += 1
    print(f"第 {count} 次猜：{mid}")
    if mid == secret:
        print(f"猜中了！一共猜了 {count} 次")
        break
    elif mid < secret:
        left = mid + 1
    else:
        right = mid - 1
```

**运行结果：**

```text
第 1 次猜：50
第 2 次猜：75
第 3 次猜：62
第 4 次猜：68
第 5 次猜：71
第 6 次猜：73
猜中了！一共猜了 6 次
```

!!! success "为什么是 6 次？"
    100 个数，每猜一次范围减半：100 → 50 → 25 → 13 → 7 → 4 → 2 → 1，
    只需约 log₂100 ≈ 7 次以内。**这就是二分法的威力。**

## 四、实践任务 7-1：统计重复元素

!!! example "任务说明"
    **输入**：一个整数列表，例如 `[3, 1, 4, 1, 5, 9, 2, 6, 5, 3]`。
    **操作要求**：找出其中所有**重复出现**的元素，并去重后升序输出。
    **预期结果**：输出 `[1, 3, 5]`。
    **评价标准**：
    - 能找出重复元素（50 分）
    - 结果去重且有序（30 分）
    - 提示：可以先排序，重复元素就会相邻（20 分）

## 五、实践任务 7-2：实现二分查找

!!! example "任务说明"
    **输入**：一个**已升序排列**的列表和一个目标值。
    **操作要求**：手写二分查找函数，返回目标值的下标（找不到返回 -1）。
    **预期结果**：`[1,3,5,7,9,11]` 中查找 `7`，返回 `3`。
    **评价标准**：
    - 循环条件与边界更新正确（60 分）
    - 能处理"找不到"的情况（20 分）
    - 能说明为什么必须数据有序（20 分）

---

下一节：[7.3 递推与贪心入门 :material-arrow-right:](recurrence.md)
