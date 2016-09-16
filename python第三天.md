# **python第三天\(高级特性\)**

## **切片**

* 1.取出一个_list_或_tuple_的部分元素：
  ```
   >>>L = ['Michael','Sarah','Tracy']
   >>>L[0:2]
   ['Michael','Sarah','Tracy']

  ```

  _Python_提供的切片_\(Slice\)_操作符，`L[0:2]`表示从索引_0_开始，直到索引_2_为止。即索引_0，1，2_，如果从索引0开始，还可以省略为
  ```
   >>>L[:2]

  ```

* 2.Python也支持`L[-1]`取倒数第一个元素，也支持倒数切片
  ```
   >>>L[-2:]
   ['Sarah','Tracy']
   >>>L[-2:-1]
   ['Sarah']

  ```

* 3.创建0~99的数列，通过切片取出某一段数列
  * 前10个数，每2个取1个
    ```
    >>>L = list(range(100))
    >>>L[:10:2]
    [0,2,4,6,8]

    ```

  * tuple也是一种list，也可以切片操作，结果仍是tuple
    ```
    >>>(0,1,2,3,4,5)[:3]
    (0,1,2)

    ```

  * 字符串'xxx'也可以看成一种list，也有切片操作
    ```
    >>>'ABCDEFG'[:3]
    'ABC'
    >>>'ABCDEFG'[::2]
    'ACEG'

    ```



## **迭代**

在`Python`中，迭代是通过`for ... in`完成的，不仅仅可以作用在list或tuple上，还可以作用于其它可迭代对象上。只要是可迭代对象，无论有无下标，都可以迭代：

* 1.
  ```
   >>>d = {'a':1,'b':2,'c':3}
   >>>for key in d:
          print(key)
   a
   b
   c

  ```

  因为`dict`不是按`list`按照顺序排列的，所依迭代出来的结果可能不一样。默认情况下，dict迭代的是key,如果要迭代value。用`for value in d.values()`,同时要迭代key和value,可以用`for k,v in d.items()`。
* 2.如何判断一个对象是否是可迭代对象，通过`collections`模块的`Iterable`类型判断
  ```
   >>>from collections import Iterable
   >>>isinstance('abc',Iterable)
   True
   >>>isinstance(123,Iterable)
   False

  ```

* 3.`Python`内置的`enumerate()`函数可以把一个list变成索引－元素对，可以同时迭代出索引和元素本身:
  ```
   >>>for i,value in enumerate(['a','b','c']):
          print(i,value)
   0 a
   1 b
   2 c

  ```


## **列表生成器**

* 1.例如生成`[1*1,2*2,.......,10*10]`的序列
  ```
   >>>[x*x for x in range(1,11)]
   [1,4,9,16,25,36,49,64,81,100]

  ```

* 2.还可以使用两层循环，生成全排列
  ```
   >>>[m + n for m in 'ABC' for n in 'XYZ']
   ['AX','AY','AZ','BX','BY','BZ','CX','CY','CZ']

  ```

* 3.列表生成器也可以使用两个变量来生成list:
  ```
   >>>d = {'x':'A','y':'B','z':'C'}
   >>>[k + '=' + v for k,v in d.items()]
   ['x=A','y=B','c=Z']

  ```


## **生成器**

* 1.要创建一个生成器，只需把列表生成器的\[\]变成\(\)，就创建了一个generator:
  ```
   >>>L = (x*x for in x in range(1,11))
   >>>g
   <generator object <genexpr> at 0x1022ef630>

  ```

  如果要一个一个打印出来，可以通过`next(g)`返回，没有更多元素时，抛出`StopIteration`错误
* 2.不断使用next\(g\)的方法一般不用，正确的方法是使用`for`迭代，因为generator也是可迭代对象:
  ```
   >>> g = (x*x for x in range(1,11))
   >>> for i in g:
           print(i)

  ```

* 3.可以用函数实现，例如斐波那契数列
  ```
   def fib(max):
       n,a,b = 0,0,1
       while(n < max):
           yield b
           a,b = b,a+b
           n += 1

  ```

  如果一个函数中包含了yield关键字，那么这个函数就不是一个普通函数，而是一个`generator`:
  ```
   >>> f = fib(6)
   >>> f
   <generator object fib at 0x104feaaa0>

  ```

  `generator`和函数的执行流程不一样，函数遇到`return`或者最后一行就返回。而变成`generator`的函数，在每次掉用`next()`的时候执行，遇到`yield`语句返回，再次执行时从上次返回的yield语句处继续执行。

