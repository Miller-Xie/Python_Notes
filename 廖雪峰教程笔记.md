## Python基础

### 数据类型

#### 整数

* Python可以处理任意大小的整数，包括负整数
* 十六进制用0x和0~9以及a~f表示

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