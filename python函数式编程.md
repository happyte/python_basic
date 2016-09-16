# **python第四天\(函数式编程\)**

## **高阶函数**

* 1.变量可以指向函数
  ```
   >>>f = abs
   >>>f
   <built-in function abs>
   >>>f(-10)
   10

  ```

  函数名其实就是指向函数的变量！对于`abs`这个函数，完全可以把函数名`abs`看成变量，它指向一个可以计算绝对值的函数！
* 2.假如把`abs`指向其他对象
  ```
   >>>abs = 10
   >>>abs(-10)
   Traceback (most recent call last):
       File "<stdin>",line 1,in <module>
   TypeError: 'int' object is not callable

  ```

  把`abs`指向10后，就无法通过`abs(-10)`调用该函数，因为`abs`已经指向一个整数`10`。
* 3.既然变量可以指向函数，函数的参数能接收变量，那么一个函数可以接收另一个函数作为参数，这种函数被称为高阶函数:
  ```
   >>>def add(x,y,f):
          return f(x)+f(y)

  ```


## **map\/reduce**

* 1.`map()`函数接收两个参数，一个是函数，一个是`Iterable`,`map`将传入的函数依次作用于序列的每一个元素，并把结果作为新的`Iterator`返回。
  ```
   >>>list(map(str,[1,2,3,4,5]))
      ['1','2','3','4','5']

  ```

* 2.`reduce`把一个函数作用在一个序列`[x1,x2,x3,...]`上，`reduce`把结果继续和序列的下一个元素做累计计算：
  ```
   reduce(f,[x1,x2,x3,x4]) = f(f(f(x1,x2),x3),x4)

  ```

* 3.如果把系列`[1,3,5,7,9]`变换成整数`13579`
  ```
   >>> from functools import reduce
   >>> def fn(x,y):
           return 10*x + y
   >>> reduce(fn,[1,3,5,7,9])

  ```

* 4.把_'13579'_变成_13579_，配合`map`函数,其实就是个`int`函数
  ```
   >>>from functools import reduce
   >>>def str2int(s):
          def fn(x,y):
              return x*10 + y
          def char2num(s):
              return {'0':0,'1':1,'2':2,'3':3,'4':4,'5':5,'6':6,'7':7,'8':8,'9':9}[s]
          return reduce(fn,map(char2num,'13579'))

  ```


## **filter**

* 1.`filter()`也接收一个函数和一个序列，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是删除该元素。
  ```
   >>>def is_odd(n):
          return n%2 == 1
      list(filter(is_odd,[1,2,3,4,5])

  ```

  `filter()`函数返回的是一个`Iterator`,也是个惰性序列

## **sorted**

* 1.Python内置的`sorted()`函数可以对list进行排序:
  ```
   >>>sorted([10,5,-1,6,-9])
      [-9,-1,5,6,10]

  ```

* 2.`sorted()`函数也是个高阶函数，还可以接收一个`key`函数实现自定义排序，例如绝对值排序
  ```
   >>>sorted([10,5,-1,6,-9],key=abs)
   [-1,5,6,-9,10]

  ```

* 3.字符串忽略大小写排序
  ```
   >>>sorted(['bob','about','Zoo','Credit'],key=str.lower)
      ['about','bob','Credit','Zoo']

  ```

* 4.要进行反向排序的话，传入第三个参数`reverse=True`
  ```
   >>>sorted(['bob','about','Zoo','Credit'],key=str.lower,reverse=True)
      ['Zoo','Credit','bob','about']

  ```


## **返回函数**

* 1.高阶函数除了可以接受函数作为参数外，还可以把函数作为结果返回。
  ```
   >>>def lazy_sum(*args):
          def sum():
              ax = 0
              for n in args:
                  ax += n
              return ax
          return sum

  ```

  当我们掉用`lazy_sum()`时，返回的并不是求和结果，而是求和函数:
  ```
   >>>f = lazy_sum(1,3,5,7,9)
   >>>f
      <function lazy_sum. <local>.sum at 0x101c6ed90>

  ```

  调用函数`f`时，才真正计算求和的结果:
  ```
   >>>f()
   25

  ```

  我们在函数`lazy_sum`中右定义了函数`sum`，并且内部函数`sum`可以引用外部函数`lazy_sum`的参数和局部变量，当`lazy_sum`返回函数`sum`时，相关参数和变量都保存在返回的函数中，这种做法称为`闭包`。

## **练习**

利用`map()`函数，把用户输入的不规范的英文名字，变为首字母大写，其它小写的规范名字。输入:`['adam','LISA','barT']`,输出:`['Adam','Lisa','Bart']`。

```
   def normalize(s):
       a = s[0].upper()
       for i in range(1,len(s)):
           a += s[i].lower()
       return a
   >>>L=['adam','LISA','barT']
   map(normalize,L)

```

利用`map`和`reduce`函数编写一个`str2float`函数，把字符串`'123.456'`转换成浮点数`123.456`

```
   from __future__ import division
   def str2float(s):
       s = s.split('.')

       def f1(x,y):
           return x*10 + y

       def f2(x,y):
           return x/10 + y

       def str2num(str):
           return {'0':0,'1':1,'2':2,'3':3,'4':4,'5':5,'6':6,'7':7,'8':8,'9':9}
       return reduce(f1,list(map(str2num,s[0])))+reduce(f2,list(map(str2num,s[1]))[::-1])/10

```

滤出0～1000的非回文数

```
   def is_palindrome(n):
       return str(n) == str(n)[::-1] #字符串同样可以切片操作
   filter(is_palindrome,range(1000))

```

假设我们用一组tuple表示学生姓名和成绩`L=[('Bob',75),('Adam',92),('Bart',66),('Lisa',88)]`，请用`sorted()`对上述列表分别按名字和成绩排序:

```
   from operator import itemgetter
   def by_name(t):
       return sorted(t,key=itemgetter(0))
   def by_score(t):
       return sorted(t,key=itemgetter(1),reverse = True)
```

