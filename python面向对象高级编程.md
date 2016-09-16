# _**python**_**第六天\(面向对象高级编程\)**

## **使用**_**slots**_

* 1.创建完一个_class_实例后，我们可以给该实例绑定任何属性和方法

  ```
   class Student(object):
       pass

  ```

  然后给实例绑定一个属性

  ```
   >>>s = Student()
   >>>s.name = 'zhangshu'

  ```

  尝试给实例绑定一个方法:

  ```
   >>>def set_age(self,age):
          self.age = age
   >>>from types import MethodType
   >>>s.set_age = MethodType(set_age,s) #给实例绑定一个方法
   >>>s.set_age(23)
   >>>s.age
   23

  ```

  给实例_s_绑定的方法对另外一个实例不起作用

  ```
   >>>s2 = Student()
   >>>s2.set_age(20)
   Traceback (most recent call last):
      File "<stdin>",line 1,in <module>
   AttributeError: 'Student' object has no attribute 'set_age'

  ```

  为了所有实例都能绑定该方法，可以给_class_绑定方法:

  ```
   >>>Student.set_age = set_age

  ```

  通常情况下,`set_age`方法可以直接定义在class中，动态语言允许我们在代码运行过程中给class添加功能。

* 2.由于_python_语言的动态性，可以给实例绑定任何属性，如果我们想要限制实例的属性，例如只允许_Student_实例绑定`name`和`age`属性。

  ```
   class Student(object):
       __slots__ = ('name','age') #用tuple定义允许绑定的属性名称

  ```

  然后尝试:

  ```
   >>>s = Student()
   >>>s.name = 'zhangshu'
   >>>s.age = 23
   >>>s.score = 90
   Traceback (most recent call last):
       File "<stdin>",line 1,in <module>
   AttributeError: 'Student' object has no attribute 'score'

  ```

  `score`不允许绑定，绑定`score`发出`AttributeError`。`__slots__`仅对当前类的实例起作用，对继承的子类不起作用

  ## **使用@**_**property**_

* 1._python_内置的`@property`装饰器把一个方法变成属性调用

  ```
   class Student(object):

       @property
       def score(self):
           return self._score

       @score.setter
       def score(self,value):
           if not isinstance(value,int):
               raise ValueError('score must be an integer')
           if value < 0 or value > 100:
               raise ValueError('score must between 0~100')
           self._score = value

  ```

  把一个_getter_方法变成属性，只需要加上`property`就可以，`property`又创建了另一个装饰器`score.setter`，把一个setter方法变成属性赋值

  ```
   >>> s= Student()
   >>> s.score = 90
   >>>s.score
   90               #跟oc的property用法类似

  ```

* 2.可以只定义只读属性，只定义_getter_方法，不定义_setter_方法


## **多重继承**

* 1.像_java,c++_这类语言，只允许单一继承。而_python_允许多重继承,一个子类可以同时获得多个父类的功能。

  ```
   #大类
   class Mammal(Animal):
       pass
   class Bird(Animal):
       pass
   #功能
   class Runnable(object):
       def run(self):
           print('Running')
   class Flyable(object):
       def fly(self):
           print('Flying')
   #多重继承
   class Dog(Mammal,Runnable):
       pass
   class Bat(Mammal,Flyable):
       pass

  ```


## **枚举类**

* 1._python_提供了`Enum`来实现枚举类

  ```
   from enum import Enum
   Month = Enum('Month',('Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'))

  ```

  我们获得了`Month`类型的枚举类，可以直接使用`Month.Jan`来引用一个常量，也可以枚举它的所有成员:

  ```
   for name,member in Month.__members__.items():
       print(name,'=>',member,',',member.value)

  ```

  输出的值为

  ```
   Jan => Month.Jan,1
   Feb => Month.Feb,2
   Mar => Month.Mar,3
   Apr => Month.Apr,4
   May => Month.May,5
   Jun => Month.Jun,6
   Jul => Month.Jul,7
   Aug => Month.Aug,8
   Sep => Month.Sep,9
   Oct => Month.Oct,10
   Nov => Month.Nov,11
   Dec => Month.Dec,12

  ```

  `value`属性自动赋给成员的`int`常量，默认从`1`开始

* 2.可以从`Enum`派生出自定义类:

  ```
   from enum impport Enum,unique
   @unique
   class Weekday(Enum):
       Sun = 0
       Mon = 1
       Tue = 2
       Wed = 3
       Thu = 4
       Fri = 5
       Sat = 6

  ```

  `@unique`装饰器用来检查是否有重复值,访问这些枚举变量

  ```
   >>>day1 = Weekday.Mon
   >>>day1
   Weekday.Mon
   >>>print(Weekday.Tue)
   Weekday.Tue
   >>>print(Weekday['Tue'])
   Weekday.Tue
   >>>print(Weekday.Tue.value)
   2
   >>>print(Weekday(1))
   Weekday.Mon

  ```

  `Enum`可以把一组常量定义在class中\(继承Enum\),且class不可变，而且成员可以直接比较。


## **静态方法与类方法**

* 1.类方法在函数声明前用`@classmethod`声明，与实例方法的一个参数为`self`不同的是，类方法第一个参数为`cls`,表示当前类

  ```
   class Test(object):
       num = 100

       def get_num(self):
           return num

       def set_num(self,num):
           self.num = num

  ```

  这个类属性_num_无论通过类还是实例都可以拿到

  ```
   >>>test = Test()
   >>>test.num
   100
   >>>Test.num
   100
   >>>test.set_num(200)
   >>>Test.num
   100

  ```

  通过实例方法只能改变实例属性，无法改变类属性，因此引入类方法来修改类属性

  ```
   class Test(object):
       num = 100

       def get_num(self):
           return num

       @classmethod
       def set_num(cls,num):
           cls.num = num

  ```

  下面我们试着修改下类属性

  ```
   >>>test = Test()
   >>>test.num
   100
   >>>Test.num
   100
   >>>Test.set_num(200)   #第一个参数cls是隐形参数不用传
   >>>Test.num
   200
   >>>test.num
   200

  ```

  修改类属性用类方法，`有一点需要注意的是`，类不允许调用实例方法\(我试了下，其实类调用实例方法传入对象是可以掉用的\)

* 2.静态方法用`@staticmethod` 方法修饰，静态方法可以没有参数，而且类和实例都可以调用静态方法


