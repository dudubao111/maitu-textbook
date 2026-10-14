# 5.2 字符串：文本的处理艺术

## 一、字符串也是"一串字符"

字符串你早就在用了（第 3 章），但那时只把它当"一句话"。其实——
**字符串就是"字符组成的列表"**，索引、切片、遍历的规则和列表几乎一样。

```python
text = "Python"
# 索引：  0 1 2 3 4 5
# 负索引：-6 -5 -4 -3 -2 -1

print(len(text))       # 6      长度
print(text[0])         # P      第一个字符
print(text[-1])        # n      最后一个字符
print(text[0:3])       # Pyt   切片（不包含索引 3）
print(text[::-1])      # nohtyP  反转（常用技巧！）
```

!!! danger "字符串不可修改"
    列表可以改，字符串**不能**：

    ```python
    text = "Python"
    text[0] = "J"      # ❌ TypeError: 'str' object does not support item assignment
    ```

    **想改，只能造一个新的字符串：**

    ```python
    text = "Python"
    text = "J" + text[1:]      # 拼出一个新字符串 "Jython"
    print(text)                # Jython
    ```

## 二、遍历字符串

```python
word = "码途"

for ch in word:
    print(ch)

for i, ch in enumerate(word):
    print(f"第 {i} 个字符是 {ch}")
```

**运行结果：**

```text
码
途
第 0 个字符是 码
第 1 个字符是 途
```

## 三、常用字符串方法

| 方法 | 作用 | 示例 | 结果 |
|---|---|---|---|
| `len(s)` | 长度 | `len("abc")` | `3` |
| `s.upper()` | 全转大写 | `"abc".upper()` | `"ABC"` |
| `s.lower()` | 全转小写 | `"AbC".lower()` | `"abc"` |
| `s.strip()` | 去掉两端空白 | `" hi ".strip()` | `"hi"` |
| `s.replace(a, b)` | 把 a 换成 b | `"a-b".replace("-", "+")` | `"a+b"` |
| `s.split(x)` | 按 x 切分成**列表** | `"1,2,3".split(",")` | `["1","2","3"]` |
| `x.join(lst)` | 用 x 把列表**拼成字符串** | `"-".join(["a","b"])` | `"a-b"` |
| `s.find(x)` | 找 x 首次出现的下标 | `"abc".find("b")` | `1`（找不到返回 `-1`） |
| `s.count(x)` | 统计 x 出现次数 | `"aba".count("a")` | `2` |
| `s.startswith(x)` | 是否以 x 开头 | `"abc".startswith("a")` | `True` |
| `s.isdigit()` | 是否全是数字 | `"123".isdigit()` | `True` |

!!! tip "两个最重要的方法：`split` 和 `join`"
    - **`split`：把一句话拆成列表**——处理输入时极其常用；
    - **`join`：把列表拼成一句话**——输出时极其常用。

    ```python
    line = "88 92 85 76"
    scores = line.split(" ")        # 拆成 ["88", "92", "85", "76"]
    print(scores)                   # ['88', '92', '85', '76']

    nums = [int(x) for x in scores] # 转成整数
    print(nums)                     # [88, 92, 85, 76]

    result = "-".join(["a", "b", "c"])
    print(result)                   # a-b-c
    ```

    **竞赛题输入经常是"一行里用空格隔开的几个数"**，
    标准处理套路就是：**`input().split()` → 转成数字**。

## 四、竞赛必会：一行读入多个数字

这是信息学竞赛中最常见的输入格式。**请把下面这段代码记牢。**

```python title="read_line.py" linenums="1"
# 输入形如：3 5
line = input()                    # 读到的是字符串 "3 5"
parts = line.split()              # 按空白切分 → ["3", "5"]

a = int(parts[0])
b = int(parts[1])

print(f"a = {a}, b = {b}, a + b = {a + b}")
```

**运行结果（输入 `3 5`）：**

```text
a = 3, b = 5, a + b = 8
```

!!! success "更简洁的写法（推荐熟悉后使用）"
    ```python
    a, b = map(int, input().split())
    print(a + b)
    ```

    **拆解**：`input().split()` 得到 `["3", "5"]`；
    `map(int, ...)` 把每个元素都转成整数；`a, b = ...` 把两个结果分别赋给 a、b。

    这种写法是竞赛选手的"肌肉记忆"，**建议先理解原理，再练习使用**。

## 五、代码实践：统计字符出现次数

> **问题**：输入一行文字，统计其中**每个字符出现的次数**。

```python title="count_chars.py" linenums="1"
text = input("请输入一行文字：")

counts = {}                       # 用字典存"字符 → 次数"（第 7 章会细讲）
for ch in text:
    if ch in counts:
        counts[ch] += 1
    else:
        counts[ch] = 1

for ch, n in counts.items():
    print(f"'{ch}' 出现了 {n} 次")
```

**运行示例（输入 `hello`）：**

```text
'h' 出现了 1 次
'e' 出现了 1 次
'l' 出现了 2 次
'o' 出现了 1 次
```

!!! note "这里用到了字典"
    `counts = {}` 是**字典**（键值对集合），`counts[ch]` 表示"字符 ch 对应的次数"。
    它是第 7 章的补充内容。本节的**核心练习目标**是"遍历字符串 + 判断 + 累加"。
    如果觉得字典陌生，可以先用两个平行列表来实现（见下面的实践任务）。

## 六、案例分析：判断回文串

> **问题**：判断一个字符串是否"回文"（正读和反读都一样），如 `"上海自来水来自海上"`。

**思路 1：字符串反转对比（最简单）**

```python title="palindrome.py" linenums="1"
text = input("请输入一个字符串：")

if text == text[::-1]:
    print("是回文串")
else:
    print("不是回文串")
```

**运行结果：**

```text
输入 abccba → 是回文串
输入 abc    → 不是回文串
```

**思路 2：双指针（竞赛常用）**

```python title="palindrome_two_pointer.py" linenums="1"
text = input("请输入一个字符串：")

left = 0
right = len(text) - 1
is_palindrome = True

while left < right:
    if text[left] != text[right]:
        is_palindrome = False
        break
    left += 1
    right -= 1

print("是回文串" if is_palindrome else "不是回文串")
```

!!! success "两种思路的对比"
    | 思路 | 优点 | 缺点 |
    |---|---|---|
    | 反转对比 | 代码只有 1 行，最直观 | 会额外复制一份字符串，占内存 |
    | 双指针 | 不需要额外空间，效率高 | 代码稍长 |

    竞赛中数据量大时，双指针更优。**"用两个指针从两端向中间靠拢"**
    是字符串类题目的经典套路，请务必掌握。

## 七、实践任务 5-3：单词计数

!!! example "任务说明"
    **输入**：一行英文句子，例如 `"I love Python and I love coding"`。
    **操作要求**：
    1. 用 `split()` 把它拆成单词列表；
    2. 输出单词总数；
    3. 输出最长的那个单词及其长度。
    **预期结果**：单词总数 `7`；最长单词 `coding`，长度 `6`。
    **评价标准**：
    - 正确使用 `split()`（30 分）
    - 正确统计个数（20 分）
    - 能找到最长单词（提示：先假设第一个最长，再逐个比较）（50 分）

## 八、实践任务 5-4：格式化输出学生信息

!!! example "任务说明"
    **输入**：一行形如 `"张三 15 92.5"` 的文本（姓名 年龄 成绩）。
    **操作要求**：用 `split()` 拆分，分别转成合适的类型，
    按 `姓名：张三　年龄：15　成绩：92.5` 的格式输出。
    **预期结果**：输出格式整洁，数值类型正确（成绩保留 1 位小数）。
    **评价标准**：
    - 能正确拆分并转换类型（40 分）
    - 输出格式符合要求（40 分）
    - 能说明为什么年龄用 `int`、成绩用 `float`（20 分）

---

## 本章小结

!!! abstract "第 5 章 小结"
    **1. 列表是有序集合。** 用 `[]` 创建，索引**从 0 开始**，支持负数索引和切片，
    切片同样遵守"左闭右开"。

    **2. 列表核心操作。** `append` 追加、`insert` 插入、`remove`/`pop` 删除、
    `len`/`sum`/`max`/`min` 统计、`sort` 排序（注意 `sort()` 无返回值）。

    **3. 三种遍历方式。** 直接遍历元素 / 用 `range(len())` 遍历索引 /
    用 `enumerate` 同时拿索引和值。需要位置时用 `enumerate`。

    **4. 字符串像列表。** 索引、切片、遍历规则一致，但**字符串不可修改**，
    改动要新建字符串。`s[::-1]` 是最快的反转写法。

    **5. 两个必会方法。** `split`（字符串 → 列表）和 `join`（列表 → 字符串）。

    **6. 竞赛输入套路。** `a, b = map(int, input().split())`
    处理"一行多个数字"，务必练熟。

    **7. 格式控制。** `f"{x:.2f}"` 保留两位小数，竞赛题常考。

## 练习与参考

!!! question "练习 1（读代码）"
    下面代码输出什么？

    ```python
    lst = [10, 20, 30, 40, 50]
    print(lst[1])
    print(lst[-1])
    print(lst[1:4])
    print(len(lst))
    ```

    ??? success "参考答案与评价要点"
        ```text
        20
        50
        [20, 30, 40]
        5
        ```
        评价要点：第 3 行答成 `[20, 30, 40, 50]` 说明没掌握左闭右开，扣 25 分。

!!! question "练习 2（改错）"
    下面代码想给列表末尾加一个元素并排序，但输出不对。请找出问题。

    ```python
    nums = [3, 1, 4, 2]
    result = nums.sort()
    print(result)
    ```

    ??? success "参考答案与评价要点"
        **问题**：`sort()` 是**原地排序**，没有返回值，所以 `result` 是 `None`。

        **正确写法**：
        ```python
        nums = [3, 1, 4, 2]
        nums.sort()
        print(nums)          # [1, 2, 3, 4]
        # 或者
        result = sorted(nums)
        print(result)        # [1, 2, 3, 4]
        ```

        评价要点：能指出"`sort()` 返回 `None`"即得满分（100 分）。

!!! question "练习 3（编程）"
    输入一行用空格隔开的整数，输出它们的和、最大值、最小值。

    ??? success "参考答案与评价要点"

        ```python
        nums = list(map(int, input().split()))
        print(f"和：{sum(nums)}")
        print(f"最大值：{max(nums)}")
        print(f"最小值：{min(nums)}")
        ```

        输入 `5 3 8 1 9` → 和 26、最大值 9、最小值 1。

        评价要点：能正确读入并转换（50 分）；三项结果正确（50 分）。

!!! question "练习 4（编程）"
    统计一个字符串中**元音字母**（a、e、i、o、u，不分大小写）的个数。

    ??? success "参考答案与评价要点"

        ```python
        text = input("请输入字符串：").lower()
        count = 0
        for ch in text:
            if ch in "aeiou":
                count += 1
        print(f"元音字母有 {count} 个")
        ```

        输入 `Hello World` → `3`（e、o、o）

        评价要点：正确使用 `lower()` 统一大小写（30 分）；
        判断逻辑正确（40 分）；结果正确（30 分）。

!!! question "练习 5（综合）"
    输入一个字符串，判断它是否是"回文串"（忽略大小写）。
    例如 `"Level"` 应判定为回文。

    ??? success "参考答案与评价要点"

        ```python
        text = input("请输入字符串：").lower()
        if text == text[::-1]:
            print("是回文串")
        else:
            print("不是回文串")
        ```

        评价要点：能想到先 `lower()` 再比较（40 分）；
        使用切片反转（30 分）；结果正确（30 分）。

---

上一节：[5.1 列表：批量管理数据 :material-arrow-left:](list.md) ｜
下一章：[第 6 章 函数与模块化 :material-arrow-right:](../06-func/index.md)
