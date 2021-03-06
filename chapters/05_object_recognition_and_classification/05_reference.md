## Glob

glob是python自己带的一个文件操作相关模块，用它可以查找符合自己目的的文件，就类似于Windows下的文件搜索，支持通配符操作，*,?,[]这三个通配符，
*代表0个或多个字符，?代表一个字符，[]匹配指定范围内的字符，如[0-9]匹配数字。它的主要方法就是glob,该方法返回所有匹配的文件路径列表，
该方法需要一个参数用来指定匹配的路径字符串（本字符串可以为绝对路径也可以为相对路径），其返回的文件名只包括当前目录里的文件名，不包括子文件夹里的文件。
glob.glob(r'c:\*.txt')
我这里就是获得C盘下的所有txt文件
glob.glob(r'E:\pic\*\*.jpg')
获得指定目录下的所有jpg文件
使用相对路径：
glob.glob(r'../*.py')


## group

groupby(iterable[, keyfunc])
返回:按照keyfunc函数对序列每个元素执行后的结果分组(每个分组是一个迭代器), 返回这些分组的迭代器
例子:

```python
from itertools import *
a = ['aa', 'ab', 'abc', 'bcd', 'abcde']
for i, k in groupby(a, len):#按照字符串的长度对a的每个元素进行分组
    for m in k:
        print m,
    print i
输出:
aa ab 2
abc bcd 3
abcde 5
```

## defaultdict对象

class collections.defaultdict([default_factory[, ...]])
返回一个新的类似字典的对象。defaultdict是内置dict类的子类。它覆盖一个方法，并添加一个可写的实例变量。其余的功能与dict类相同，这里就不再记录。
第一个参数提供default_factory属性的初始值；它默认为None。所有剩余的参数都视为与传递给dict构造函数的参数相同，包括关键字参数。
defaultdict对象除了支持标准的dict操作，还支持以下方法：

__missing__ （ key ）
如果default_factory属性为None，则以key作为参数引发KeyError异常。
如果default_factory不为None，则不带参数调用它以用来给key提供默认值，此值将插入到字典中用于key，并返回。
如果调用default_factory引发异常，则该异常会保持原样传播。
当未找到请求的key时，此方法由dict类的__getitem__()方法调用；__getitem__()将返回或引发它返回或引发的。
请注意，除了__getitem__()之外的任何操作，都不会调用__missing__()。这意味着get()会像正常的字典一样返回None作为默认值，而不是使用default_factory。
defaultdict对象支持以下实例变量：
default_factory
此属性由__missing__()方法使用；如果构造函数的第一个参数存在，则初始化为它，如果不存在，则初始化为None。


### defaultdict示例

```python
# 使用list作为default_factory，可以很容易地将一系列键值对分组为一个列表字典：
>>>
>>> s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
>>> d = defaultdict(list)
>>> for k, v in s:
...     d[k].append(v)
...
>>> sorted(d.items())
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
```

当每个键第一次遇到时，它不在映射中；因此使用返回空list的default_factory函数自动创建一个条目。然后，list.append()操作将值附加到新列表。当再次遇到这个键时，查找正常继续（返回该键的列表），并且list.append()操作向列表中添加另一个值。这种技术比使用等效的dict.setdefault()技术更简单和更快：

```python
>>>
>>> d = {}
>>> for k, v in s:
...     d.setdefault(k, []).append(v)
...
>>> sorted(d.items())
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
```

将default_factory设置为int可使defaultdict用于计数（如其他语言的bag或multiset）：
```python
>>>
>>> s = 'mississippi'
>>> d = defaultdict(int)
>>> for k in s:
...     d[k] += 1
...
>>> sorted(d.items())
[('i', 4), ('m', 1), ('p', 2), ('s', 4)]
```

当一个字母第一次遇到时，映射中缺少该字母，因此default_factory函数调用int()以提供默认计数零。增量操作然后建立每个字母的计数。
始终返回零的函数int()只是常量函数的特殊情况。创建常量函数的更快和更灵活的方法是使用lambda函数，它可以提供任何常量值（不只是零）：

```python
>>>
>>> def constant_factory(value):
...     return lambda: value
>>> d = defaultdict(constant_factory('<missing>'))
>>> d.update(name='John', action='ran')
>>> '%(name)s %(action)s to %(object)s' % d
'John ran to <missing>'
```

将default_factory设置为set可使defaultdict有助于构建集合字典：

```python
>>>
>>> s = [('red', 1), ('blue', 2), ('red', 3), ('blue', 4), ('red', 1), ('blue', 4)]
>>> d = defaultdict(set)
>>> for k, v in s:
...     d[k].add(v)
...
>>> sorted(d.items())
[('blue', {2, 4}), ('red', {1, 3})]
```

## Lambda

lambda的一般形式是关键字lambda后面跟一个或多个参数，紧跟一个冒号，以后是一个表达式。lambda是一个表达式而不是一个语句。它能够出现在python语法不允许def出现的地方。作为表达式，lambda返回一个值（即一个新的函数）。lambda用来编写简单的函数，而def用来处理更强大的任务。

```python
  1. f = lambda x,y,z : x+y+z  
  2. print f(1,2,3)  
  3.   
  4. g = lambda x,y=2,z=3 : x+y+z  
  5. print g(1,z=4,y=5)
输出
  1. 6  
  2. 10  
```

## Split

Python中有split()和os.path.split()两个函数，具体作用如下：
split()：拆分字符串。通过指定分隔符对字符串进行切片，并返回分割后的字符串列表（list）
os.path.split()：按照路径将文件名和路径分割开

一、函数说明
1、split()函数
语法：str.split(str="",num=string.count(str))[n]
参数说明：
str:表示为分隔符，默认为空格，但是不能为空('')。若字符串中没有分隔符，则把整个字符串作为列表的一个元素
num:表示分割次数。如果存在参数num，则仅分隔成 num+1 个子字符串，并且每一个子字符串可以赋给新的变量
[n]:表示选取第n个分片
注意：当使用空格作为分隔符时，对于中间为空的项会自动忽略

2、os.path.split()函数
语法：os.path.split('PATH')
参数说明：
1.PATH指一个文件的全路径作为参数：
2.如果给出的是一个目录和文件名，则输出路径和文件名
3.如果给出的是一个目录名，则输出路径和为空文件名

二、分离字符串
string = "www.gziscas.com.cn"
1.以'.'为分隔符
print(string.split('.'))
['www', 'gziscas', 'com', 'cn']

2.分割两次
print(string.split('.'，2))
['www', 'gziscas', 'com.cn']

3.分割两次，并取序列为1的项
print(string.split('.',2)[1])
gziscas

4.分割两次，并把分割后的三个部分保存到三个文件
u1, u2, u3 =string.split('.',2)
print(u1)—— www
print(u2)—— gziscas
print(u3) ——com.cn

三、分离文件名和路径
import os
print(os.path.split('/dodo/soft/python/'))
('/dodo/soft/python', '')

print(os.path.split('/dodo/soft/python'))
('/dodo/soft', 'python')

四、实例
str="hello boy<[www.baidu.com]>byebye"
print(str.split("[")[1].split("]")[0])
www.baidu.com

## filter

filter（）函数包括两个参数，分别是function和list。该函数根据function参数返回的结果是否为真来过滤list参数中的项，最后返回一个新列表，如下例所示
```python
>>>a=[1,2,3,4,5,6,7]
>>>b=filter(lambda x:x>5, a)
>>>print b
>>>[6,7]
如果filter参数值为None，就使用identity（）函数，list参数中所有为假的元素都将被删除。如下所示：
>>>a=[0,1,2,3,4,5,6,7]
b=filter(None, a)
>>>print b
>>>[1,2,3,4,5,6,7]
```

## map

map()的两个参数一个是函数名，另一个是列表或元组。
```python
>>>map(lambda x:x+3, a) #这里的a同上
>>>[3,4,5,6,7,8,9,10]
#另一个例子
>>>a=[1,2,3]
>>>b=[4,5,6]
>>>map(lambda x,y:x+y, a,b)
>>>[5,7,9]
```

## reduce

```python
reduce 函数按照指定规则递归求值
>>>reduce(lambda x,y:x*y, [1,2,3,4,5])
>>>120
>>>reduce(lambda x,y:x*y, [1,2,3], 10)
>>>60   # ((1*2)*3)*10
```

## enumerate
enumerate(iteration, start)：返回一个枚举的对象。迭代器(iteration)必须是另外一个可以支持的迭代对象。初始值默认为零，也就是你如果不输入start那就代表从零开始。迭代器的输入可以是列表，字符串，集合等，因为这些都是可迭代的对象。返回一个对象，如果你用列表的形式表现出来的话那就是一个列表，列表的每个元素是一个元组，元祖有两个元素，第一个元素代表编号，也就是第几个元素的意思，第二个元素就是迭代器的对应的元素，这是在默认start为零的情况下。如果不为零，那就是列表的第一个元组的第一个元素就是start的值，后面的依次累加，第二个元素还是一样的意思。
例如：
```python

  1. str1 = 'lplplp'  # string
  2. list1 = [1, 5, 6]  # list
  3. tuple1 = (5, 8, 4, 2)  # 元组
  4. set1 = {'kl', 'lk'}  # 集合
  5.   
  6. print list(enumerate(str1))  
  7. print list(enumerate(str1, start=2))  
  8.   
  9. print list(enumerate(list1))  
  10. print list(enumerate(list1, start=2))  
  11.   
  12. print list(enumerate(tuple1))  
  13. print list(enumerate(tuple1, start=2))  
  14.   
  15. print list(enumerate(set1))  
  16. print list(enumerate(set1, start=2))  

输出：
[(0, 'l'), (1, 'p'), (2, 'l'), (3, 'p'), (4, 'l'), (5, 'p')]
[(2, 'l'), (3, 'p'), (4, 'l'), (5, 'p'), (6, 'l'), (7, 'p')]
[(0, 1), (1, 5), (2, 6)]
[(2, 1), (3, 5), (4, 6)]
[(0, 5), (1, 8), (2, 4), (3, 2)]
[(2, 5), (3, 8), (4, 4), (5, 2)]
[(0, 'lk'), (1, 'kl')]
[(2, 'lk'), (3, 'kl')]
```

## tf.train.shuffle_batch
tf.train.shuffle_batch(tensor_list, batch_size, capacity, min_after_dequeue, num_threads=1, seed=None, enqueue_many=False, shapes=None, name=None)

Creates batches by randomly shuffling tensors.
通过随机打乱张量的顺序创建批次.

简单来说就是读取一个文件并且加载一个张量中的batch_size行

This function adds the following to the current Graph:
这个函数将以下内容加入到现有的图中.

A shuffling queue into which tensors from tensor_list are enqueued.
一个由传入张量组成的随机乱序队列
A dequeue_many operation to create batches from the queue.
从张量队列中取出张量的出队操作
A QueueRunner to QUEUE_RUNNER collection, to enqueue the tensors
from tensor_list.
一个队列运行器管理出队操作.

If enqueue_many is False, tensor_list is assumed to represent a
single example. An input tensor with shape [x, y, z] will be output
as a tensor with shape [batch_size, x, y, z].
If enqueue_many is True, tensor_list is assumed to represent a
batch of examples, where the first dimension is indexed by example,
and all members of tensor_list should have the same size in the
first dimension. If an input tensor has shape [*, x, y, z], the
output will have shape [batch_size, x, y, z].

'enqueue_many’主要是设置tensor中的数据是否能重复,如果想要实现同一个样本多次出现可以将其设置为:“True”,如果只想要其出现一次,也就是保持数据的唯一性,这时候我们将其设置为默认值:"False"
The capacity argument controls the how long the prefetching is allowed to
grow the queues.
容量控制了预抓取操作对于增加队列长度操作的长度.
For example:-

# Creates batches of 32 images and 32 labels.
image_batch, label_batch = tf.train.shuffle_batch(
      [single_image, single_label],
      batch_size=32,
      num_threads=4,
      capacity=50000,
      min_after_dequeue=10000)
Args:

tensor_list: The list of tensors to enqueue.
入队的张量列表
batch_size: The new batch size pulled from the queue.
表示进行一次批处理的tensors数量.
capacity: An integer. The maximum number of elements in the queue.
容量:一个整数,队列中的最大的元素数.
这个参数一定要比min_after_dequeue参数的值大,并且决定了我们可以进行预处理操作元素的最大值.
推荐其值为:
capacity=(min_after_dequeue+(num_threads+a small safety margin∗batchsize)

min_after_dequeue: Minimum number elements in the queue after a
dequeue(出列), used to ensure a level of mixing of elements.
当一次出列操作完成后,队列中元素的最小数量,往往用于定义元素的混合级别.
定义了随机取样的缓冲区大小,此参数越大表示更大级别的混合但是会导致启动更加缓慢,并且会占用更多的内存
num_threads: The number of threads enqueuing tensor_list.
设置num_threads的值大于1,使用多个线程在tensor_list中读取文件,这样保证了同一时刻只在一个文件中进行读取操作(但是读取速度依然优于单线程),而不是之前的同时读取多个文件,这种方案的优点是:
避免了两个不同的线程从同一文件中读取用一个样本
避免了过多的磁盘操作
seed: Seed for the random shuffling within the queue.
打乱tensor队列的随机数种子
enqueue_many: Whether each tensor in tensor_list is a single example.
定义tensor_list中的tensor是否冗余.
shapes: (Optional) The shapes for each example. Defaults to the
inferred shapes for tensor_list.
用于改变读取tensor的形状,默认情况下和直接读取的tensor的形状一致.
name: (Optional) A name for the operations.
Returns:

A list of tensors with the same number and types as tensor_list.
默认返回一个和读取tensor_list数据和类型一个tensor列表.

## tf.where

tf.where(condition, x=None, y=None, name=None)

功能：若x,y都为None，返回condition值为True的坐标;
    若x,y都不为None，返回condition值为True的坐标在x内的值，condition值为False的坐标在y内的值

输入：condition:bool类型的tensor

```python
a = tf.constant([True, False, False, True])
x = tf.constant([1, 2, 3, 4])
y = tf.constant([5, 6, 7, 8])
z = tf.where(a)
z2 = tf.where(a, x, y)

sess = tf.Session()
print(sess.run(z))
print(sess.run(z2))
sess.close()

# z==>[[0]
#      [3]]
# z2==>[ 1 6 7 4]
```

## 标签的匹配
```
train_labels = tf.map_fn(lambda l: tf.where(tf.equal(labels, l)
                                            )[0][0], label_batch, dtype=tf.int64)
```
## 注解：
label_batch是一个[batch_size,1]的张量，labels储存有所有的图片标签的信息，是一个[pictures_num,1]的张量。  
很明显label_batch的行数比picture_num小得多，这时候如果我们直接使用tf.equal函数会出现维度不匹配的问题，使用map_fn主要是将定义的函数运用到后面集合中每个元素中。这里的l其实是label_batch标签张量中的一个秩相同的单个张量。

tf.equal(labels,l)会得到一个[Flase,True,Flase,True,False,False,False]的张量，tf.where会找到此布尔值数组的第一个为True的索引。由于函数返回的是一个二维数组，所以使用[0][0]提取出该值。

## Example
```python
"""主要测试tf.where的使用"""
import tensorflow as tf
import numpy as np


a = np.array([[5]])
a1 = np.array([[1], [2], [3]])
b = np.array([[1], [7], [8], [4], [5], [2], [3], [2], [3]])
# 对于[n,1]shape张量匹配必须使用map_fn函数，否则会出shape函数维度不匹配的错误
c1 = tf.map_fn(lambda l: tf.where(tf.equal(b, l))[0][0], a1, dtype=tf.int64)
c = tf.where(tf.equal(a, b))[0][0]

# c = tf.where(tf.equal(a1, b))[0][0] 这个语句就会出现下面维度不匹配的错误。
# Dimensions must be equal, but are 3 and 7 for 'Equal' (op: 'Equal') with input shapes: [3,1], [7,1].
sess = tf.Session()
print(sess.run(c))
print(sess.run(c1))

# 4
# [0 5 6]

```
