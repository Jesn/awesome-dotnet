## virtual和abstract都是用来修饰父类的，通过覆盖父类的定义，让子类重新定义。

```
 1.virtual修饰的方法必须有实现（哪怕是仅仅添加一对大括号),
 而abstract修饰的方法一定不能实现。
 2.virtual可以被子类重写，而abstract必须被子类重写。
 3.如果类成员被abstract修饰，则该类前必须添加abstract，
 因为只有抽象类才可以有抽象方法。
 4.无法创建abstract类的实例，只能被继承无法实例化。

```
## 重载和重写

重载(overload)是提供了一种机制, 相同函数名通过不同的返回值类型以及参数来表来区分的机制。

重写(override)是用于重写基类的虚方法,这样在派生类中提供一个新的方法。



简介：

c#中Abstract和Virtual比较容易混淆，都与继承有关，并且涉及override的使用。virtual可以被子类重写，而abstract必须被子类重写。virtual修饰的方法必须有实现（哪怕是仅仅添加一对大括号),而abstract修饰的方法一定不能实现。它们有一个共同点：如果用来修饰方法，前面必须添加public，要不然就会出现编译错误：虚拟方法或抽象方法是不能私有的。毕竟加上virtual或abstract就是让子类重新定义的，而private成员是不能被子类访问的。下面讨论一下二者的区别：

 

一、Virtual方法（虚方法）

 

virtual 关键字用于在基类中修饰方法。virtual的使用会有两种情况：

情况1：在基类中定义了virtual方法，但在派生类中没有重写该虚方法。那么在对派生类实例的调用中，该虚方法使用的是基类定义的方法。

情况2：在基类中定义了virtual方法，然后在派生类中使用override重写该方法。那么在对派生类实例的调用中，该虚方法使用的是派生重写的方法。

virtual可以被子类重写：

复制代码
class BaseTest1
    {
       public virtual void fun() { }//必须有实现
    }
    class DeriveTest1:BaseTest1
    {
        //public override void fun() { }
    }
复制代码
virtual修饰的方法必须有实现（哪怕是仅仅添加一对大括号)：

 public class Test1
        {
            public virtual void fun1(){}
        }
 

二、Abstract方法（抽象方法）

 

abstract关键字只能用在抽象类中修饰方法，并且没有具体的实现。抽象方法的实现必须在派生类中使用override关键字来实现。

接口和抽象类最本质的区别：抽象类是一个不完全的类，是对对象的抽象，而接口是一种行为规范。

abstract必须被子类重写：

复制代码
abstract class BaseTest2
    {
        public abstract void fun();
    }
    class DeriveTest2 : BaseTest2
    {
        //public override void fun();错误1:没有实现
        //public  void fun() { }  错误2：重写时没有添加override
        //override void fun() { }错误3：虚拟成员或者抽象成员不能是私有的（只要在父类中声明了虚拟成员或抽象成员，即便是继承的也要加上这个限制）
        public override void fun() { }//如果重写方法； 错误：“A.DeriveTest2”不实现继承的抽象成员“A.BaseTest2.fun()”    

    }
复制代码
 

abstract修饰的方法一定不能实现：

public abstract class Test2
        {
            public abstract void fun2() ;
        }

三、关键字

 

Static：当一个方法被声明为Static时，这个方法是一个静态方法，编译器会在编译时保留这个方法的实现。也就是说，这个方法属于类，但是不属于任何成员，不管这个类的实例是否存在，它们都会存在。就像入口函数Static void Main，因为它是静态函数，所以可以直接被调用。

 

Virtua：当一个方法被声明为Virtual时，它是一个虚拟方法，直到你使用ClassName variable = new ClassName();声明一个类的实例之前，它都不存在于真实的内存空间中。这个关键字在类的继承中非常常用，用来提供类方法的多态性支持。

 

overrride：表示重写 这个类是继承于Shape类
virtual，abstract是告诉其它想继承于他的类 你可以重写我的这个方法或属性，否则不允许。


abstract：抽象方法声明使用，是必须被派生类覆写的方法，抽象类就是用来被继承的；可以看成是没有实现体的虚方法；如果类中包含抽象方法，那么类就必须定义为抽象类，不论是否还包含其他一般方法；抽象类不能有实体的。


转自: https://www.cnblogs.com/wml-it/p/14775776.html