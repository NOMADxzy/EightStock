## Python

### 一、数据类型

#### 分类

1. 数字类型

   整数(int)

   ```python
   # Python 3 中的整数是任意精度的，即它的大小只受内存限制。
   a = 100
   ```

   浮点数(float)

   ```python
   # 遵循 IEEE 754 标准，是有符号的双精度浮点数(64位)
   b = 3.14159
   ```

2. 序列类型

   字符串(str)

   ```python
   # 不可变的字符序列，用于表示文本数据。
   s = "hello"
   ```

   列表(list)

   ```python
   # 动态大小，可以存储不同类型的数据
   lst = [1, 2, 3, "a", True]
   ```

   元组(tuple)

   ```python
   # 不可变，创建后不能修改。
   tup = (1, 2, 3)
   ```

   range对象

   ```python
   # 一个整数序列
   r = range(5)
   print(type(r))  # <class 'range'>
   ```

3. 映射类型

   dict

   ```python
   # 无序的键值对, 键唯一、值不唯一
   d = {"name": "Alice", "age": 25}
   ```

4. 布尔类型

   bool

   ```python
   # 是 int 的子类，True 表示 1，False 表示 0
   flag = True
   ```

5. 集合类型

   set

   ```python
   # 无序的不重复,支持数学上的集合操作（如并集、交集、差集）。
   s = {1, 2, 3, 4}
   ```

6. None类型

   None

   ```python
   # 空值或无值
   x = None
   print(type(x))  # <class 'NoneType'>
   ```

7. 二进制类型

   bytes

   ```python
   # 不可变的字节序列
   b = b"hello"
   print(type(b))  # <class 'bytes'>
   ```

   bytearray

   ```python
   # 可变的字节序列
   ba = bytearray(b"hello")
   print(type(ba))  # <class 'bytearray'>
   ```

#### 一切皆对象

Python 中的一切都是对象，包括基本的数据类型（如整数、浮点数、字符串等）。

```python
# int 是由类 int 定义的对象。
# float 是由类 float 定义的对象。

a = 10
b = 10
print(id(a))  # 输出对象 a 的内存地址
print(id(b))  # 输出对象 b 的内存地址，和 a 一样，因为 Python 共享了相同的数值对象
```

- 可变类型：列表、字典、集合、自定义对象等
- 不可变类型：元组、字符串、数值、布尔

#### 更改对象

- 不可变类型 更改后 会指向新的地址

- 可变类型中

  - 字典、集合 更改后地址不发生变化

    ```python
    matrix = [{} for _ in range(2)]
    tmp_dict = matrix[0]
    tmp_dict[0] = 0
    print(matrix)
    for i in range(1000):
        tmp_dict[i] = i
    print(matrix)
    ```
  
  - 数组更改后分情况：
  
    ```
    1. 小规模添加元素，不需要扩展容量时，列表地址不会改变
    2. 现有的容量不足以容纳新元素，列表的地址会发生变化
    ```
  



#### 不可变对象

##### 字典——Dict

```

```

从 Python 3.7 开始，字典在 for 枚举时是**按插入顺序有序的**。

### 二、浅拷贝 和 深拷贝

这里说的拷贝通常只针对**可变类型**：

列表、字典、集合、自定义对象

字符串、数值、元组类型是不可变类型，它们的值是固定的，无论是浅拷贝还是深拷贝，得到的结果都一样——它们都只是指向相同的对象。copy.copy() 都会返回对原始对象的引用

```python
import copy

# 原始列表
original_list = [[1, 2], [3, 4], 5]

# 创建拷贝
shallow_copy_list = copy.copy(original_list)
deep_copy_list = copy.deepcopy(original_list)

# 修改深拷贝中的可变对象
deep_copy_list[0][0] = 'a'
deep_copy_list[1].append(6)

print("Original List:", original_list)
print("Deep Copy:", deep_copy_list)

# 修改浅拷贝中的可变对象
shallow_copy_list[0][0] = 'a'
shallow_copy_list[1].append(6)

print("Original List:", original_list)
print("Shallow Copy:", shallow_copy_list)
```

copy() 方法实际是通过调用对象的 __copy__ 方法来实现的。

```python
import copy

class MyClass:
    def __init__(self, value):
        self.value = value

    def __copy__(self): # 可以自己实现该方法
        return MyClass(self.value)

obj = MyClass(10)
shallow_copy_obj = copy.copy(obj)
```

### 三、魔术方法

以双下划线开头和结尾，这些方法允许我们定义对象的行为，对象的初始化、复制、表示、比较、数学运算等。

`__init__(self)` 是对象的构造函数，用于初始化一个类的新实例。

```python
def __init__(self, value):
        # 在对象创建时初始化属性
        self.value = value
```

`__copy__(self)	` 用于定义对象的浅拷贝行为。

```python
def __copy__(self):
        # 自定义浅拷贝的行为
        new_obj = MyClass(self.value)
        return new_obj
```

`__str__(self)`：定义当使用 str() 或 print() 函数时，返回的对象字符串表示。

```python
def __str__(self):
    return "This is a string representation of the object"
```

`__add__(self, other)：`定义加法 (+) 的行为。

```python
def __add__(self, other):
    return self.value + other.value
```

`__len__(self)：`定义对象的长度

```python
def __len__(self):
    return len(self.data)
```