## Python基础

### 基本数据类型

#### 整数

* Python可以处理任意大小的整数，包括负整数
* 十六进制用`0x`和`0~9`以及`a~f`表示

#### 浮点数

* 科学计数法表示：1.23X10<sup>9</sup>表示为1.23e9或12.3e8

* **整数**和**浮点数**在计算机内部存储的方式是不同的，整数运算永远是**精确**的，而浮点数运算则可能会有四舍五入的误差。

#### 字符串

* `'`或`"`表示，当`'`本身是字符时，用`""`将字符串括起来，如`"I'm miller"`

* `\`作为转义字符，`\\`表示`\`，`\n`表示换行，`\t`表示制表符

* `r''`表示`''`内部的字符不进行转义

* 表示多行字符时，除了`\n`，还可以使用`'''...'''`表示
  
   * 在命令行模式下：
   
   ```bash
   >>> print('''line1
   ... line2
   ... line3''')
   line1
   line2
   line3
   ```

   * 在`.py`文件中：
   
   ```python
   print('''line1
   line2
   line3''')
   ```
   
   **注意**：`r'''...'''`能够去除`''''''`内的转义

#### 布尔值

* 两种值：`True`和`False`
* 三种运算：`or` ，`and`和`not`

#### 空值

`None`表示，`None`不能理解为`0`，因为`0`是有意义的，而`None`是一个特殊的空值。



### 变量和常量

#### 变量

**Python**是**动态语言**，可以将任何数据类型赋值给变量

对于语句：

```
a = 'ABC'
```

**Python**解释器干了两件事情：

1. 在内存中创建了一个`'ABC'`的字符串；
2. 在内存中创建了一个名为`a`的变量，并把它指向`'ABC'`。

对于语句：

```python
a = 'ABC'
b = a
a = 'XYZ'
print(a)
print(b)
#打印XYZ
#打印ABC
```

执行`a = 'ABC'`，解释器创建了字符串`'ABC'`和变量`a`，并把`a`指向`'ABC'`：

![py-var-code-1](https://www.liaoxuefeng.com/files/attachments/923791878255456/0)

执行`b = a`，解释器创建了变量`b`，并把`b`指向`a`指向的字符串`'ABC'`：

![py-var-code-2](https://www.liaoxuefeng.com/files/attachments/923792058613440/0)

执行`a = 'XYZ'`，解释器创建了字符串'XYZ'，并把`a`的指向改为`'XYZ'`，但`b`并没有更改：

![py-var-code-3](https://www.liaoxuefeng.com/files/attachments/923792191637760/0)

所以，最后打印变量`b`的结果自然是`'ABC'`了。

#### 常量

在Python中，通常用全部大写的变量名表示常量：

```python
PI = 3.14159265359
```

**注意**：用全部大写的变量名表示常量只是一个**习惯**的用法，如果你一定要改变变量`PI`的值，也没人能拦住你。



### 运算

整数的两种除法：`/`和`//`

* `/`是精确的除法，结果是浮点数

```bash
>>> 10 / 3
3.3333333333333335
```

* `//`是**地板除**，结果为整数

```
>>> 10 // 3
3
```



### 字符串和编码

#### 字符编码

1. `ASCII`编码：美国发明，主要表示**英文字母**，1个字节表示；

2. `Unicode`编码：Unicode把所有语言都统一到一套编码里，最常用的是用**两个字节**表示一个字符（如果要用到非常偏僻的字符，就需要4个字节）。现代操作系统和大多数编程语言都直接支持Unicode。

字母`A`用ASCII编码是十进制的`65`，二进制的`01000001`；

字符`0`用ASCII编码是十进制的`48`，二进制的`00110000`，注意字符`'0'`和整数`0`是不同的；

汉字`中`已经超出了ASCII编码的范围，用Unicode编码是十进制的`20013`，二进制的`01001110 00101101`。

3. `UTF-8`编码：“可变长编码”，可以**节省存储空间**。将字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。

用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件：

![rw-file-utf-8](https://www.liaoxuefeng.com/files/attachments/923923787018816/0)

浏览网页的时候，服务器会把动态生成的Unicode内容转换为UTF-8再传输到浏览器：

![web-utf-8](https://www.liaoxuefeng.com/files/attachments/923923759189600/0)



#### Python的字符串

* 在**Python 3**版本中，字符串是以Unicode编码的，支持多语言
* 对于单个字符，`ord()`函数获取字符的整数表示，`chr()`函数把编码转换为对应的字符

```
>>> ord('A')
65
>>> ord('中')
20013
>>> chr(66)
'B'
>>> chr(25991)
'文'
```



由于Python的字符串类型是`str`，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把`str`变为以字节为单位的`bytes`。

Python对`bytes`类型的数据用带`b`前缀的单引号或双引号表示：

```
x = b'ABC'
```

要注意区分`'ABC'`和`b'ABC'`，前者是`str`，后者虽然内容显示得和前者一样，但`bytes`的每个字符都只占用一个字节。



* 以Unicode表示的`str`通过`encode()`方法可以编码为指定的`bytes`，例如：

```
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>>  
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

**注意**：纯英文的`str`可以用`ASCII`编码为`bytes`，内容是一样的，含有中文的`str`可以用`UTF-8`编码为`bytes`。含有中文的`str`无法用`ASCII`编码，因为中文编码的范围超过了`ASCII`编码的范围，Python会报错。

* 如果我们从网络或磁盘上读取了字节流，那么读到的数据就是`bytes`。要把`bytes`变为`str`，就需要用`decode()`方法：

```
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

如果`bytes`中包含无法解码的字节，`decode()`方法会报错：

```
>>> b'\xe4\xb8\xad\xff'.decode('utf-8')
Traceback (most recent call last):
  ...
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
```

如果`bytes`中只有一小部分无效的字节，可以传入`errors='ignore'`忽略错误的字节：

```
>>> b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
'中'
```

要计算`str`包含多少个字符，可以用`len()`函数：

```
>>> len('ABC')
3
>>> len('中文')
2
```

`len()`函数计算的是`str`的字符数，如果换成`bytes`，`len()`函数就计算**字节数**：

```
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```



#### 格式化

在Python中，采用的格式化方式和C语言是一致的，用`%`实现，举例如下：

```
>>> 'Hello, %s' % 'world'
'Hello, world'
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
```

在字符串内部，`%s`表示用字符串替换，`%d`表示用整数替换，有几个`%?`占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个`%?`，括号可以省略。

常见的占位符有：

| 占位符 | 替换内容     |
| :----- | :----------- |
| %d     | 整数         |
| %f     | 浮点数       |
| %s     | 字符串       |
| %x     | 十六进制整数 |

其中，格式化整数和浮点数还可以指定是否补0和整数与小数的位数：

```python
print('%2d-%02d' % (3, 1))
print('%.2f' % 3.1415926)
# 3-01
#3.14
```

##### format()

用传入的参数依次替换字符串内的占位符`{0}`、`{1}`……

```
>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
'Hello, 小明, 成绩提升了 17.1%'
```



### 内置数据类型

#### 列表list

**定义**：一种有序的集合，可以随时添加和删除其中的元素。

定义空列表：

```python
lt = []
lt = list()
```

* `len()`得到列表的长度
* `append(a)`在列表末尾添加元素`a`
* `insert(i,a)`在索引`i`位置插入`a`
* `pop()`删除列表末尾的元素并返回，`pop(i)`删除索引为`i`的元素并返回

#### 元组tuple

**定义**：和list类似，但是一旦初始化就不能修改。

定义空元组：

```python
tp = ()
tp = tuple()
```

定义一个元素的元组：

```python
tp = (a,)
```

**注意**：

1. tuple没有`append`、`insert`等操作函数

2. 当tuple的每个元素都不能改变时，tuple的内容才不可变

例如，当tuple中有元素类型为list时：

```
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```

**解释**：

定义的时候tuple包含的3个元素：

![tuple-0](https://www.liaoxuefeng.com/files/attachments/923973516787680/0)

当我们把list的元素`'A'`和`'B'`修改为`'X'`和`'Y'`后，tuple变为：

![tuple-1](https://www.liaoxuefeng.com/files/attachments/923973647515872/0)

> tuple所谓的“不变”是指，tuple的每个元素，指向永远不变。



#### 字典dict

**定义**：也叫map，使用键值对(`key-value`)存储，key不能重复，具有很快的查询速度。

* 创建dict：

```python
d = {'Miller':180,'Pony':175,'Tony':170}
```

* 获取value的方法：

1. 调用d[key]得到，如果key不存在，会报错；

1. 调用d.get("key")，如果key不存在，会返回`None`或者自己指定的值。

   ```
   >>> d.get('Thomas')
   >>> d.get('Thomas', -1)
   -1
   ```

* 删除key的方法：pop(key)，对应的value也会删除

**注意**：

1. dict内部存储的顺序和key放入的顺序没有关系
2. dict的key必须是**不可变对象**，如整数、字符串、tuple都是不可变，而列表list是可变的，不能作为key

#### 集合set

**定义**：是key的集合，不存储value，key不能重复。

* 创建set需要list作为输入集合：

```python
s = set([1,2,3])
#或者 s = {1,2,3}
print(s)
# {1,2,3}
```

* 添加元素`add(key)`(可以重复添加，但是没有用)
* 删除元素：`remove(key)`
* 交并集操作：

```python
s1 = set([1,2,3])
s2 = set([2,3,4])
s3 = s1 & s2
s4 = s1 | s2
print(s3)
#{2,3}
print(s4)
#{1,2,3,4}
```

**注意**：

* 和dict类似，set不能放入可变对象

#### 不可变对象

* 对于可变对象内部进行操作，内容是会变化的，如list

```python
lt = [3,2,1]
lt.sort()
print(lt)
#[1,2,3]
```

* 对于不可变对象，如str，调用replace：

```python
a = 'abc'
b = a.replace('a','A')
print(a)
#abc
print(b)
#Abc
```

**解释**：`a`是变量，`'abc'`是字符串对象，`a`指向的内容是`'abc'`

```ascii
┌───┐                  ┌───────┐
│ a │─────────────────>│ 'abc' │
└───┘                  └───────┘
```

在调用`a.replace('a','A')`之后，创建了一个新字符串`'Abc'`并返回，变量`a`仍指向原有的字符串对象`'abc'`，而变量`b`指向新字符串`'Abc'`

```ascii
┌───┐                  ┌───────┐
│ a │─────────────────>│ 'abc' │
└───┘                  └───────┘
┌───┐                  ┌───────┐
│ b │─────────────────>│ 'Abc' │
└───┘                  └───────┘
```

>  对于不变对象来说，调用对象自身的任意方法，也不会改变该对象自身的内容，而是创建新的对象并返回。
>  **多任务环境**下同时读取不变对象不需要加锁，可以同时读。因此，编写程序时，尽量设计一个为不变对象。



## 函数

### 定义和调用函数

* 定义函数：使用`def`语句

* 空函数使用`pass`语句，`pass`可以用来作为占位符，比如现在还没想好怎么写函数的代码

```python
def func():
    pass
```

* 返回多个值：实际上是返回tuple。在语法上，返回一个tuple可以省略括号，而多个变量可以同时接收一个tuple

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny

x, y = move(100, 100, 60, math.pi / 6)
print(x,y)
#151.96152422706632 70.0
```

* 函数传入的参数如果进行类型检查，可以用内置函数`isinstance()`实现



### 函数参数

#### 位置参数

* 最普通的必选参数即为位置参数。

```python
def power(x):
    return x * x
```

对于`power(x)`函数，参数`x`就是一个位置参数。



#### 默认参数

当函数的某个参数大多数情况下不变的时候，可以考虑默认参数。

例如，我们经常计算x<sup>2</sup>,可以将第二个参数默认设为2

```
def power(x, n=2):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s
```

**注意**：

* 必选参数在前，默认参数在后
* 当函数有多个参数时，将变化大的参数放前面，变化小的参数放后面，那么变化小的参数就可以作为默认参数
* 默认参数的好处：降低函数调用的难度
* 如果不按顺序提供默认参数，需要将参数名写上
* 默认实参要使用不变对象，例如

```python
def add_end(L=[]):
    L.append('END')
    return L
```

对于函数`add_end`，向传入的list添加`END`，然后返回。但是多次调用`add_end()`之后，结果不正确

```python
add_end()
#['END']
add_end()
#['END','END']
```

**解释**：Python函数在定义的时候，默认参数`L`的值就被计算出来了，即`[]`，因为默认参数`L`也是一个变量，它指向对象`[]`，每次调用该函数，如果改变了`L`的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的`[]`了。

可以使用`None`进行改进：

```python
def add_end(L=None):
    if L is None:
        L = []
    L.append('END')
    return L
```



#### 可变参数

定义：在参数前加`*`号

```python
def func(*nums):
    sum = 0
    for a in nums:
        sum += a
    return sum

func(1,2)
func(1,2,3)

nums = [1,2,3]
func(nums[0],nums[1],nums[2])
func(*nums)
```

在函数`func`内部，参数`nums`接受的是一个tuple。在调用时，可以传入任意个参数(包括0个)，同时可以在tuple或者list前面加`*`号传给可变参数



#### 关键字参数

定义：关键字允许传入0个或者任意多个参数，它们在函数内部自动组成装成一个dict

```python
def func1(name,age,**k):
    print('name:' , name , 'age:' , age , 'other:' , k)
    
func1('miller',23,height = 180,job = IT)
#name: miller age: 23 other: {'height': 180}

d = {'height':180}
func1('miller',23,**d)
#name: miller age: 23 other: {'height': 180}
```

**注意**：调用函数时，参数`k`获得一个dict，它是`d`的一份拷贝，函数内的更改不会影响`d`本身。



#### 命名关键字参数



#### 参数组合

