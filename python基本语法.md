# **python第一天**

## **基本语法**

* 1.在_Pyhton_中，通常用全部大写的`变量名`表示常量：PI = 3.1415926。
* 2.在Python中，有两种除法，一种是／，例如：10／3=3.333333333.另一种为／／称为地板除，两个整数的除法仍然是整数，例如10／／3=3。
* 3.在Python中内置的一种数据类型是列表：list。list是一种有序的集合，用\[\]表示，list可以随时添加和删除其中的元素。如果要获取最后一个元素，除了计算索引位置，还可以用－1做索引 。

  ```
  classmates[-1]

  ```

* 4.

  * list是可变的有序表，往list中添加元素到末尾

    ```
    classmates.append('Adam')

    ```

  * 插入到指定位置

    ```
    classmates.insert(1,'Jack')

    ```

  * 删除指定位置的元素

    ```
    classmates.pop(1)

    ```



* 要把某个元素替换成别的元素，直接赋值给对应的索引位置

  ```
  classmates[1] = 'Sarah'

  ```

* 5.Python中另一种有序表叫tuple。但tuple一旦初始化就不能修改，比如同样是列出同学的名字

  ```
  classmates = ('Michael','Bob','Tracy')

  ```

  tuple陷阱，在定义tuple时所有的元素必须被确定下来，比如t=\(1,2\),如果要定义一个空tuple,t=\(\)。但是要定义一个只有1个元素的tuple，如果定义为t=\(1\),其实定义的不是tuple,而是1这个数，应该定义为t=\(1,\)。

* 6.elif是else if的缩写，完整的条件判断语句

  ```
  if<条件判断1>:
  <执行1>
  elif<条件判断2>:
  <执行2>
  elif<条件判断3>:
  <执行3>
  else:

  ```

* 7.Python中，input\(\)返回的数据类型是str,由于str不能直接与整数比较，必须把str先转换成整数。

  ```
  s = input('birth:')
  birth = int(s)

  ```

* 8.Python中有两种循环，一种为for...in循环，依次把list或tuple中的每个元素迭代出来

  ```
  names = ['Michael','Bob','Tracy']
  for name in names:
      print(name)
  ```


