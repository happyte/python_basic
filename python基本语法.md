# **_python_**第一天

## **基本语法**

* 1.在_Pyhton_中，通常用全部大写的`变量名`表示常量：_PI = 3.1415926_。
* 2.在_Python_中，有两种除法，一种是／，例如：_10\/3=3.333333333_.另一种为\/\/称为地板除，两个整数的除法仍然是整数，例如_10\/\/3=3_。
* 3.在_Python_中内置的一种数据类型是列表：_list_。_list_是一种有序的集合，用\[\]表示，list可以随时添加和删除其中的元素。如果要获取最后一个元素，除了计算索引位置，还可以用_－1_做索引 。

  ```
  classmates[-1]

  ```

* 4.

  * list是可变的有序表，往_list_中添加元素到末尾

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

* 5._Python_中另一种有序表叫_tuple_。但tuple一旦初始化就不能修改，比如同样是列出同学的名字

  ```
  classmates = ('Michael','Bob','Tracy')

  ```

  _tuple_陷阱，在定义_tuple_时所有的元素必须被确定下来，比如_t=\(1,2\)_,如果要定义一个空_tuple,t=\( \)_。但是要定义一个只有1个元素的_tuple_，如果定义为_t=\(1\)_,其实定义的不是_tuple_,而是_1_这个数，应该定义为_t=\(1,\)_。

* 6._elif_是_else if_的缩写，完整的条件判断语句

  ```
  if<条件判断1>:
  <执行1>
  elif<条件判断2>:
  <执行2>
  elif<条件判断3>:
  <执行3>
  else:

  ```

* 7._Python_中，_input\(\)_返回的数据类型是_str,_由于_str_不能直接与整数比较，必须把_str_先转换成整数。

  ```
  s = input('birth:')
  birth = int(s)

  ```

* 8._Python_中有两种循环，一种为_for...in_循环，依次把_list_或_tuple_中的每个元素迭代出来

  ```
  names = ['Michael','Bob','Tracy']
  for name in names:
      print(name)
  ```


