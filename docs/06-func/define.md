# 6.1 函数：把代码打包

## 一、定义与调用

**函数就是"一段有名字、可以被反复调用的代码"。**

```python
def greet(name):
    """向指定的人打招呼"""
    print(f"你好，{name}！")

greet("小明")
greet("小红")
```

**运行结果：**

```text
你好，小明！
你好，小红！
```

**语法拆解：**

| 部分 | 内容 | 说明 |
|---|---|---|
| `def` | 关键字 | 表示"定义一个函数" |
| `greet` | 函数名 | 自己取，命名规则和变量一样 |
| `(name)` | 参数列表 | 函数需要的"输入"，可以有多个或没有 |
| `:` | 冒号 | 必须写 |
| 缩进块 | 函数体 | 函数被调用时执行的代码 |
| `"""..."""` | 文档字符串 | 说明函数用途，可选但强烈建议写 |
| `greet("小明")` | 调用 | 传入实际数据，执行函数 |

!!! tip "函数名也要见名知义"
    好的函数名是一个**动词或动词短语**：`is_prime`、`calculate_area`、`print_menu`。
    看到名字就知道它干什么，这是专业代码的标志。

## 二、参数：函数的"入口"

参数是**函数从外界接收数据的方式**。

```python
def add(a, b):
    """返回两个数的和"""
    return a + b

result = add(3, 5)
print(result)          # 8
```

- `a`、`b` 是**形参**（定义时写的占位符）；
- `3`、`5` 是**实参**（调用时传入的真实数据）；
- 调用时**按顺序一一对应**。

```python
def introduce(name, age, city):
    print(f"{name}今年{age}岁，来自{city}。")

introduce("小明", 13, "湛江")
```

**运行结果：**

```text
小明今年13岁，来自湛江。
```

## 三、`return`：函数的"出口"

**`return` 把结果交回给调用者，并且立即结束函数。**

```python
def calculate_area(length, width):
    """计算长方形面积"""
    return length * width

area = calculate_area(8, 5)
print(f"面积是 {area}")     # 面积是 40
```

!!! danger "`print` 和 `return` 完全不同"
    这是初学函数时最大的困惑：

    | | `print` | `return` |
    |---|---|---|
    | 作用 | 把内容显示在屏幕上 | 把结果交回给调用者 |
    | 能否继续用这个结果 | ❌ 不能 | ✅ 可以 |
    | 示例 | `print(3+5)` 只显示 8 | `x = add(3,5)` 后 x 就是 8 |

    ```python
    def add_wrong(a, b):
        print(a + b)      # ❌ 只打印，没有返回

    def add_right(a, b):
        return a + b      # ✅ 返回结果

    x = add_wrong(3, 5)   # 屏幕上出现 8，但 x 是 None！
    y = add_right(3, 5)   # 屏幕上什么都没有，但 y 是 8
    print(x * 2)          # ❌ TypeError: NoneType 不能相乘
    print(y * 2)          # ✅ 16
    ```

### 没有 `return` 时，函数返回 `None`

```python
def say_hi():
    print("hi")

result = say_hi()
print(result)      # None
```

### 返回多个值

```python
def min_max(nums):
    """同时返回最小值和最大值"""
    return min(nums), max(nums)

low, high = min_max([3, 1, 4, 2])
print(f"最小 {low}，最大 {high}")     # 最小 1，最大 4
```

!!! info "本质上是返回了一个元组"
    `return min(nums), max(nums)` 返回的是一个**元组**，
    左边用 `low, high = ...` 可以"解包"。这是 Python 的常用技巧。

## 四、默认参数

给参数设定"默认值"，调用时可以不传：

```python
def power(base, exponent=2):
    """计算 base 的 exponent 次方，默认求平方"""
    return base ** exponent

print(power(5))        # 25   （用了默认的 exponent = 2）
print(power(5, 3))     # 125  （传入 3，覆盖默认值）
```

!!! warning "默认参数必须放在后面"
    ```python
    def f(a=1, b):     # ❌ SyntaxError
        pass

    def f(a, b=1):     # ✅ 正确
        pass
    ```

## 五、变量的作用域：函数内的变量出不去

```python
def test():
    local_var = 100    # 这个变量只在函数内存在
    print(local_var)

test()                  # 输出 100
print(local_var)        # ❌ NameError: name 'local_var' is not defined
```

**函数内部定义的变量叫"局部变量"，函数执行完就消失了。** 这其实是好事——
不同函数里的同名变量互不干扰，程序更安全。

!!! tip "推荐的写法：函数内部不要直接改外部变量"
    函数应该通过**参数接收数据**、通过 **`return` 返回结果**，
    而不是偷偷修改外面的变量。这样函数才"独立、可复用"。

## 六、什么时候该写函数？

一个实用的判断标准：**如果一段代码你复制粘贴了第二遍，就该把它变成函数。**

| 出现的情况 | 是否该抽成函数 |
|---|---|
| 同样的逻辑写了 2 遍以上 | ✅ 应该 |
| 一段代码超过 20 行，且逻辑独立 | ✅ 建议 |
| 只写了一次，且很简单 | ❌ 不必 |
| 逻辑可以起一个清楚的名字 | ✅ 说明它值得独立 |

## 七、代码实践：重构"判断素数"

把第 4 章的程序改成函数版，并加上"找出 1 到 100 内所有素数"：

```python title="prime_list.py" linenums="1"
def is_prime(n):
    """判断 n 是否为素数（试除法）"""
    if n < 2:
        return False
    for i in range(2, n):
        if n % i == 0:
            return False
    return True

primes = []
for num in range(1, 101):
    if is_prime(num):
        primes.append(num)

print(f"1 到 100 内共有 {len(primes)} 个素数：")
print(primes)
```

**运行结果：**

```text
1 到 100 内共有 25 个素数：
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97]
```

!!! success "重构的收获"
    对比第 4 章的版本：**主程序从 15 行降到 5 行**，而且逻辑一目了然：
    "对 1 到 100 的每个数，如果是素数就加入列表"。
    **这就是函数带来的可读性提升。**

## 八、案例分析：用函数实现"进制转换"

> **问题**：输入一个十进制整数和一个目标进制（2 或 8），输出转换后的结果。

```python title="base_convert.py" linenums="1"
def to_base(n, base):
    """把十进制数 n 转为 base 进制（2 ≤ base ≤ 10），返回字符串"""
    if n == 0:
        return "0"
    digits = ""
    while n > 0:
        digits = str(n % base) + digits     # 取余数，放到最前面
        n = n // base                       # 整除，去掉最后一位
    return digits

number = int(input("请输入十进制数："))
base = int(input("请输入目标进制（2 或 8）："))

print(f"{number} 转为 {base} 进制是：{to_base(number, base)}")
```

**运行结果：**

```text
输入 13，进制 2  → 13 转为 2 进制是：1101
输入 13，进制 8  → 13 转为 8 进制是：15
```

!!! tip "进制转换的算法思想"
    十进制转其他进制，标准方法是**"除基取余，逆序排列"**：
    用 n 不断除以进制数，把每次的余数从后往前拼起来。
    这个算法请理解过程，而不只是记住代码。

## 九、实践任务 6-1：封装"判断闰年"

!!! example "任务说明"
    **输入**：一个起始年份和一个结束年份。
    **操作要求**：
    1. 把第 4 章的闰年判断逻辑封装成函数 `is_leap(year)`，返回 `True` / `False`；
    2. 主程序遍历区间内所有年份，输出所有闰年。
    **预期结果**：输入 `2000` 和 `2020`，输出 2000、2004、2008、2012、2016、2020。
    **评价标准**：
    - 函数定义正确、有返回值（40 分）
    - 主程序遍历正确（30 分）
    - 代码结构清晰（函数与主程序分离）（30 分）

## 十、实践任务 6-2：封装"求最大公约数"

!!! example "任务说明"
    **输入**：两个正整数。
    **操作要求**：写一个函数 `gcd(a, b)` 求最大公约数
    （提示：可以用"辗转相除法"：`gcd(a, b) = gcd(b, a % b)`，直到 `b == 0`）。
    **预期结果**：输入 `48` 和 `18`，输出 `最大公约数是 6`。
    **评价标准**：
    - 函数能正确返回结果（50 分）
    - 理解并能说明辗转相除法的原理（30 分）
    - 处理边界情况（如 `b = 0`）（20 分）

---

下一节：[6.2 递归：函数调用自己 :material-arrow-right:](recursion.md)
