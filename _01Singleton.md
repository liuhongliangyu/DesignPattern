# Singleton 单件模式（创建型模式）

## 模式分类

*	从目的来看：

1. 创建型（Creational）模式：负责对象创建

2. 结构性（Structural）模式：处理类与对象间的组合

3. 行为型（Behavioral）模式：类与对象交互中的职责分配。

* 从范围来看：

1. 类模式处理类与子类的静态关系。

2. 对象模式处理对象间的动态关系。

## 动机（Motivation）

* 在软件系统中，经常有这样一些特殊的类，必须保证它们在系统中只存在一个实例，才能确保它们的逻辑正确性，以及良好的效率。

* 如何绕过常规的构造器，提供一种机制来保证一个类只有一个实例？

* 这应该是类设计者的责任，而不是使用者的责任

## 意图

保证一个类仅有一个实例，并提供一个该实例的安全访问点。

### 单线程实例

* 饱汉式单例模式

```C#
public class Singleton
{
	private static Singleton instance;    　　//定义一个私有的静态全局变量来保存该类的唯一实例
  private Singleton() {}      //提供一个私有的构造函数，避免C#自动生成共有的构造函数（如果程序员不写构造函数，C#会自动生成一个共有的构造函数,如果是共有的，那么其他类就可以随便创建该类对象了)，不让外界创建此类对象
  public static Singleton Instance  {
    get{ //提供一个共有的静态的属性，让外界可以访问，在属性中，提供一个实例，仅此一个实例，那么外界访问时就会只有这一个实例，是唯一的。
      if(instance == null)
      {
            instance = new Singleton;
      }
      return instance;
     }
  }
}
```

* 饿汉式单例模式

```C#
public class Singleton
{
	private static Singleton instance = new Ingleton();    　　//定义一个私有的静态全局变量来保存该类的唯一实例
  private Singleton() {}      //提供一个私有的构造函数，避免C#自动生成共有的构造函数（如果程序员不写构造函数，C#会自动生成一个共有的构造函数,如果是共有的，那么其他类就可以随便创建该类对象了)，不让外界创建此类对象
  public static Singleton Instance  {
	  get{ //提供一个共有的静态的属性，让外界可以访问，在属性中，提供一个实例，仅此一个实例，那么外界访问时就会只有这一个实例，是唯一的。
      return instance;
    }
  }
}
```

#### 单线程Sigleton模式的几个要点

* Singleton模式中的实例构造器可以设置为protected以允许子类派生。

* Singleton模式一般不要支持ICloneable接口，因为这可能会导致多个对象实例与Singleton模式的初衷违背

*Singleton模式一般不要支持序列化，因为这也有可能导致多个对象实例，同样与Singleton模式的初衷违背

* Singleton模式只考虑到了对象创建的管理，没有考虑对象销毁的管理，就只是垃圾回收的平台和对象地开销来讲，我们一般没有必要对其销毁进行特殊的管理。

* 不适用应多线程环境：在多线程环境下，使用Singleton模式仍然有可能得到Singleton类的多个实例对象。

### 多线程下的单例

* 第一种

```C#
public class Singleton
{
  private static volatile Singleton instance;    //提供一个私有的静态的类类型变量，volatile是被设计用来修饰被不同线程访问和修改的变量。volatile的作用是作为指令关键字，确保本条指令不会因编译器的优化而省略，且要求每次直接读值。
	private static object lockHelper = new object();   //内部提供线程锁用，这个对象是在程序运行时创建的
  private Singleton() {}      //提供一个私有的构造函数，避免C#自动生成共有的构造函数（如果程序员不写构造函数，C#会自动生成一个共有的构造函数）   ，不让外界创建此类对象
  public static Singleton Instance  {   //定义一个全局访问点,可以是静态方法，也可以是静态属性
    get{
      if(instance == null)
      {
        lock(lockKelper)    //提供一个线程锁
        {
          if(instance == null)
            instance = new Singleton;
        }
      }
      return instance;
    }
  }
}
```

* 第二种

```C#
class Singleton
{
  private static readonly Singleton Instance = new Singleton();  //只读,不能作传参的构造实例，只能使用无参的
  private Singleton() {}
}
```

* 等价于

```C#
class Singleton
{
   public static readonly Signleton Instance;
   static Singleton()
   {
      instance = new Singleiton();
   }
   private Singleton() {}
}
```

* 注：还有需要传参的的构造函数

## Singleton模式扩展

* 将一个实例扩展到n个实例，例如对象池的实现。

* 将new构造器的调用转移到其他类中，例如多个类协同工作环境中，某个局部环境只需要拥有某个类的一个实例

* 理解和扩展Singleton模式的核心是“如何控制用户使用new对一个类的实例构造器的任意调用”。




















