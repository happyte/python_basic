# **python第五天\(面向对象编程\)**

## **类和实例**

* 1.面向对象最重要的是类和实例,牢记类是抽象的模板，例如`Student`类，实例是根据类创建出来的一个个具体对象。
  ```
   class Student(object):
       pass

  ```

  `class`后紧跟类名，类名一般大写开头，紧接着是`object`,表示该类从哪个类继承下来。
* 2.定义好`Student`类，可以根据`Student`创建出`Student`的实例，创建实例是通过类名+\(\)实现。
  ```
   >>>zs = Student()
   >>>zs
   <_main_.Student object at 0x10a67a590>
   >>>Student
   <class '__main__.Student'>

  ```

  可以自由地给实例变量绑定属性，这是由于_python_的`鸭子特性`,后面会提起给实例绑定属性的几个特殊例子。
  ```
   >>>zs.name = 'zhangshu'

  ```

* 3.在创建实例的时候，把我们认为必须绑定的属性强制传进去，通过`__init___`方法
  ```
   class Student(object):
       def __init__(self,name,age):
           self.name = name
           self.age  = age

  ```

  `__init__`方法是创建实例自动会调用的，第一个参数永远是`self`,表示`创建实例本身`，因此在`__init__`方法内部可以把各种属性绑定到`self`上，因为`self`就是创建的实例本身。有了`__init__`方法，就必须传入与`__init__`相匹配的参数，但`self`不需要传，python解释器会把实例变量传进去。
  ```
   >>>zs = Student('zhangshu',23)
   >>>zs.name
   'zhangshu'
   >>>zs.age
   23

  ```

* 4.和普通函数相比，在类中定义的函数有一点不同，第一个参数永远是实例变量`self`，并且调用时不用传递该参数。

## **访问限制**

* 1.从前面Student类的定义来看，外部代码可以自由修改一个实例的`name`,`age`属性，如果要内部属性不被外部访问，在属性前加入`__`，实例的变量如果以`__`开头，就变成一个私有变量。
  ```
   class Student(object):
       def __init__(self,name,age):
           self.__name = name
           self.__age  = age
       def print_info(self):
           print('%s:%s' % (self.__name,self.__age))

  ```

  修改完成后，已经无法从外部访问`实例变量.__name`和`实例变量.__age`。
  ```
   >>>zs = Student('zhangshu',23)
   >>>zs.__name
   Traceback(most recent call last):
      File "<stdin>",line 1,in <module>
   AttributeError:'Student' object has no attribute '__name'

  ```

* 2.在_python_中，变量名类似`__xx__`的，是特殊变量，特殊变量是可以访问的，不是`private`变量，因此不能用`__name__`和`__age__`这样的变量名。
* 3.像实例变量名，以单下划线开头，例如`_name`，这样的实例变量外部也是可以访问的，但是按照约定俗称的规矩，这样的变量“我可以被访问，但请把我视为私有变量，不要随意访问”。
* 4.不能直接访问`__name`是因为python解释器把`__name`改成`_Student__name`,因此仍然可以通过`_Student__name`访问`__name`
  ```
   >>>zs._Student__name
   'zhangshu'

  ```

  `但强烈不建议这么使用!!!!!!`
* 5.下面看一种错误写法
  ```
   >>>zs = Student('zhangshu',23)
   >>>zs.get_name()
   'zhangshu'
   >>>zs.__name = 'New'
   >>>zs.__name
   'New'

  ```

  从表面看外部代码似乎成功设置了`__name`变量，但是`__name`变量和class内部的`__name`变量`不是`一个变量!!!内部的`__name`已经被解释器改成了`_Student__name`，而外部代码只是给`zs`新增了一个`__name`变量。
  ```
   >>>zs.get_name()
   'zhangshu'

  ```


## **继承和多态**

* 1.例如我们已经有一个名为`Animal`的类，有一个`run`方法可以直接打印:

  ```
   class Animal(object):
       def run(self):
           print('Animal is running')

  ```

  当我们需要编写`Dog`和`Cat`类，直接从`Animal`继承

  ```
   class Dog(Animal):
      def run(self):
          print('Dog is running')

   class Cat(Animal):
       def run(self):
           print('Cat'is running)

  ```

  继承的好处就是子类获得了父类的全部功能，当父类和子类存在相同的`run()`方法时，子类的`run()`方法会覆盖父类的`run()`方法，在代码运行时，总会调用子类的`run()`。这样，我们获得继承的另一个好处:`多态`。

* 2.
  ```
   >>>a = Animal()
   >>>b = Dog()
   >>>c = Cat()
   >>>isinstance(a,Animal)
   Ture
   >>>isinstance(b,Dog)
   Ture
   >>>isinstance(c,Cat)
   Ture

  ```

  判断变量是否是某个类型可以用`isinstance`判断
  ```
   >>>isinstance(c,Animal)
   True

  ```

  `c`不仅是`Cat`,`c`还是`Animal`，在继承关系中一个实例的数据类型是某个子类，那么它的数据类型也可以被看成父类，但是反过来不行。
  ```
   >>>isinstance(a,Cat)
   False

  ```

* 3.多态的好处，再编写一个函数，接受一个`Animal`类型的变量:
  ```
   def run_twice(animal):
       animal.run()
       animal.run()

  ```

  当我们传入`Animal`实例，`run_twice`打印出:
  ```
   >>>run_twice(Animal())
   Animal is running
   Animal is running

  ```

  当我们传入`Dog`实例，`run_twice`打印出:
  ```
   >>>run_twice(Dog())
   Dog is running
   Dog is running

  ```

  当我们传入`Cat`实例，`run_twice`打印出:
  ```
   >>>run_twice(Cat())
   Cat is running
   Cat is running

  ```


新添一个`Animal`类，不必对`run_twice`做任何修改，任何依赖`Animal`作为参数的函数都可以不加修改地正常运行，原因就在于多态。

多态的好处在于我们传入`Dog`,`Cat`时，我们只需要接收`Animal`类型就可以，因为`Dog`,`Cat`都是`Animal`类型，然后按照`Animal`类型操作即可。由于`Animal`类型有`run()`方法，因此传入的任意类型只要是`Animal`类或者子类，就会自动调用实际类型的`run()`方法，这就是多态。

对于一个变量，我们只需要知道是`Animal`类型，无需知道它的确切类型，具体调用的`run()`方法是作用在`Dog`,`Cat`对象上，由运行时该对象的确切类型决定，满足“开闭原则”:

* 对扩展开放:允许新增`Animal`子类
* 对修改封闭:不需要依赖`Animal`类型的`run_twice` 函数

## **获取对象信息**

* 1.基本数据类型都可以用`type`判断:
  ```
   >>>type(123)
   <class 'int'>
   >>>type('str')
   <class 'str'>

  ```

* 2.判断基本数据类型可以直接写`int`,`str`,判断一个对象是否是函数可以用`types`模块中定义的常量:
  ```
   >>>import types
   >>>def fn():
          pass
   >>>type(fn)==types.FunctionTye
   True
   >>>type(lambda x:x)==types.LambdaType
   True
   >>>type((x for x in range(10)))==types.LambdaType
   True

  ```

* 3.使用`isinstance`，能用`type`判断的也能用`isinstance`判断
  ```
   >>>isinstance('a',str)
   True
   >>>isinstance(b'a',bytes)
   True

  ```

  还可以判断一个变量是否为某些类型中的一个
  ```
   >>>isinstance([1,2,3],(list,tuple))
   True

  ```

* 4.使用`dir()`获得一个对象的所有属性和方法，例如获得一个str对象的所有属性和方法,类似`__xx__`的属性和方法在python中都是有特殊用处的，例如:
  ```
   >>>len('ABC')
   3
   >>>'ABC'.__len__()
   3

  ```


## **实例属性和类属性**

* 1.python语言的动态性，根据创建的实例可以绑定任意属性，给实例绑定属性的方法是通过实例变量，或者通过`self`

  ```
   class Student(object):
       def __init__(self,name):
           self.name = name

   s = Student('zhangshu')
   s.age = 23

  ```

* 2.如果`Student`这个类需要绑定一个属性呢？可以在class类中定义属性，这种属性就是类属性，但所有实例都可以访问到
  ```
   >>>class Student(object):
          name = 'zhangshu'
   >>>s = Student()  #创建实例s
   >>>s.name    #因为实例没有name属性，继续查找class的name属性
   'zhangshu'
   >>>s.name = 'Michale' #给实例绑定name属性
   >>>s.name  #实例的属性优先级比类的属性要高，屏蔽掉了类的name的属性
   'Michale'
   >>>Student.name
   'zhangshu'  #类属性并未消失，用Student.name仍然可以访问
   >>>del s.name  #删除实例的name属性
   >>>s.name
   'zhangshu'  #由于实例的name属性没有找到，类的name属性就显示出来了

  ```

  注意一点，`实例的属性名不要和类的属性名使用相同的名字!!!`

