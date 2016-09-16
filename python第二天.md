# **_python_**_第二天_

## **调用函数**

* 1.要掉用一个函数，需要执导函数的名称和参数，可以通过`help('abs')`查看`abs`函数
* 2._python_内置的常用函数还包括数据类型转换函数

  ```
   >>>int('123')
      123
   >>>str(1.23)
      '1.23'

  ```


## **定义函数**

* 1.在_python_中，定义一个函数用`def`语句，依次写出函数名、括号、括号内的参数和冒号`:`,然后在缩进块中编写函数体，函数的返回值用`return`语句返回。例如:

  ```
   def my_abs(x):
       if x>= 0:
           return x
       else:
           return -x

  ```

  假如已经把`my_abs`的函数定义保存为`abstest.py`文件，那么可以在该文件的当前目录下启动python解释器，用`from abstest import my_abs`来导入`my_abs()`函数，注意`abstest`是文件名\(不含`.py`扩展名\)

* 2.定义一个什么也不做的空函数，可以用`pass`语句

  ```
   >>>def nop():
          pass

  ```

  比如没有想好代码怎么写，可以用`pass`先当个占位符

* 3.参数检查,参数个数不对时,_python_解释器会检查出并抛出`TypeError`

  ```
   >>>my_abs(1,2)
          TypeError: my_abs() takes 1 positional argument but 2 were given

  ```

  但是参数类型不对，_python_解释器就无法帮我们检查

  ```
   def my_abs(x):
       if not isinstance(x,(int,float)):
           raise TypeError('bad operand type')
       if x >= 0:
           return x
       else:
           return -x

  ```

* 3.函数可以同时返回多个值，但其实就是一个_tuple_,而多个变量可以同时接受一个_tuple_，按位置赋给对应的值。


## **函数的参数**

* 1.位置参数

  ```
   def power(x):
       return x*x

  ```

  参数`x`就是一个位置参数，位置参数是必须传入的，有多个参数依次传入

* 2.默认参数

  ```
   def power(x,n=2):
       s = 1;
       while n>0:
            n -= 1
            s = s*x
       return s

  ```

  默认参数可以简化函数的调用，要注意几点:

  * 一、必选参数在前，默认参数在后
  * 二、当函数有多个参数，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以用默认参数。


```
   def enroll(name,gender,age=10,city='Ningbo'):
       print('name',name)
       print('gender',gender)
       print('age',age)
       print('city',city)

```

有多个默认参数时，调用的时候，既可以按顺序提供默认参数，比如掉用`enroll('Bob','M',7)`,`city`参数没有提供，仍然使用默认值。当不按顺序提供部分默认参数时，需要把参数名写上。比如调用`enroll('Adam','M',city='Hangzhou')`,表示`city`参数是传进去的，其它参数继续使用默认值。

* 3.可变参数

  ```
   def calc(*numbers):
       sum = 0
       for n in numbers:
           sum += n*n
       return sum

  ```

  定义一个可变参数和定义一个_list_和_tuple_参数相比，仅仅在参数前面加了一个`*`号。如果已经有一个_list_或者_tuple_，要调用一个可变参数

  ```
   >>>nums = [1,2,3]
   >>>calc(*nums)    #可以把list或tuple的元素变成可变参数

  ```

* 4.关键字参数 可变参数允许你传入_0_个或任意个参数，这些可变参数在函数调用时自动组装成一个_tuple_。而关键字参数允许你传入_0_个或任意个`含参数名的参数`,这些关键字参数在函数内部自动组装成一个`dict`。

  ```
   def person(name,age,**kw):
       print('name:',name,'age':age,'other:',kw)

  ```

  函数`person`除了必选参数`name`和`age`外，还接受关键字参数`kw`。在调用该函数时，可以只传入必选参数:

  ```
   >>>person('Michael',30)
   name:Michael,age:30,other:{}

  ```

  也可以传入任意个数的关键字参数:

  ```
   >>>person('Adam',40,gender='M',job='Engineer')
   name:Adam,age:40,other:{'gender':'M','job':'Engineer'}

  ```

  和可变参数一样，也可以先组装出一个_dict_,把这个_dict_转换成关键字参数穿进去：

  ```
   >>>extra = {'city':'Ningbo','job'='Engineer'}
   >>>person('Jack',24,**extra)

  ```

  `**extra`表示把`extra` 这个dict所有的key-value用关键字参数传入到函数的`**kw`参数，`kw`将获得一个_dict_，注意获得的只是一份拷贝，对`kw`的改动不会影响到函数外的`extra`。

* 5.命名关键字参数

  * 如果要限制关键字参数的名字，就可以用命名关键字参数，例如只接受`city`和`job`作为关键字。

    ```
    def person(name,age,*,city,job):
       print(name,age,city,job)

    ```

    和关键字参数`**kw`不同，命名关键字参数需要一个特殊分隔符`*`,`*`后面的参数被视为命名关键字参数。必须传入参数名,没有传入参数名掉用将会报错

    ```
    >>>person('Jack',24,city='Ningbo',job='Engineer')

    ```

  * 如果函数定义中已经有一个可变参数，后面的关键字参数就不需要特殊分隔符`*`:

    ```
    def person(name,age,*args,city,job)

    ```

  * 命名关键字参数可以有缺省值，简化调用:

    ```
    def person(name,age,*args,city='Ningbo',job)

    ```



* 6.参数组合

## **递归函数**

* 1.在函数内部，掉用自身本身，这个函数就是递归函数

  ```
   def fact(n):
       if n == 1:
           return 1
       return n*fact(n-1)

  ```


## **练习**

编写`move(n,a,b,c)`函数，表示接受参数`n`,_3_个柱子_A、B、C_中第_1_个柱子_A_的盘子数量，打印出把所有盘子从_A_借助_B_移动到_C_的方法

```
   def move(n,a,b,c):
       if n == 1:
           print('move',a,'-->',c)
       move(n-1,a,c,b)
       print('move',a,'-->',c)
       move(n-1,b,a,c)
```

