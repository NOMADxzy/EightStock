## JAVA

### 一、基础

- 面向过程：就是具体化的一步一步的去实现，优点 就是性能比较快因为不需要进行实例化。 缺点就是不容易进行维护，扩展和复用
- 面向对象：因为面向对象的三大特点就是 封装、继承和多态

| 类型     | 特点                       | 关注重点                   | 最小程序单元 |
| -------- | -------------------------- | -------------------------- | ------------ |
| 面向过程 | **采用自顶而下的设计模式** | 强调的是功能行为           | 函数         |
| 面向对象 | 多个功能合理放到不同对象里 | 强调的是具备某些功能的对象 | **类**       |

#### 面向对象（OOP）

##### 特点

```
1. 单一职责原则
单一职责原则指出，一个类应该只有一个引起变化的原因，即一个类只负责一个功能领域中的相应职责。这意味着每个类都应该专注于执行一个特定的任务，这样如果未来需要修改某个功能，则只需要修改与该功能直接相关的类。

2. 开放封闭原则
软件实体（如类、模块、函数等）应该对扩展开放，而对修改封闭。这意味着已经设计好的类应当在不修改其源代码的情况下可以被扩展，例如通过继承和实现接口来增强或改变其行为。

3. 里氏替换原则
里氏替换原则是关于子类型继承的原则，子类对象应该能够替换掉它们的父类对象，而不影响程序的正确性。

4. 接口隔离原则 
接口隔离原则建议不要使用大而全的接口，而是应该将接口拆分成更小且更具体的接口，让实现类只需要关心他们需要的接口。

5. 依赖倒置原则
依赖倒置原则主张高层模块不应该依赖低层模块，二者都应该依赖于抽象。抽象不应该依赖于细节；细节应该依赖于抽象。换言之，类之间的依赖关系应当建立在抽象（接口或抽象类）上，而不是具体的类上。

6. 合成复用原则
合成复用原则鼓励使用对象的组合/聚合来重用代码，而不是通过继承来达到重用。这是因为继承使得类与类之间的关系密切连接，牵一发而动全身，从而可能导致脆弱的设计。通过使用合成，类之间的关系会更加灵活。

迪米尼特原则：最小知识原则，一个对象应该对其他对象有尽可能少的了解，每个单元应该只了解与它直接相关的单元，并且它只应该与它的直接朋友通信。
```

###### 3.1、封装

定义：使用对象封装一些变量和函数

作用：复用和信息隐藏，封装，也就是把客观事物封装成抽象的类，并且类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏。

###### 3.2、抽象

抽象就是找出一些事物的相似和共性之处，然后将这些事物归为一个类，这个类只考虑这些事物的相似和共性之处，并且会忽略与当前主题和目标无关的那些方面，将注意力集中在与当前目标有关的方面。例如，看到一只蚂蚁和大象，你能够想象出它们的相同之处，那就是抽象。抽象包括行为抽象和状态抽象两个方面。

###### 3.3、继承

定义：一个类获取另外一个类属性和方法的一种方式

作用：代码复用，继承，就是可以使用已创建好的类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。

###### 3.4、多态

定义：同一个操作，作用于不用的对象，会有不同的行为

作用：具有可拓展性详解：多态首先是建立在继承的基础上的，先有继承才能有多态。多态是指不同的子类在继承父类后分别都重写覆盖了父类的方法，即父类同一个方法，在继承的子类中表现出不同的形式。js天生就具备多态的特性(弱类型语言)

原理：靠的是父类或接口定义的引用变量可以指向子类或具体实现类的实例对象，而程序调用的方法在运行期才动态绑定

```markdown
## AOP和OOP
OOP主要功能封装、继承、多态，AOP它是将系统分解为不同的关注点(不同的切面)。

OOP，我们会根据业务将应用划分为多个不同的业务模块，每个模块的核心功能只为某个核心业务提供服务，如学生宿舍管理系统，学生管理、班级管理、房间管理，分别学生、班级、房间服务的。
除此之外，还会有一些非业务的通用功能，如日志、权限、事务管理等。它们和业务无关，但是几乎所有的模块都会用到它。因此这些通用功能会散布在不同的业务模块中。此时会有很多重复性的代码，不理模块的复用。
为了解决这个问题，AOP(面向切面编程)就出来了，它是把非业务的通用功能（横切关注点，指的是那些与业务逻辑分离的、但又贯穿于多个模块或功能点的代码部分，例如日志记录、事务管理、安全检查、缓存等）抽取出来单独维护，并通过声明的方式(定义切入点)去指定这些功能以何种方式(通知类型)作用在哪里(方法连接点--目标方法)，而不是直接在模块的代码中去直接添加。

切面 (Aspect)：一个关注点的模块化，这个关注点可能横切多个对象。比如日志系统可以看作是一个切面，因为它影响了整个应用的很多部分。
连接点 (Join Point)：在程序执行过程中某个特定的点，例如方法的调用或者异常的抛出。在 Spring AOP 中，一个连接点总是代表一个方法的执行。
通知 (Advice)：切面在特定的连接点采取的动作。通知类型包括“前置通知”、“后置通知”、“环绕通知”、“异常通知”和“最终通知”。
切入点 (Pointcut)：匹配连接点的断言。切入点帮助指定我们想要在哪些连接点执行切面的通知。

## 例子：
以日志记录为例，如果没有 AOP，你可能需要在每个方法开始和结束的时候手动插入日志记录代码。而使用 AOP，你可以单独创建一个日志记录的切面，然后选择你要记录日志的切入点（比如所有服务层的方法），之后框架会在运行时自动在这些切入点处应用日志记录逻辑，无需修改实际业务方法的代码。
```

##### 对象底层原理

- 堆（Heap）：此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。这一点在Java虚拟机规范中的描述是：所有的对象实例以及数组都要在堆上分配。
- 栈（Stack）：是指虚拟机栈。虚拟机栈用于存储局部变量等。局部变量表存放了编译期可知长度的各种基本数据类型（boolean、byte、char、short、int、float、long、double）、对象引用（reference类型，它不等同于对象本身，是对象在堆内存的首地址）。方法执行完，自动释放。
- 方法区（MethodArea）：用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwYWRlX0t3bw==,size_16,color_FFFFFF,t_70-20240120225240754.png)

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwYWRlX0t3bw==,size_16,color_FFFFFF,t_70-20240120225341322.png)

#### 数据类型

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/fc1f4134970a304e051fda77fdc5bc8cc9175c10.png)

##### 基本数据类型

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/09aefb43c53846f39034e786af458f25.png)

###### 为什么float a = 3.4报错

3.4 默认是浮点double类型的，如果赋值给float是向下转型，会出现精度缺失，，需要强制转换

###### Switch支持的6种数据类型？

byte、char 、short、int、 enum 、 String

###### 基本类型与包装类的区别

```markdown
## 存储位置不同：
基本数据类型直接将值放在栈中；（非static的成员变量存在堆中）
包装类型是把对象放在堆中，然后通过对象的引用来调用他们 ；
## 初始值不同：
int的初始值为 0 、 boolean的初始值为false ；
包装类型的初始值为null ；
## 使用方式不同：
基本数据类型直接赋值使用就好；
在集合如 coolectionMap 中只能使用包装类型；包装类型可以使用在泛型中
```
###### 基本类型与包装类的相互转换

```markdown
## 手动转换
基本数据类型 → 包装类：
    通过对应包装类的构造方法实现（Integer.valueOf(10)），除了Character外，其他包装类都可以传入一个字符串参数构建包装类对象。
包装类 → 基本数据类型
    通过包装类的实例方法 xxxValue() 实现; // xxx表示包装类对应的基本数据类型
## 自动装箱&自动拆箱（jdk1.5以后）
基本类型添加到集合中时，进行自动装箱
包装类型在涉及到运算的时候，“加，减，乘， 除” 以及 “比较 equals,compareTo”，进行自动拆箱
```
###### 包装类型的缓存机制

`Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的缓存数据，`Character` 创建了数值在 **[0,127]** 范围的缓存数据，(可以通过`Djava.lang.Integer.IntegerCache.high=范围`来设置`Integer`缓存的最大值)

`Boolean`类只有两个预定义的值：`Boolean.TRUE`和`Boolean.FALSE`。这意味着不管你何时需要一个`true`或者`false`的包装对象，总是得到同一个预定义的实例。

浮点类型的包装类`Float`和`Double`并没有实现缓存机制，因为它们代表的数值范围非常广泛，且通常应用场景下对浮点数的精确值要求更高。每次创建这些类型的包装对象都会生成新的实例。

这些包装类的缓存是存放在jvm的堆内存区域的，因为他们是普通的Java对象。

```java
Integer i1 = 33;
Integer i2 = 33;
System.out.println(i1 == i2);// 输出 true

Integer i1 = 40;
Integer i2 = new Integer(40);
System.out.println(i1==i2); // 输出 False
```

引用数据类型 和 数组

```markdown
## 引用数据类型的变量定义及赋值格式：
数据类型 变量名 = new 数据类型();
调用该类型实例的功能：变量名.方法名();
## 数组
一组数据的集合，数组中的每个数据被称作'元素'
在数组中可以存放'任意类型'的元素但'同一个数组'里存放的元素类型必须一致。
格式: int[] x = new int[100];
使用属性：数组名.length
```

##### 数组

初始化

```java
动态初始化：数组声明且为数组元素分配空间与赋值的操作分开进行
int[] arr= new int[3];
arr[0] = 3;
arr[1] = 9;
arr[2] = 8;
静态初始化：在定义数组的同时就为数组元素分配空间并赋值
int arr[] = new int[]{ 3, 9, 8};
int[] arr = {3,9,8};
```

Arrays工具类

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwYWRlX0t3bw==,size_16,color_FFFFFF,t_70-20240120224851943.png)

#### 方法

构成：

- 访问修饰符

  方法允许被访问的权限范围。( public，protected，无，private )

  其他修饰（abstract`, `static`, `final`, `synchronized`, `native，transient）

- 参数列表：可以省略。参数类型 + 参数名，...

- 返回类型：void、任何数据类型

注意：

- 方法参数是 基本类型 ，传递的是值。（包含String类型），形式参数的改变对实际参数不影响
- 方法参数是 引用类型，传递的是内存地址值。（String类型除外），形式参数的改变对实际参数有影响

##### 可变参数

```java
public void sum(int... n){}
```

- 可变参数一定在最后，可以将数组传递给可变参数列表
- 可变参数的实质就是数组：一个参数是可变参数，一个参数是一维数组，这两个方法不是重载，因为 一个可变参数等价于相应类型的一维数组
- 方法重载的时候，既可以定义有可变参数的同名方法，也可以定义有确定参数个数的方法，jvm调用方法时，会优先调用 有确定参数个数 的方法

##### 默认方法

背景：实现接口的类必须为接口中定义的每个方法提供一个实现，或者从父类中继承它的实现。但是，一旦类库的设计者需要更新接口，向其中加入新的方法，这种方式就会出现问题。现存的实体类往往不在接口设计者的控制范围之内，这些实体类为了适配新的接口约定也需要进行修改，为了让类库的设计者放心地改进应用程序接口，无需担忧对遗留代码的影响：

Java 8引入了一个新功能，叫默认方法，通过默认方法你可以指定接口方法的默认实现接口能提供方法的具体实现。实现接口的类如果不显式地提供该方法的具体实现，就会自动继承默认的实现。

```java
// List 接口中的 sort 方法是Java 8中全新的方法,新 default 修饰符
default void sort(Comparator<? super E> c){
  Collections.sort(this, c);
}
```

作用：采用默认方法之后，你可以为这种类型的方法提供一个默认的实现，这样实体类就无需在自己的实现中显式地提供一个空方法。通过这种方式，你可以减少无效的模板代码。实现 Iterator 接口的每一个类都不需要再声明一个空的 remove 方法了，因为它现在已经有一个默认的实现。

```java
// Iterator 接口就为 remove 方法提供了一个默认实现
interface Iterator<T> {
  boolean hasNext();
  T next();
  default void remove() {
    throw new UnsupportedOperationException();
  }
}
```

##### 抽象方法

- 抽象方法不可以与sychronized共存，因为synchronized方法的锁对象为this，而抽象方法无法确定this是什么，所以不能共存。
- 抽象方法不能和private、final、static共存，abstract修饰的类需要被子类继承，abstract修饰的方法需要子类重写，但是final修饰的类不能被继承，final修饰的方法也不能被子类重写，private修饰的方法对子类不可见。
- 抽象方法不能是静态的，因为抽象方法要被子类实现，而静态方法属于一个类，不能同时属于两个类。
- abstract与native不能共存，Native方法表示该方法要用另外一种依赖平台的语言实现，不存在着被子类实现的问题，所以不能是抽象的。

#### 类

##### 包

作用：

- **提供搜索和定位**类、接口、枚举和注释等
- **防止命名冲突**（包采用树形目录的存储方式，同包中类的名字不同，不同包中类的名字可以相同。）
- **访问控制**（拥有包访问权限的类才能访问某个包中的类）

java.lang：包含语言支持类（例如分类，用于定义基本数据类型，数学运算）。该软件包会自动导入。
java.io：包含分类以支持输入/输出操作。
java.util：包含实现像链接列表，字典和支持等数据结构的实用类; 用于日期/时间操作。
java.applet：包含用于创建Applets的类。
java.awt：包含用于实现图形用户界面组件的类（如按钮，菜单等）。
java.net：包含支持网络操作的类。

##### Object类

```markdown
## java源文件中只能有一个public类，包中public的类的类名必须和文件名相同

1. 简化查找：这项约束使得编译器在查找类定义时更高效。当其他代码引用一个公共类时，编译器可以直接定位到具有相应名称的文件，而无需查看所有源文件来寻找类定义。
2. 提高安全性：强制执行这种匹配减少了源代码中潜在的混乱和安全问题，因为它保证了源文件具有透明和可预测的行为。
```

**main**方法一个特殊的函数，作为**程序的入口，可被JVM调用**，（String[] args）：main函数的参数，类型是一个数组，该数组中的元素为**字符串数组**，每个类中有且仅有一个主函数

Java类都有一个共同的祖先类：Object类（实体类），Object类拥有的方法：hashcode()、equals()、toString()、getClass()、wait()、notify()、notifyAll()、finalize()、clone()

```markdown
## Class类和Object类的关系 
Object类和Class类没有直接的关系
Object类是一切java类的父类，对于普通的java类，即便不声明，也是默认继承了Object类
Class类是用于java反射机制的，一切java类，都有一个对应的Class对象，他是一个final类
Class 类的实例表示正在运行的 Java 应用程序中的类和接口

clone方法
    保护方法，实现对象的浅复制，只有实现了Cloneable接口才可以调用该方法，否则抛出CloneNotSupportedException异常
hashCode方法

    该方法用于哈希查找，重写了equals方法一般都要重写hashCode方法。obj1.equals(obj2)==true。可以推出obj1.hash- Code()==obj2.hashCode()，但是hashCode相等不一定就满足equals。不过为了提高效率，应该尽量使上面两个条件接近等价
```
##### 内部类

1. 成员内部类
   - 成员内部类作为外部类的成员，可以访问外部类的私有成员或属性
   - 当外部类变量与方法和内部类同名，内部类默认访问自己的成员变量或方法，引用外部变量使用时加类名.this：类名.this.变量名、类名.this.方法名（）
   - 外部类不能直接使用内部类的成员和方法,可先创建内部类的对象，然后通过内部类的对象来访问其成员变量和方法。

2. 静态内部类
   - 静态内部类不能直接访问外部类的非静态成员，但可以通过 new 外部类().成员 的方式访问
   - 外部类的静态成员与内部类的成员名称相同，可通过“类名.静态成员”访问外部类的静态成员
   - 创建静态内部类的对象时，不需要通过外部类的对象，可以直接创建 内部类 对象名= new 内部类();
3. 局部内部类（方法内、代码块内、构造器内）
   - 局部内部类中不可定义静态变量，可以访问外部类的局部变量(即方法内的变量)，但是变量必须是final的。
   - 不能在方法内部类中创建可变的局部变量。
   - 可以访问外围类的成员变量。如果是static方法，则只能访问static修饰的成员变量。

4. 匿名内部类

   ```java
   public class TestDemo {
      public static void main(String[] args) {
          Person per = new Person() {
              public void work() {// 匿名内部类自定义的方法work
                  System.out.println("work方法调用");
              }
              @Override
              public void eat() {// 实现接口的的方法eat
                  System.out.println("eat方法调用");
              }
          };
          per.eat();// 可调用
          per.work();// 出错，不能调用，因为 Person per = new Person()创建的是Person的对象，而非匿名内部类的对象。匿名内部类连名字都没有，无法实例化调用对象，但继承父类的方法和实现的方法是可以正常调用的
      }
   }
   interface Person {
      public void eat();
   }
   ```

注意：

- 要创建 Inner class 的实例，必须先创建一个包裹类的实例（创建静态内部类的对象时，不需要通过外部类），也正因如此，内部类不能定义静态成员（不然怎么访问呢？）
- 内部类的声明会覆盖掉包裹类的声明，解决办法：OuterClass.this
- 局部类只能访问 final or effectively final 的外部变量，当局部类只使用一次时，可以考虑使用匿名类。

##### 抽象类

- 使用abstract关键字修饰的类

- 有构造器，但不能实例化（可以在其子类被实例化时被调用，以便执行父类的初始化。）

- 若子类没有实现/覆盖父类所有的抽象方法，那么子类也得作为抽象类(**抽象派生类**)，需要使用abstract修饰

- abstract不能用来修饰private、static、final、属性、构造器

- 抽象类中可以没有抽象方法，为什么？

  防止直接实例化，提供了一些基础功能，但需要子类来完善或扩展某些行为

  预期在未来会添加抽象方法

  与接口相比，抽象类可以含有非静态或非final的字段，这样这些字段就可以在子类之间共享

##### Static

1. 修饰属性

   ```
   将类中的属性划分为 静态属性(静态变量) 和非静态属性(实例变量)
   静态变量随着类的加载而加载。可以通过  类.静态变量  的方式进行调用，早于对象的创建
   由于类只会加载一次，则静态变量在内存中也只会存在一份：存在方法区的静态域中
   静态属性举例：System.out; Math.PI;
   ```

2. 修饰方法

   ```
   ①随着类的加载而加载，可以通过 类.静态方法 的方式进行调用  
   ②静态方法中，只能调用静态的方法或属性
   ③静态方法中，不能使用this关键字、super关键字，因为没有对象
   ```

3. 修饰代码块

   ```markdown
   代码块的作用是：限制局部变量的作用范围，提前释放内存，一般结合if,while ,for等关键字使用
       3.每创建一个对象，就执行一次非静态代码块
       4.作用：创建对象时对对象的属性进行初始化
       5.非静态代码块内可以调用静态的属性、静态的方法，或非静态的属性、非静态的方法
   static代码块的作用：用于给类进行初始化，在加载的时候就执行，并且只执行一次。一般用于加载驱动
   ```

##### 构造方法

- 所有的class都必须有一个构造方法，如果没有在代码里声明，系统会自动生成一个公有无参的构造方法，**没有返回类型**，其默认的返回类型为对象类型本身
- 所有的子类构造器都要求在**第一行代码**中调用父类构造器，如果不写，系统默认去调用父类的无参构造器。
- 不能被 static、final、synchronized、abstract 和 native 修饰（原因：构造方法用于初始化一个新对象，所以用 static 修饰没有意义；构造方法不能被子类继承，所以用 final 和 abstract 修饰没有意义；多个线程不会同时创建内存地址相同的同一个对象，所以用 synchronized 修饰没有必要

```markdown
## 子类调用父类构造方法：
当子类构造方法调用父类无参构造方法，一般都是默认不写的，要写的话就是super（），且要放在构造方法的第一句
当子类构造方法要调用父类有参数的构造方法，必须要用super（参数）来调用父类构造方法，且要放在构造方法的第一句
当子类的构造方法是无参构造方法时，必须调用父类无参构造方法。
```

##### 代码块

局部代码块

- 位置：局部位置（方法内部）
- 作用：限定变量的生命周期，尽早释放，节约内存
- 调用：调用其所在的方法时执行

构造代码块

- 位置：类成员的位置，就是类中方法之外的位置
- 作用：把多个构造方法共同的部分提取出来，共用构造代码块
- 调用：每次调用构造方法时，都会优先于构造方法执行，也就是每次new一个对象时自动调用，对对象的初始化

静态代码块

- 位置：类成员的位置，用static修饰的代码块
- 作用：对类进行初始化，当new多个对象时，只能加载第一个new对象，执行一次
- 调用：new对象时自动调用，并只能调用一次

```java
//A类
public class A {
static {
    Log.i("HIDETAG", "A静态代码块");
}
private static C c = new C("A静态成员");
private  C c1 = new C("A成员");
{
    Log.i("HIDETAG", "A代码块");
}
static {
    Log.i("HIDETAG", "A静态代码块2");
}
public A() {
    Log.i("HIDETAG", "A构造方法");
}
}
//B类
public class B extends A {
private static C c1 = new C("B静态成员");
{
    Log.i("HIDETAG", "B代码块");
}
private C c = new C("B成员");
static {
    Log.i("HIDETAG", "B静态代码块2");
}
static {
    Log.i("HIDETAG", "B静态代码块");
}
public B() {
    Log.i("HIDETAG", "B构造方法");
} 
}
//C类
public class C {
public C(String str) {
    Log.i("HIDETAG", str + "构造方法");
}
}
// 主函数
public static void main(String[] args) {
		B a = new B();
}
//输出：
I/HIDETAG: A静态代码块
I/HIDETAG:A静态成员构造方法
I/HIDETAG:A静态代码块2
I/HIDETAG:B静态成员构造方法
I/HIDETAG:B静态代码块2
I/HIDETAG:B静态代码块
I/HIDETAG:A成员构造方法
I/HIDETAG:A代码块
I/HIDETAG:A构造方法
I/HIDETAG:B代码块
I/HIDETAG:B成员构造方法
I/HIDETAG:B构造方法
```

执行顺序：

父类的静态成员和代码块——>子类的静态成员和代码块——>父类成员初始化和代码块——>父类构造方法——>子类成员初始化和代码块——>子类构造方法

##### 继承

- Java抽象类可以继承抽象类
- 抽象类也可以继承具体类。但是，这有一个前提条件，即具体类必须具有明确的构造函数。如果具体类没有写构造函数，系统会自动生成默认的无参构造器，这意味着没有写构造函数的具体类也可以被抽象类继承。然而，一旦将具体类的无参构造器设置访问修饰符为 private，那么抽象类就不能再继承这个具体类了
- Java不支持多重继承呢，因为会导致菱形继承问题：一个类同时继承了两个具有共同父类的类时，如果这两个父类有相同的方法，子类将无法确定使用哪个父类的方法。这样就导致了二义性的产生

##### 上下转型

1. 向上转型

   向上转型就是子类的对象赋给父类的引用或（父类的引用指向子类的对象）

   ```java
   Person p1=new Man();
   Person p1=new Woman();
   ```

2. 向下转型

   向上转型无法调用子类特有的属性和方法，为了能够调用子类特有的属性和方法，就有了向下转型。向下转型是**子类对象被父类引用之后**，再把父类引用强转成子类，强转时叫做向下转型。

   使用强转时，可能出现ClassCastException的异常。会用到instanceof关键字

   ```java
   Person p1=new Man();//向上转型
   if(p1 instanceof Man){
       Man m2 = (Man)p1;
       m2.work();
       System.out.println("******Man******");
   }
   ```

##### this关键字

它在方法内部使用，即这个方法所属对象的引用；它在构造器内部使用，表示该构造器正在初始化的对象。

```java
// 调用方法
public void getInfo(){
		System.out.println("姓名：" + name) ;
		this.speak();
	}
// 调用属性
public boolean compare(Person p){
		return this.name==p.name;
	}
// 调用本类的构造器
public Person(String name){
		this(); // 调用本类中的无参构造器
    this(name) ; // 调用有一个参数的构造器
		this.name = name ;
	}
```

父类方法中的this也可能代表的是调用该父类方法的子类的实例对象，**abstract类中的非抽象方法中的this对象只能是代表子类自己的实例对象指针**

父类的final修饰方法，意味着子类无法去重写该方法，但不影响子类对象直接去调用父类的final方法，所以**final方法中的this对象既可以代表子类自己的实例对象指针，又可以代表父类本身的实例对象指针**

#### 集合类

![集合类-思维导图](https://gitee.com/xu_zuyun/picgo/raw/master/img/bb2a9bb3857146a0b6a2483c0f52e5ea.png)

##### 集合（Collection）

1、 List列表 ： 有序 可重复

```
1、ArrayList : 数组列表 ，内部是通过Array实现，对数据列表进行插入、删除操作时都需要对数组进行拷贝并重排序，因此在知道存储数据量时，尽量初始化初始容量，提升性能 。
2、LinkedList : 双向链表  每个元素都有指向前后元素的指针，顺序读取的效率较高，随机读取的效率较低 
3、Vector : 向量 ， 线程安全的列表，与ArrayList 一样也是通过数组实现的
4、Stack : 栈 ， 后进先出 LIFO ， 继承自Vector，也是用数组，线程安全的栈
```

| 类型       | 底层实现 | 线程安全 | 扩容方式                       | 特点           |
| ---------- | -------- | -------- | ------------------------------ | -------------- |
| ArrayList  | 数组     | 否       | 初始容量是 10，扩容因子是 0.5  | 查询快，增删慢 |
| LinkedList | 链表     | 否       | 没有扩容的机制                 | 查询慢，增删快 |
| Vector     | 数组     | 是       | 默认初始容量为10，扩容因子是 1 | 查询快，增删慢 |

2、Queue队列：有序 可重复

```
1、ArrayDeque :  数组实现的的双端队列， 可以在队列两端插入和删除元素 
2、LinkedList : 也属于双端队列 
3、PriorityQueue : 优先队列  ，数组实现的二叉树， 完全二叉树实现的小顶堆(可改变比较方法)
```

3、Set集合： 无序 不重复

```
1、HashSet 基于哈希实现的集合， 链表形式
2、LinkedHashSet 
3、TreeSet 红黑树结构 
```

| 类型          | 底层实现          | 线程安全 | 扩容方式                                        | 特点 |
| ------------- | ----------------- | -------- | ----------------------------------------------- | ---- |
| HashSet       | 基于HashMap实现   | 否       | 添加元素时，table数组扩容为16，加载因子为0.75   | 无序 |
| LinkedHashSet | 基于LinkedHashMap | 否       | 初始容量为16，临界值为12，以后再次扩容，扩容2倍 | 有序 |
| TreeSet       | 基于TreeMap实现   | 否       | 容量翻倍                                        | 有序 |

##### 集合中的设计模式

1. 适配器模式

   ```java
   // 数组到列表的适配器：
   String[] array = new String[]{"element1", "element2", "element3"};
   List<String> list = Arrays.asList(array);
   
   // 集合到线程安全集合的适配器：
   List<String> list = new ArrayList<>();
   List<String> syncList = Collections.synchronizedList(list);
   ```

2. 迭代器模式

   ```java
   List<String> names = new ArrayList<>();
   names.add("Alice");
   names.add("Bob");
   names.add("Charlie");
   
   // 获取迭代器
   Iterator<String> iterator = names.iterator();
   
   // 使用迭代器遍历集合元素
   while (iterator.hasNext()) {
       String name = iterator.next();
       System.out.println(name);
       // 示例中省略了 remove 的调用，但如果需要，可以这样移除元素：
       // iterator.remove();
   }
   ```

##### 集合（Collection）和数组的区别

- 2、数组的长度是固定的，集合长度是可以改变的，提供很多成员方法。
- 3、数组的存放的类型只能是一种（基本类型/引用类型），集合存放的类型可以不是一种(不加泛型时添加的类型是Object)。（当将元素放入集合时，它们会被转换成Object类型，之后在访问集合中的元素时需要进行强制类型转换。这种设计虽然方便，但也带来了类型不安全的隐患，以及类型转换的性能损失。）
- 4、数组是java语言中内置的数据类型,是线性排列的,执行效率或者类型检查都是最快的。

##### 工具

- 遍历集合：Iterator 和 Enumeration
- 操作集合：Arrays 和 Collections

##### Map

```
1、HashMap 哈希映射 无序 ， key 和 value 都可以为null 
2、TreeMap 红黑树实现的, 可排序， 红黑树是一种自平衡二叉查找树
3、LinkedHashMap  链表映射 ，继承于HashMap,又实现了双向链表的特性 ，保留了元素插入顺序 
```

| 类型             | 底层实现                                                     | 线程安全 | 扩容方式                                                 | 特点                        |
| ---------------- | ------------------------------------------------------------ | -------- | -------------------------------------------------------- | --------------------------- |
| HashMap          | 数组+红黑树                                                  | 否       | 初始容量是 16，2倍扩容                                   | 无序集合                    |
| LinkedHashMap    | 基于HashMap，`并自己维持了一个双向链表`，按照插入顺序进行访问，实现了LRU算法 | 否       | 初始容量是 16，2倍扩容                                   | 有序集合                    |
| CocurrentHashMap | Segments数组+ReentrantHashMap(`作为互斥锁来控制并发访问`)+链表，采用分段锁保证安全 | 是       | 链表元素超8个，数组大小超64，转红黑树                    | 无序集合，性能比HashTable好 |
| HashTable        | 数组+链表                                                    | 是       | 初始大小为11，扩容为2n+1(为了得到一个素数，减少哈希碰撞) | 无序集合                    |
| TreeMap          | 基于红黑树                                                   | 不       | 2倍                                                      | 有序集合                    |

为什么是2n+1？

在哈希表中，最终的数组索引通常是通过对哈希码进行模运算（取余）得到的，即 index = hashCode(key) % arraySize。如果哈希表的大小是一个偶数，那么由于大多数哈希函数会产生均匀分布的整数，那些产生偶数哈希码的键将只能映射到偶数索引；反之亦然。这意味着表的一半空间未被充分利用，导致碰撞的可能性增加。在选择奇数作为表的大小时，没有这样的限制，每个索引位置都有相同的机会被填充，从而减少了碰撞

> - List：有序、可重复。
> - Set：无序、不可重复的集合。重复元素会覆盖掉。
> - Map：键值对，键唯一、值不唯一。Map 集合中存储的是键值对，键不能重复，值可以重复。

##### TreeMap

- **插入**：在添加新元素时，`TreeMap` 将该元素作为一个节点插入到红黑树中，并执行必要的旋转或重新着色操作以保持红黑树的平衡。
- **删除**：在删除元素时，`TreeMap` 找到并移除指定的节点，并执行任何必要的旋转或重新着色以维持树的平衡。
- **查找**：查找操作利用了二叉搜索树的特性，可以快速定位到具体的节点。
- **迭代**：`TreeMap` 提供了有序的键集、值集合和键值对集合视图，可以按升序或者降序遍历键值对

```markdown
## 为什么不经常使用？

1. **性能**：对于大多数操作，`TreeMap` 提供 O(log n) 的时间复杂度，而 `HashMap` 提供近似 O(1) 的性能。在随机访问和更新元素时，`HashMap` 通常更快。
2. **内存占用**：`TreeMap` 因为基于红黑树的数据结构，每个节点都需要额外的存储空间来维护树的结构（如指向子节点的引用、节点颜色等），这比 `HashMap` 在内存上的开销要大。
3. **排序不总是必要**：`TreeMap` 维护键的有序状态，但并不是所有的应用场景都需要键的顺序。如果不需要排序，使用 `HashMap` 会更加高效。
4. **Null 键问题**：`TreeMap` 不允许键为 `null`，因为它依赖于键之间的比较操作。而 `HashMap` 允许一个 `null` 键。
```

##### LinkedHashMap

LinkedHashMap继承自 HashMap，在 HashMap 基础上，通过维护一条双向链表，解决了 HashMap 不能随时保持遍历顺序和插入顺序一致的问题。在一些场景下，该特性很有用，比如缓存。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/17341aae7d364066b4088d91679d667c.png)

###### LinkedHashMap实现LRU

accessOrder用于决定具体的迭代顺序：

当accessOrder标志位为true时，put和get方法均有调用recordAccess方法，将当前访问的Entry（put进来的Entry或get出来的Entry）移到双向链表的尾部，双向链表中的元素按照访问的先后顺序排列。

当标志位accessOrder的值为false时，只有put方法会调用recordAccess，即每次put到LinkedHashMap中的Entry都放在双向链表的尾部，按照Entry插入LinkedHashMap到中的先后顺序排序

```java
public class LRU<K,V> extends LinkedHashMap<K, V> implements Map<K, V>{
    private static final long serialVersionUID = 1L;
    public LRU(int initialCapacity,
             float loadFactor,
                        boolean accessOrder) {
        super(initialCapacity, loadFactor, accessOrder);
    }
 
    /** 
     * @description 重写LinkedHashMap中的removeEldestEntry方法，当LRU中元素多余6个时，
     *              删除最不经常使用的元素 **/
    @Override
    protected boolean removeEldestEntry(java.util.Map.Entry<K, V> eldest) {
        // TODO Auto-generated method stub
        if(size() > 6){
            return true;
        }
        return false;
    }
    public static void main(String[] args) {
        LRU<Character, Integer> lru = new LRU<Character, Integer>(
                16, 0.75f, true);
        String s = "abcdefghijkl";
        for (int i = 0; i < s.length(); i++) {
            lru.put(s.charAt(i), i);
        }
        System.out.println("LRU中key为h的Entry的值为： " + lru.get('h'));
        System.out.println("LRU的大小 ：" + lru.size());
        System.out.println("LRU ：" + lru);
    }
}
```

###### hashMap和LinkedHashMap区别

- 插入顺序：HashMap不保证映射的顺序，而LinkedHashMap会根据元素插入的顺序维护一个双向链表，因此保证了映射的顺序，可以通过get操作访问元素的插入顺序。
- `迭代顺序：`LinkedHashMap迭代元素的顺序是插入顺序，而HashMap的迭代顺序是随机的。
- `性能：`由于LinkedHashMap在底层额外维护了一个双向链表，因此在插入或删除元素时需要更多的操作，因此LinkedHashMap的性能通常比HashMap要低，内存占用通常比HashMap要高一些。

##### hashMap和TreeMap区别

- `插入顺序：`HashMap不保证映射的顺序，而TreeMap会根据元素的键值进行排序，因此保证了映射的顺序。
- `元素访问时间复杂度：`HashMap的元素访问时间复杂度是常数级别的，即O(1)，而TreeMap的元素访问时间复杂度是基于红黑树的复杂度，通常是O(log(n))。
- `键的类型：`HashMap可以使用任何类型的对象作为键，只要它们能正确的实现hashCode()和equals()方法，而TreeMap的键必须实现Comparable接口或提供自定义的Comparator比较器来进行比较。
- `内存占用：`由于TreeMap需要维护红黑树的结构，因此它的内存占用相对较高。而HashMap在元素较少时，占用内存较小。

##### HashMap原理

- HashMap在Jdk1.8以后是基于数组+链表+红黑树来实现的，特点是，key不能重复，可以为null，线程不安全

- HashMap的扩容机制：

HashMap的默认容量为16，默认的负载因子为0.75，当HashMap中元素个数超过容量乘以负载因子的个数时，就创建一个大小为前一次两倍的新数组，再将原来数组中的数据复制到新数组中。当数组长度到达64且链表长度大于8时，链表转为红黑树

- HashMap存取原理：

（1）计算key的hash值，然后进行二次hash，根据二次hash结果找到对应的索引位置

（2）如果这个位置有值，先进性equals比较，若结果为true则取代该元素，若结果为false，就使用高低位平移法将节点插入链表

- 为什么不一开始就使用红黑树？

​		因为直接采用红黑树的话每次加入元素需要进行平衡，而在超过8时再旋转变为红黑树可以达成平衡，因为大部分哈希槽的元素个数**正态分布在8个**左右，所以此时变为红黑树也满足了查找的效率。

```
- 想要线程安全的哈希表
（1）使用ConcurrentHashMap
（2）使用HashTable
（3）Collections.synchronizedHashMap()方法
```

```markdown
## hash表：
构造：① 直接定址法；②平方取中法；③折叠法；④除留取余法
冲突解决：① 开放定址法（线性探测）② 链地址法
```

##### HashTable与HashMap的区别

- （1）HashTable的每个方法都用synchronized修饰，因此是线程安全的，但同时读写效率很低

- （2）HashTable的Key不允许为null

- （3）HashTable只对key进行一次hash，HashMap进行了两次Hash

- （4）HashTable底层使用的数组加链表

##### ConcurrenHashMap与HashTable的区别

ConcurrentHashMap性能更高，它基于分段锁+CAS 保证线程安全，分段锁基于 synchronized 实现，**它仅仅锁住某个数组的某个槽位，而不是整个数组**

1. ConcurrentHashMap 没有大量使用 `synchronsize` 这种重量级锁。而是在一些关键位置使用乐观锁(CAS，Compare-And-Swap), 线程可以无阻塞的运行。
2. ConcurrentHashMap读方法没有加锁
3. ConcurrentHashMap扩容时老数据的转移是并发执行的，这样扩容的效率更高。

##### ArrayList和LinkedList的区别

ArrayList的底层使用动态数组，默认容量为10，当元素数量到达容量时，生成一个新的数组，大小为前一次的1.5倍，然后将原来的数组copy过来；

因为数组有索引，所以ArrayList查找数据更快，但是添加数据效率更低

LinkedList的底层使用链表，在内存中是离散的，没有扩容机制；LinkedList在查找数据时需要从头遍历，所以查找慢，但是添加数据效率更高

```
如何保证ArrayList的线程安全？
（1）使用collentions.synchronizedList（）方法为ArrayList加锁
（2）使用Vector，Vector底层与Arraylist相同，但是每个方法都由synchronized修饰，速度很慢
```

```
在Queue接口中， poll() 和 remove() 方法都用于从队列中移除并返回队头的元素。
如果队列为空，即没有元素可供移除时, pol0 方法会返回null。它是一个安全的方法不会抛出异常。
remove()在没有元素可供移除时，会抛出NoSuchElementException 异常。
```

##### 怎么确保集合不可更改？

Java 集合框架提供了一些不可变集合类，如不可变列表（ImmutableList）、不可变集合（ImmutableSet）和不可变映射（ImmutableMap）。

```java
List<String> list = ImmutableList.of("a", "b", "c");
Set<String> set = ImmutableSet.of("x", "y", "z");
Map<String, Integer> map = ImmutableMap.of("a", 1, "b", 2, "c", 3);
List<String> list2 = ImmutableList.<String>builder().addAll(list1).add("d").build();
```

#### Lambda表达式

```java
// 使用 Lambda 表达式计算两个数的和
MathOperation addition = (a, b) -> a + b;
// 调用 Lambda 表达式
int result = addition.operation(5, 3);
// MathOperation 是一个函数式接口，它包含一个抽象方法 operation，Lambda 表达式 (a, b) -> a + b 实现了这个抽象方法，表示对两个参数进行相加操作。
// 在 Lambda 表达式当中不允许声明一个与局部变量同名的参数或者局部变量。
```

##### Lambda表达式与匿名内部类：

- 内部类编译之后将会产生两个 class 文件（编译器自动命名Test$1.class），lambda编译后只会产生一个文件 （LambdaTest.class），即 Lambda 表达式不会产生新的类
- Lambda 表达式中使用 this 关键字时，其指向的是外部类的引用（因为其不会创建匿名内部类）

##### 函数式接口

函数式接口是为使现有的函数友好地支持Lambda表达式而提出的概念

```java
@FunctionalInterface // Java 8为函数式接口引入了一个新注解@FunctionalInterface，主要用于编译级错误检查，加上该注解，当你写的接口不符合函数式接口定义的时候，编译器会报错。
interface GreetingService {
  void sayMessage(String message);
}
GreetingService greetService1 = message -> System.out.println("Hello " + message);
```

- 函数式接口只定义了唯一的抽象方法的接口（除了隐含的Object对象的公共方法），若增加新的方法，就非函数式接口，编译会失败

- 函数式接口里还允许定义 java.lang.Object 里的 public 方法，因为任何一个函数式接口的实现，默认都继承了 Object 类，包含了来自 java.lang.Object 里对这些抽象方法的实现
- 可以在接口中添加默认方法与静态方法，无论几个

```java
@FunctionalInterface
public interface XttblogService{
    void sayMessage(String message);
  
    default void doSomeMoreWork1(){//增加默认方法（default），static和default不能同时使用
    }
    static void printHello(){  //增加静态方法（static）
      System.out.println("Hello");
    }
    @Override //Object类中的public方法
    String toString();
}
```

```java
// 典型的函数式接口：
java.lang.Runnable
java.util.concurrent.Callable
java.util.Comparator
```

##### 方法引用

可以直接引用已有Java类或对象的方法或构造器。方法引用与lambda表达式结合使用，可以进一步简化代码。

```java
new Random().ints(10)
        .map(i->Math.abs(i))
        .forEach(i -> System.out.println(i));
//其中 i -> Math.abs(i)代理了如下代码：
new IntUnaryOperator() {
    @Override
    public int applyAsInt(int operand) {
        return Math.abs(operand);
    }
}
// 通过方法引用进一步简化：
new Random().ints(10)
        .map(Math::abs)
        .forEach(System.out::println);
```

四种方法引用:

| 种类                                 | 语法                                 | 例子           |
| ------------------------------------ | ------------------------------------ | -------------- |
| 静态方法引用                         | ContainingClass::staticMethodName    | Math::abs      |
| 特定对象的实例方法引用               | containingObject::instanceMethodName | this::equals   |
| 实例方法的任意一个特定类型的对象引用 | ContainingClass::staticMethodName    | String::concat |
| 构造器引用                           | ClassName::new                       | HashSet::new   |

```java
@FunctionalInterface // 上表中的3
interface TestInterface<T> {
    String handleString(T a, String b);
}
class TestClass {
    String oneString;
    public String concatString(String a) {
        return this.oneString + a;
    }
    public String startHandleString(TestInterface<TestClass> testInterface, String str) {
        String result = testInterface.handleString(this, str);
        return result;
    }
}

public class Test {
    public static void main(String[] args) {
        TestClass testClass = new TestClass();
        testClass.oneString = "abc";
        String result = testClass.startHandleString(TestClass::concatString, "123");
        System.out.println(result);

        //相当于以下效果
        TestClass testClass2 = new TestClass();
        testClass2.oneString = "abc";
        TestInterface theOne2 = (a, b) -> testClass2.concatString(b);
        String result2 = theOne2.handleString(theOne2, "123");
        System.out.println(result2);

    }
}
```

#### 泛型[ref](https://www.cnblogs.com/zhaojinhui/p/17566192.html#%E5%9B%9B%E6%B3%9B%E5%9E%8B%E6%96%B9%E6%B3%95)

##### 定义

```java
// 正确示范：
ArrayList<String> list = new ArrayList<>();
ArrayList<String> list = new ArrayList<>(10); // 初始容量，这有助于提升性能，因为它减少了数组扩容的次数
ArrayList<String> list = new ArrayList<>(anotherList); // 从另一个集合初始化

// 错误示范：
ArrayList<String> list = new ArrayList<String>(); // Java 7+ 中，右侧的尖括号内不再需要重复左侧的类型,编译器会根据左侧的声明推断出相应的类型
ArrayList<int> list = new ArrayList<>(); // 错误，int 是基本数据类型，应该使用它的封装类 Integer
ArrayList<Object> list = new ArrayList<String>(); // 错误，泛型类型不匹配
```

在Java 7及以上版本中，可以利用“钻石语法”（diamond syntax）简化代码，即在右侧的尖括号中省略具体的类型参数，编译器会根据左侧的声明推断出相应的类型。

```
E - Element (在集合中使用，因为集合中存放的是元素)
 T - Type（Java 类）
 K - Key（键）
 V - Value（值）
 N - Number（数值类型）
```

**泛型类：**通过泛型可以完成对一组类的操作对外开放相同的接口。最典型的就是各种容器类，如：**List、Set、Map。**

T可以随便写为任意标识，常见的如T、E、K、V等形式的参数

```java
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{
    //在类中声明的泛型整个类里面都可以用，除了静态部分，因为泛型是实例化时声明的。
    //静态区域的代码在编译时就已经确定，只与类相关
    class A <E>{
        T t;
    }
    //类里面的方法或类中再次声明同名泛型是允许的，并且该泛型会覆盖掉父类的同名泛型T
    class B <T>{
        T t;
    }
    //静态内部类也可以使用泛型，实例化时赋予泛型实际类型
    static class C <T> {
        T t;
    }
    public static void main(String[] args) {
        //报错，不能使用T泛型，因为泛型T属于实例不属于类
        T t = null;
    }
    public static <T> void show(T t){
 
    }
    //key这个成员变量的类型为T,T的类型由外部指定
    private T key;
 
    public Generic(T key) { //泛型构造方法形参key的类型也为T，T的类型由外部指定
        this.key = key;
    }
 
    public T getKey(){ //泛型方法getKey的返回值类型为T，T的类型由外部指定
        return key;
    }
}
```

泛型类，是在实例化类的时候指明泛型的具体类型；

```java
public class Generic<T> {
    private T key;
    public Generic(T key) {
        this.key = key;
    }
    public T getKey(){
        return key;
    }
}
// 当创建一个 Generic< T > 类对象时，会向尖括号 <> 中传入具体的数据类型。
@Test
public void test() {
    Generic<String> generic = new Generic<>();// 传入 String 类型
    // <> 中什么都不传入，等价于 Generic<Object> generic = new Generic<>();
    Generic generic = new Generic();
}
// 传入 String 类型时，原泛型类可以想象它会自动扩展，其类型参数会被替换:
public class Generic { 
    private String key;
    public Generic(String key) { 
        this.key = key;
    }
    public String getKey() { 
        return key;
    }
}
```

泛型方法，是在调用方法的时候指明泛型的具体类型 ，当在一个方法签名中的返回值前面声明了一个 < T > 时，该方法就被声明为一个泛型方法。< T >表明该方法声明了一个类型参数 T，并且这个类型参数 T 只能在该方法中使用。当然，泛型方法中也可以使用泛型类中定义的泛型参数。

```java
public class Test<U> {
    // 该方法只是使用了泛型类定义的类型参数，不是泛型方法
    public void testMethod(U u){
        System.out.println(u);
    }
    // <T> 真正声明了下面的方法是一个泛型方法， < T >表明该方法声明了一个类型参数 T，并且这个类型参数 T 只能在该方法中使用。为了避免混淆，如果在一个泛型类中存在泛型方法，那么两者的类型参数最好不要同名。
    public <T> T testMethod1(T t){
        return t;
    }
}
// 泛型方法中可以同时声明多个类型参数。
public class TestMethod<U> {
    public <T, S> T testMethod(T t, S s) {
        return null;
    }
}

public class Test2<T> {
    // 泛型类定义的类型参数 T 不能在静态方法中使用
    // 但可以将静态方法声明为泛型方法，方法中便可以使用其声明的类型参数了
    public static <E> E show(E one) {
        return null;
    }
}
```

当调用泛型方法时，根据外部传入的实际对象的数据类型，编译器就可以判断出类型参数 T所代表的具体数据类型。

```java
public class Demo {
    public static void main(String args[]) {
        GenericMethod d = new GenericMethod(); // 创建 GenericMethod 对象  

        String str = d.fun("汤姆"); // 给GenericMethod中的泛型方法传递字符串  
        int i = d.fun(30);  // 给GenericMethod中的泛型方法传递数字，自动装箱  
        System.out.println(str); // 输出 汤姆
        System.out.println(i);  // 输出 30

        GenericMethod.show("Lin");// 输出: 静态泛型方法 Lin
    }
}

class GenericMethod {
    // 普通的泛型方法
    public <T> T fun(T t) { // 可以接收任意类型的数据
        return t;
      }

    // 静态的泛型方法
    public static <E> void show(E one){
         System.out.println("静态泛型方法 " + one);
    }
}
```

- 当泛型方法的形参列表中有多个类型参数时，在不指定类型参数的情况下，方法中声明的的类型参数为泛型方法中的几种类型参数的共同父类的最小级，直到 Object。
- 在指定了类型参数的时候，传入泛型方法中的实参的数据类型必须为指定数据类型或者其子类。

```java
public class Test {

    // 这是一个简单的泛型方法
    public static <T> T add(T x, T y) {
        return y;
    }

    public static void main(String[] args) {
        // 一、不显式地指定类型参数
        //（1）传入的两个实参都是 Integer，所以泛型方法中的<T> == <Integer> 
        int i = Test.add(1, 2);

        //（2）传入的两个实参一个是 Integer，另一个是 Float，
        // 所以<T>取共同父类的最小级，<T> == <Number>
        Number f = Test.add(1, 1.2);

        // 传入的两个实参一个是 Integer，另一个是 String，
        // 所以<T>取共同父类的最小级，<T> == <Object>
        Object o = Test.add(1, "asd");
        // 二、显式地指定类型参数
        //（1）指定了<T> = <Integer>，所以传入的实参只能为 Integer 对象
        int a = Test.<Integer>add(1, 2);

        //（2）指定了<T> = <Integer>，所以不能传入 Float 对象
        int b = Test.<Integer>add(1, 2.2);// 编译错误
        //（3）指定<T> = <Number>，所以可以传入 Number 对象
        // Integer 和 Float 都是 Number 的子类，因此可以传入两者的对象
        Number c = Test.<Number>add(1, 2.2);
    }
}
```

##### 类型擦除 和 转型

泛型的本质是将数据类型参数化，它通过擦除的方式来实现，即编译器会在编译期间**擦除**代码中的所有泛型语法，并相应的做出一些**类型转换**动作。

擦除是为了保证新生成的字节码与旧版本的 Java 代码兼容，转型是为了后面正常使用。

成功编译过后的 class 文件中不包含任何泛型信息，泛型信息不会进入到运行时阶段。

```java
List<String> list = new ArrayList<>();
list.add("hello");
String s = list.get(0); // 自动类型转换
变为 ->
List list = new ArrayList();
list.add("hello");
String s = (String) list.get(0); // 显式类型转换
```

```java
public class GenericType {
    public static void main(String[] args) {
        ArrayList<String> arrayString = new ArrayList<String>();
        ArrayList<Integer> arrayInteger = new ArrayList<Integer>();
        System.out.println(arrayString.getClass() == arrayInteger.getClass());// true
    }
}
// 在编译期间，所有的泛型信息都会被擦除， ArrayList< Integer > 和 ArrayList< String >类型，在编译后都会变成ArrayList< Object>类型。
```

```java
public class Caculate<T> {
    private T num;
}//变为：
public class Caculate {
    public Caculate() {}// 默认构造器，不用管
    private Object num;// T 被替换为 Object 类型
}

public class Caculate<T extends Number> {
    private T num;
}// 变为：
public class Caculate {
    public Caculate() {}// 默认构造器，不用管

    private Number num;
}
```

1. 泛型信息（包括泛型类、接口、方法）只在代码编译阶段存在，在代码成功编译后，其内的所有泛型信息都会被擦除，并且类型参数 T 会被统一替换为其原始类型（默认是 Object 类，若有 extends 或者 super 则另外分析）；
2. 在泛型信息被擦除后，若还需要使用到对象相关的泛型信息，编译器底层会自动进行类型转换（从原始类型转换为未擦除前的数据类型）。

##### 通配符

通配符`?`是一种特殊的泛型类型，用于限定泛型类型参数的范围，在java中，数组是可以协变的，比如dog extends Animal，那么Animal[] 与dog[]是兼容的。而<u>集合是不能协变</u>的，也就是说List<Animal>不是List<dog>的父类，这时候就可以用到通配符了。

**限定通配符**包括两种：
 \1. 表示类型的上界，格式为：<？ extends T>，即类型必须为T类型或者T子类
 \2. 表示类型的下界，格式为：<？ super T>，即类型必须为T类型或者T的父类

```java
public class Test { //上界
	public static void printIntValue(List<? extends Number> list) {
    l.add(1); // 编译报错，不能使用add方法，因为我们在写这个方法时不能确定传入的什么类型的数据
		for (Number number : list) {
			System.out.print(number.intValue()+" "); 
		}
		System.out.println();
	}
	public static void main(String[] args) {
		List<Integer> integerList=new ArrayList<Integer>();
		integerList.add(2);
		integerList.add(2);
		printIntValue(integerList);
		List<Float> floatList=new ArrayList<Float>();
		floatList.add((float) 3.3);
		floatList.add((float) 0.3);
		printIntValue(floatList);
	}
}
// 下界
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 10; i++) {
        list.add(i); // 能够调用add方法, 因为这里元素就是Integer类型的, 而传入的list不管是什么, 都一定是Integer或其父类泛型的List
    }
}
public static void main(String[] args) {
    List<Object> list1 = new ArrayList<>();
    addNumbers(list1);
    System.out.println(list1);
    List<Number> list2 = new ArrayList<>();
    addNumbers(list2);
    System.out.println(list2);
    List<Double> list3 = new ArrayList<>();
    // addNumbers(list3); // 编译报错
}
// 所传入的数据类型可能是Integer到Object之间的任何类型, 这是无法预料的, 也就无法接收. 唯一能确定的就是Object, 因为所有类型都是其子类型.
public static void getTest2(List<? super Integer> list) {
    // Integer i = list.get(0); //编译报错
    Object o = list.get(1);
}
```

下界通配符的另一个常见用法：TreeSet<E> t = TreeSet(Comparator<? super E> comparator) ，在Comparator中可以利用类E的任意一个父类的属性进行排序，只有元素是？的子类型的TreeSet才可以应用这个comparator，如下：

```java
public class Person {
    private String name;
    private int age;
    /*
     * 构造函数与getter, setter省略
     */
}

public class Student extends Person {
    public Student() {}

    public Student(String name, int age) {
        super(name, age);
    }
}

class comparatorTest implements Comparator<Person>{
    @Override
    public int compare(Student s1, Student s2) {
        int num = s1.getAge() - s2.getAge();
        return num == 0 ? s1.getName().compareTo(s2.getName()) :  num;
    }
}

public class GenericTest {
    public static void main(String[] args) {
        TreeSet<Person> ts1 = new TreeSet<>(new comparatorTest());
        ts1.add(new Person("Tom", 20));
        ts1.add(new Person("Jack", 25));
        ts1.add(new Person("John", 22));
        System.out.println(ts1);

        TreeSet<Student> ts2 = new TreeSet<>(new comparatorTest());
        ts2.add(new Student("Susan", 23));
        ts2.add(new Student("Rose", 27));
        ts2.add(new Student("Jane", 19));
        System.out.println(ts2);
    }
}
```

**非限定通配符**：无边界的通配符(Unbounded Wildcards), 就是<?>, 比如List<?>，无边界的通配符的主要作用就是让泛型能够接受未知类型的数据. 

```  java      
/* 这种使用List<?>的方式就是父类引用指向子类对象. 注意, 这里不能printList(List<Object> list)的形式, ， 虽然Object类是所有类的父类, 但是List<Object>跟其他泛型的List如List<String>, List<Integer>不存在继承关系,  */
public static void printList(List<?> list) {
    for (Object o : list) {
        System.out.println(o);
    }
}

public static void main(String[] args) {
    List<String> l1 = new ArrayList<>();
    l1.add("aa");
    l1.add("bb");
    l1.add("cc");
    printList(l1);
    List<Integer> l2 = new ArrayList<>();
    l2.add(11);
    l2.add(22);
    l2.add(33);
    printList(l2);
}
// 我们不能对List<?>使用add方法, 仅有一个例外, 就是add(null). 为什么呢? 因为我们不确定该List的类型, 不知道add什么类型的数据才对, 只有null是所有引用数据类型都具有的元素
public static void addTest(List<?> list) {
    Object o = new Object();
    list.add(o); // 编译报错
    list.add(1); // 编译报错
    list.add("ABC"); // 编译报错
    list.add(null);
}
// Object是所有数据类型的父类, 所以只有接受他可以
public static void getTest(List<?> list) {
    String s = list.get(0); // 编译报错
    Integer i = list.get(1); // 编译报错
    Object o = list.get(2);
}
```

*因为我们不知道c的元素类型，我们不能向其中添加对象。add方法有类型参数E作为集合的元素类型。我们传给add的任何参数都必须是一个未知类型的子类。因为我们不知道那是什么类型，所以我们无法传任何东西进去。*

##### 常见泛型使用错误

```java
不可以给Array加泛型
Array<Integer>,//Type Array does not have type parameters
//而使用通配符创建泛型数组是可以的，如下面这个例子：
List<?>[] ls = new ArrayList<?>[10];  

Vector<String> v = new Vector<Object>(); //错误!///不写<Object>没错，写了就是明知故犯

Vector<Object> v = new Vector<String>(); //也错误!
```

#### 枚举

- 默认继承 java.lang.Enum 类，不能继承其他父类，并自动添加了values（获取枚举类中的所有枚举值）和valueOf（获取对应的枚举类型）方法， java.lang.Enum 类实现了 java.lang.Serializable 和 java.lang.Comparable 接口
- enum默认使用 final 修饰，不能派生子类，构造器默认使用 private 修饰，且只能使用 private 修饰
- 枚举类所有实例必须在第一行给出，默认添加 public static final 修饰，用内部类实现，该内部类继承了枚举类。所有枚举常量都通过静态代码块来进行初始化

```markdown
    java.util.EnumSet：保证集合中的元素不重复
    java.util.EnumMap：EnumMap中的 key是enum类型，而value则可以是任意类型
```

注意：

1. 枚举类型对象之间的值比较可以使用“==”直接来比较值是否相等的，因为枚举类Enum已经重写了equals方法
2. 每个枚举都定义了两个属性，name和ordinal，name表示我们定义的枚举常量的名称，如FRIDAY、TUESDAY，而ordinal是一个顺序号，根据定义的顺序分别赋予一个整形值，从0开始。在枚举常量初始化时，会自动为初始化这两个字段，设置相应的值，在构造方法中添加了两个参数。

3. name和ordinal属性都是final的，clone、readObject、writeObject这三个方法也是final的，这三个方法和枚举通过静态代码块来一起进行初始化。
4. clone、readObject、writeObject三个方法保证了枚举类型的不可变性（即不能通过克隆，不能通过序列化和反序列化来复制枚举），保证枚举常量是单例的。**注解是绑定到程序源代码元素的元数据，对运行代码的操作没有影响**

#### 注解

注解是一种标记在 Java 类、方法、字段和其他程序元素上的特殊标签，它提供了一种安全的类似注释的机制，用来将任何的信息或元数据（metadata）与程序元素（类、方法、成员变量等）进行关联。

注解分为三类：

1. 内置注解：这些注解用于特殊的用途，如告诉编译器生成警告或错误，控制序列化过程等。

   ```markdown
   1. @Override 注解用于告诉编译器，希望重写（覆盖）父类中的方法。如果父类中不存在与该方法签名匹配的方法，编译器会产生一个错误。
   2. @Deprecated 注解用于标记方法、类或字段已过时，不推荐使用。编译器会发出警告，提示开发者尽量避免使用被标记为过时的元素。
   3. @SuppressWarnings 注解用于告诉编译器忽略特定类型的警告。这对于处理旧代码或集成第三方库时非常有用。
   @SuppressWarnings("unchecked")
   public List<String> getItems() {
       // 忽略类型未检查的警告
       return new ArrayList();
   }
   ```

2. 自定义注解：可以用来添加程序的元数据，或者用于特定的用途，例如测试框架、依赖注入等

   ```java
   // 定义自定义注解
   public @interface MyAnnotation {
       String value() default "default value"; // 定义一个元素
       int number() default 0; // 定义另一个元素
   }
   @MyAnnotation(value = "Custom Value", number = 42)
   public class MyClass {
       // 类的内容
   }
   Class<?> clazz = MyClass.class;
   MyAnnotation annotation = clazz.getAnnotation(MyAnnotation.class);
   
   if (annotation != null) {
       String value = annotation.value();
       int number = annotation.number();
       
       System.out.println("Value: " + value);
       System.out.println("Number: " + number);
   } else {
       System.out.println("MyAnnotation not found.");
   }
   // 注解的元素可以是基本数据类型、字符串、枚举类型、注解类型或以上类型的数组
   public @interface MyAnnotation {
       int value() default 0;
       String name() default "John";
       Color color() default Color.RED;
       String[] tags() default {};
       Class<?>[] classes() default {};
       MyOtherAnnotation otherAnnotation() default @MyOtherAnnotation;
   }
   ```

3. 元注解：是用于定义注解的注解，包括@Retention、@Target、@Inherited、@Documented 等6种

```
@Retention：指定其所修饰的注解的保留策略
@Document：该注解是一个标记注解，用于指示一个注解将被文档化
@Target：用来限制注解的使用范围
@Inherited：该注解使父类的注解能被其子类继承
```

```java
@Target(ElementType.TYPE) // 该注解可以用在类上
@Retention(RetentionPolicy.RUNTIME) // 注解信息会保留到运行时
public @interface ExcellentStudent {
}
@ExcellentStudent // 我们在 Student 类上应用 @ExcellentStudent 注解
public class Student {
    private String name;
    private int age;
    private double gpa;
    // 构造方法和其他方法省略
}
// 使用反射来查找并识别优秀学生
public class Main {
    public static void main(String[] args) {
        // 获取 Student 类的 Class 对象
        Class<?> clazz = Student.class;
        // 检查类上是否有 ExcellentStudent 注解
        if (clazz.isAnnotationPresent(ExcellentStudent.class)) {
            // 如果有，打印学生信息
            System.out.println("优秀学生信息：");
            Student student = new Student("Alice", 20, 4.0);
            System.out.println(student);
        } else {
            System.out.println("没有优秀学生信息。");
        }
    }
}

```

注意：

- 注解本身不影响程序的运行，只提供了元数据。

- 注解在编译时可以被处理，也可以在运行时被处理，具体取决于注解的类型和用途。

- 自定义注解需要使用 @Retention 指定它的保留策略，通常是 RUNTIME，以便在运行时读取注解信息。

- 注解的元素名称通常为 value，但可以自定义其他名称。

**Retention– 定义该注解的生命周期**

RetentionPolicy.SOURCE：在编译阶段丢弃。这些注解在编译结束之后就不再有任何意义，所以它们不会写入字节码。@Override, @SuppressWarnings都属于这类注解
RetentionPolicy.CLASS：在类加载的时候丢弃。在字节码文件的处理中有用。注解默认使用这种方式
RetentionPolicy.RUNTIME：始终不会丢弃，运行期也保留该注解，因此可以使用反射机制读取该注解的信息。我们自定义的注解通常使用这种方式

**Target –注解用于什么地方**

1. ElementType.CONSTRUCTOR:用于描述构造器
2. ElementType.FIELD:成员变量、对象、属性（包括enum实例）
3. ElementType.LOCAL_VARIABLE:用于描述局部变量
4. ElementType.METHOD:用于描述方法

@Override -标记方法是否覆盖超类中声明的元素。如果它无法正确覆盖该方法，编译器将发出错误
@Deprecated - 表示该元素已弃用且不应使用。如果程序使用标有此批注的方法，类或字段，编译器将发出警告
@SuppressWarnings - 告诉编译器禁止特定警告。在与泛型出现之前编写的遗留代码接口时最常用的
@FunctionalInterface - 在Java 8中引入，表明类型声明是一个功能接口，可以使用Lambda Expression提供其实现
注解方法声明返回类型必须是基本类型，String，Class，Enum或数组类型之一。否则，编译器将抛出错误
@Target注解可以限制应用注解的元素

#### 基本原则

- **单一职责：**一个对象应该只包含单一的职责，并且该职责被完整地封装在一个类中。

  ```
  一个类（或者大到模块，小到方法）承担的职责越多，它被复用的可能性越小，而且如果一个类承担的职责过多，就相当于将这些职责耦合在一起，当其中一个职责变化时，可能会影响其他职责的运作。
  ```

- 开闭原则：面向修改关闭，面向扩展开放

  ```
  设计一个模块的时候，应当使这个模块可以在不被修改的前提下被扩展，即实现在不修改源代码的情况下改变这个模块的行为。
  比如我们的程序员分为Java程序员、C#程序员、C++程序员、PHP程序员、前端程序员等，而他们要做的都是去打代码，而具体如何打代码是根据不同语言的程序员来决定的，我们可以将程序员打代码这一个行为抽象成一个统一的接口或是抽象类，而具体哪个程序员使用什么语言怎么编程，是自己在负责，不需要其他程序员干涉
  ```

  

- 里氏替换：子类可以替换父类

  ```java
  // 我们不需要再继承自Coder了
  public abstract class Coder {
      public void coding() {
          System.out.println("我会打代码");
      }
  
  class JavaCoder extends Coder{
          public void game(){
              System.out.println("艾欧尼亚最强王者已上号");
          }
          /**
           * 这里我们对父类的行为进行了重写，现在它不再具备父类原本的能力了
           */
          public void coding() {
              System.out.println("我寒窗苦读十六年，到最后还不如培训班三个月出来的程序员");
          }
      }
  }
  ```

- 依赖倒置：面向接口编程

  ```java
  代码要依赖于抽象的类，而不要依赖于具体的类；要针对接口或抽象类编程，而不是针对具体类编程
  public class Main {
  
      public static void main(String[] args) {
          UserController controller = new UserController();
      }
  
      interface UserMapper {
          //接口中只做CRUD方法定义
      }
  
      static class UserMapperImpl implements UserMapper {
          //实现类完成CRUD具体实现
      }
  
      interface UserService {
          //业务代码定义....
      }
  
      static class UserServiceImpl implements UserService {
          @Resource   //现在由Spring来为我们选择一个指定的实现类，然后注入，而不是由我们在类中硬编码进行指定
          UserMapper mapper;
          
          //业务代码具体实现
      }
  
      static class UserController {
          @Resource
          UserService service;   //直接使用接口，就算你改实现，我也不需要再修改代码了
  
          //业务代码....
      }
  }
  ```

- 聚合/组合优于继承

  ```java
  class A { // 如果有一天，由于业务的更改，我们的数据库连接操作，不再由A来负责，而是由新来的C去负责，那么这个时候，我们就不得不将需要复用A中方法的子类全部进行修改
      public void connectDatabase(){
          System.out.println("我是连接数据库操作！");
      }
  }
  
  class B {   //不进行继承，而是在用的时候给我一个A，当然也可以抽象成一个接口，更加灵活
      public void test(A a){
          System.out.println("我是B的方法，我也需要连接数据库！");
          a.connectDatabase();   //在通过传入的对象A去执行
      }
  }
  
  // to
  class A {
      public void connectDatabase(){
          System.out.println("我是连接数据库操作！");
      }
  }
  
  class B {   //不进行继承，而是在用的时候给我一个A，当然也可以抽象成一个接口，更加灵活
      public void test(A a){
          System.out.println("我是B的方法，我也需要连接数据库！");
          a.connectDatabase();   //在通过传入的对象A去执行
      }
  }
  ```

- 接口隔离：接口要小而专，而不是大而全

  ```
  客户端不应该依赖那些它不需要的接口。注意，在该定义中的接口指的是所定义的方法。
  一旦一个接口太大，则需要将它分割成一些更细小的接*，使用该接口的客户端仅需知道与之相关的方法即可。将一组相关的操作定义在一个接口中，且在满足高内聚的前提下，接口中的方法越少越好。
  ```

- 迪米特法则：最小知识原则

  ```markdown
  1. 在类的划分上，应当尽量创建松耦合的类，类之间的耦合度越低，就越有利于复用，一个处在松耦合中的类一旦被修改，不会对关联的类造成太大波及。
  2. 在类的结构设计上，每一个类都应当尽量降低其成员变量和成员函数的访问权限。
  3. 在类的设计上，只要有可能，一个类型应当设计成不变类；
  4. 在对其他类的引用上，一个对象对其他对象的引用应当降到最低。
  ```

  ```java
  public class Main {
      public static void main(String[] args) throws IOException {
          Socket socket = new Socket("localhost", 8080);   //假设我们当前的程序需要进行网络通信
          Test test = new Test();
          test.test(socket);   //现在需要执行test方法来做一些事情
      }
  
      static class Test {
          /**
           * 比如test方法需要得到我们当前Socket连接的本地地址
           */
          public void test(Socket socket){
              System.out.println("IP地址："+socket.getLocalAddress());
          }
      }
  }
  // to
  static class Test {
          public void test(String str){   //一个字符串就能搞定，就没必要丢整个对象进来
              System.out.println("IP地址："+str);
          }
      }
  ```

#### 设计模式

```markdown
## 1、创建型模式
对象实例化的模式，创建型模式用于解耦对象的实例化过程。
单例模式：某个类智能有一个实例，提供一个全局的访问点。
工厂模式：一个工厂类根据传入的参量决定创建出哪一种产品类的实例。
抽象工厂模式：创建相关或依赖对象的家族，而无需明确指定具体类。
建造者模式：封装一个复杂对象的创建过程，并可以按步骤构造。
原型模式：通过复制现有的实例来创建新的实例。

## 2、结构型模式
把类或对象结合在一起形成一个更大的结构。
装饰器模式：动态的给对象添加新的功能。
代理模式：为其它对象提供一个代理以便控制这个对象的访问。
桥接模式：将抽象部分和它的实现部分分离，使它们都可以独立的变化。
适配器模式：将一个类的方法接口转换成客户希望的另一个接口。
组合模式：将对象组合成树形结构以表示“部分-整体”的层次结构。
外观模式：对外提供一个统一的方法，来访问子系统中的一群接口。
享元模式：通过共享技术来有效的支持大量细粒度的对象。

## 3、行为型模式
类和对象如何交互，及划分责任和算法。
策略模式：定义一系列算法，把他们封装起来，并且使它们可以相互替换。
模板模式：定义一个算法结构，而将一些步骤延迟到子类实现。
命令模式：将命令请求封装为一个对象，使得可以用不同的请求来进行参数化。
迭代器模式：一种遍历访问聚合对象中各个元素的方法，不暴露该对象的内部结构。
观察者模式：对象间的一对多的依赖关系。
仲裁者模式：用一个中介对象来封装一系列的对象交互。
备忘录模式：在不破坏封装的前提下，保持对象的内部状态。
解释器模式：给定一个语言，定义它的文法的一种表示，并定义一个解释器。
状态模式：允许一个对象在其对象内部状态改变时改变它的行为。
责任链模式：将请求的发送者和接收者解耦，使的多个对象都有处理这个请求的机会。
访问者模式：不改变数据结构的前提下，增加作用于一组对象元素的新功能。
```

设计模式是一组重复的经验，大多数人都知道，编目和代码设计经验。使用设计模式是重用代码，使代码更容易被其他人理解，并确保代码的可靠性。

##### 单例模式

单例模式就是在程序运行中只实例化一次，创建一个全局唯一对象。

饿汉模式：不能实现懒加载，造成空间浪费

```java
//在类加载时就完成了初始化，所以类加载较慢，但获取对象的速度快
public class SingletonObject {
    // 利用静态变量来存储唯一实例
    private static final SingletonObject instance = new SingletonObject();
 
    // 私有化构造函数
    private SingletonObject(){
        // 里面可能有很多操作
    }
 
    // 提供公开获取实例接口
    public static SingletonObject getInstance(){
        return instance;
    }
}
```

懒汉模式：

- 在不加锁的情况下，线程不安全，可能出现多份实例
- 在加锁的情况下，会是程序串行化，使系统有严重的性能问题

```java
public class SingletonObject {
    // 定义静态变量时，未初始化实例
    private static SingletonObject instance;
 
    // 私有化构造函数
    private SingletonObject(){
 
    }
 
    public static SingletonObject getInstance(){
        // 使用时，先判断实例是否为空，如果实例为空，则实例化对象
        // 这段代码在多线程的情况下是不安全的
        if (instance == null)
            instance = new SingletonObject();
        return instance;
    }
}
```

静态内部类单例模式：静态内部类单例模式实例由内部类创建，由于 JVM 在加载外部类的过程中, 是不会加载静态内部类的, 只有内部类的属性/方法被调用时才会被加载, 并初始化其静态属性。

```java
public class SingletonObject {
    private SingletonObject(){
 
    }
    // 单例持有者
    private static class InstanceHolder{
        private  final static SingletonObject instance = new SingletonObject();
    }
    // 
    public static SingletonObject getInstance(){
        // 调用内部类属性
        return InstanceHolder.instance;
    }
}
```

枚举类单例模式：枚举类型是线程安全的，并且只会装载一次，而且枚举类型是所用单例实现中唯一一种不会被破坏的单例实现模式。

```java
public class SingletonObject {
    private SingletonObject(){
 
    }
    private enum Singleton{
        INSTANCE;
        private final SingletonObject instance;
 
        Singleton(){
            instance = new SingletonObject();
        }
 
        private SingletonObject getInstance(){
            return instance;
        }
    }
    public static SingletonObject getInstance(){
        return Singleton.INSTANCE.getInstance();
    }
}
```

除枚举方式外，其他方法都会通过反射的方式破坏单例，反射是通过调用构造方法生成新的对象，所以如果我们想要阻止单例破坏，可以在构造方法中进行判断，若已有实例, 则阻止生成新的实例。

```java
private SingletonObject(){
    if (instance !=null){
        throw new RuntimeException("实例已经存在，请通过 getInstance()方法获取");
    }
}
```

如果单例类实现了序列化接口Serializable, 就可以通过反序列化破坏单例，所以我们可以不实现序列化接口，如果非得实现序列化接口，可以重写反序列化方法readResolve(), 反序列化时直接返回相关单例对象。

```java
public Object readResolve() throws ObjectStreamException {
    return instance;
}
```

##### 工厂模式

- 简单工厂模式：一个工厂对象决定创建哪一种产品类型的实例，只适用于工厂类负责创建的对象较少的场景
- 工厂方法模式：定义一个创建对象的接口，但让实现这个接口的类来决定实例化哪个类。工厂方法让类的实例化推迟到子类中进行

```java
public class CarFactory { //简单工厂,我们的调用者不必依赖我们的创建者，仅仅需要告诉工厂你要什么，无需知道工厂去哪里帮你实现你的需求
	static final int CAR_BYD = 0;
  static final int CAR_TSL = 1;
	public static Car createCar(int type){
		switch (type) {
		case 0:
			return new Byd();
		case 1:
			return new Tsl();
		default:
			System.out.println("无法识别的品牌");
			return null;
		}
	}
}
//抽象工厂
interface CarFactory {
	Car createCar();
}
public class BydFactory implements CarFactory{
	@Override
	public Car createCar() {
		return new Byd();
	}
}
public class TslFactory implements CarFactory{
	@Override
	public Car createCar() {
		return new Tsl();
	}
}
//调用者
public class CarUse {
	public static void main(String[] args) {
		Car c1 = new BydFactory().createCar();
		Car c2 = new TslFactory().createCar();
		c1.run();
		c2.run();
	}
}
//工厂方法模式和简单工厂模式最大的不同在于，简单工厂模式只有一个工厂类，而工厂方法模式有一组实现了相同接口的工厂类。调用者不是什么都找一个工厂，而是需要什么就找什么工厂提供，这样可选择的范围更大
```

##### 抽象工厂模式

- 抽象工厂模式建议为系列中的每件产品明确声明接口（例如椅子、沙发或咖啡桌）。然后，确保所有产品变体都继承这些接口。例如，所有风格的椅子都实现`椅子`接口；所有风格的咖啡桌都实现`咖啡桌`接口，以此类推。
- 接下来，我们需要声明*抽象工厂*——包含系列中所有产品构造方法的接口。例如`create­Chair`创建椅子、`create­Sofa`创建沙发和`create­Coffee­Table`创建咖啡桌。这些方法必须返回**抽象**产品类型，即我们之前抽取的那些接口：椅子，沙发和咖啡桌等等。
- 我们都将基于`抽象工厂`接口创建不同的工厂类。每个工厂类都只能返回特定类别的产品，例如，现代家具工厂  Modern­Furniture­Factory 只能创建现代椅子 Modern­Chair 、现代沙发 Modern­Sofa  和现代咖啡桌Modern­Coffee­Table 对象。
- 客户端代码可以通过相应的抽象接口调用工厂和产品类。你无需修改实际客户端代码，就能更改传递给客户端的工厂类，也能更改客户端代码接收的产品变体。
- 最后一点说明：如果客户端仅接触抽象接口，那么谁来创建实际的工厂对象呢？一般情况下， 应用程序会在初始化阶段创建具体工厂对象。而在此之前，应用程序必须根据配置文件或环境设定选择工厂类别。

```java
抽象产品（Abstract Product）为构成系列产品的一组不同但相关的产品声明接口。
具体产品（Concrete Product）是抽象产品的多种不同类型实现。所有变体（维多利亚/现代）都必须实现相应的抽象产品（椅子/沙发）。
抽象工厂（Abstract Factory）接口声明了一组创建各种抽象产品的方法。
具体工厂（Concrete Factory）实现抽象工厂的构建方法。每个具体工厂都对应特定产品变体，且仅创建此种产品变体。

//----在构造组合这些产品的工厂
interface BydFactory{ //比亚迪工厂
	Tyre addTyre();  //装配轮胎
	Seat addSeat(); //装配座椅
	Engine addEngine(); //装配发动机
}
//高端比亚迪工厂
class HighBydFactory implements BydFactory{
	@Override
	public Engine addEngine() {
		return new HighEngine();
	}
	@Override
	public Seat addSeat() {
		return new HighSeat();
	}
	@Override
	public Tyre addTyre() {
		return new HighTyre();
	}
}
//低端比亚迪工厂
class LowBydFactory implements BydFactory{
	@Override
	public Engine addEngine() {
		return new LowEngine();
	}
	@Override
	public Seat addSeat() {
		return new LowSeat();
	}
	@Override
	public Tyre addTyre() {
		return new LowTyre();
	}
}
```

```markdown
## 简单工厂、工厂方法、抽象工厂对比
​ 1）简单工厂：生产同一产品等级的任意指定的产品。（不支持增加新产品，增加新产品需要修改代码）
​ 2）工厂方法：生产同一产品等级的固定工厂产品。（支持增加新产品）
​ 3）抽象工厂：生产不同产品族的全部产品。（支持增加产品族，但不支持增加新产品）
```

##### 建造者模式

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-2fccf18a39abfd89fa219ea04583a341_1440w.webp)

一般端直接和Director导向器沟通，通过向Director传入不同的ConcreteBuilder（具体建造者）构建不同表示的Product（产品）。所以建造者模式把对象的构建（由Builder的实现来负责的）和装配（由Director负责的）进行了解耦，不同的构建器，相同的装配，也可以做出不同的产品。

- ​    1）可以逐步构建对象、延迟构建步骤，将构建和表示分离;
- ​    2）各个具体的建造者相互独立，您可以重用相同的构建代码；

```java
//抽象建造者
public interface Builder {
 void setTyre(Tyre tyre);
 void setSeat(Seat seat);
 void setEngine(Engine engine);
}
//实际建造者，提供方法设置不同类型的组件
public class BydBuilder implements Builder {
 private Tyre tyre;
 private Seat seat;
 private Engine engine;
 
 @Override
 public void setEngine(Engine engine) {
  this.engine = engine;
 }

 @Override
 public void setSeat(Seat seat) {
        this.seat = seat; 
 }

 @Override
 public void setTyre(Tyre tyre) {
        this.tyre = tyre;
 }
    //得到产品
 public BydCar getMyBydCar(){
  return new BydCar(tyre, seat, engine);
 }
}
class BydCar{
 private Tyre tyre;
 private Seat seat;
 private Engine engine;
 
 public BydCar(Tyre tyre,Seat seat,Engine engine){
  this.tyre = tyre;
  this.seat = seat;
  this.engine = engine;
  System.out.println(this.toString());
 }
}

//创建导向器，提供不同的方法组装不同类型的组件
public class Director {

 public void highBydCar(Builder builder){
  builder.setEngine(new Engine("高端发动机"));
  builder.setTyre(new Tyre("高端轮胎，可用10年"));
  builder.setSeat(new Seat("高端真皮座椅"));
 }
 
 public void lowBydCar(Builder builder){
  builder.setEngine(new Engine());
  builder.setTyre(new Tyre());
  builder.setSeat(new Seat());
 }
 //目前预售中端款比亚迪
 public void MiddleBydCar(Builder builder){
  builder.setEngine(new Engine("高端发动机"));
  builder.setTyre(new Tyre()); //低端轮胎
  builder.setSeat(new Seat()); //低端座椅
 }
}
public class Client { // 客户端调用
 public static void main(String[] args) {
   //导向器
   Director director = new Director();
   //建造者
   BydBuilder builder = new BydBuilder();
   //产品
   System.out.println("顾客1，看中了高端比亚迪");
   director.highBydCar(builder);
   builder.getMyBydCar();
   System.out.println("顾客2，看中了低端比亚迪");
   director.lowBydCar(builder);
   builder.getMyBydCar();
   System.out.println("顾客3，看中了中端比亚迪");
   director.MiddleBydCar(builder);
   builder.getMyBydCar();
   
   System.out.println("顾客4，希望按照自己的意愿定制比亚迪");
   builder.setEngine(new Engine("高端发动机"));
   builder.setSeat(new Seat("高端座椅"));
   builder.setTyre(new Tyre());//低端轮胎
   builder.getMyBydCar();
 }
}
```

##### 装饰器模式

装饰器（Decorator）模式的定义：指在不改变现有对象结构的情况下，动态地给该对象增加一些职责（即增加其额外功能）的模式，它属于对象结构型模式。

装饰器模式的主要优点有：

- 装饰器是继承的有力补充，比继承灵活，在不改变原有对象的情况下，动态的给一个对象扩展功能，即插即用
- 通过使用不用装饰类及这些装饰类的排列组合，可以实现不同效果
- 装饰器模式完全遵守开闭原则

```java
package decorator;
import java.awt.*;
import javax.swing.*;
public class MorriganAensland {
    public static void main(String[] args) {
        Morrigan m0 = new original();
        m0.display();
        Morrigan m1 = new Succubus(m0);
        m1.display();
        Morrigan m2 = new Girl(m0);
        m2.display();
    }
}
//抽象构件角色：莫莉卡
interface Morrigan {
    public void display();
}
//具体构件角色：原身
class original extends JFrame implements Morrigan {
    private static final long serialVersionUID = 1L;
    private String t = "Morrigan0.jpg";
    public original() {
        super("《恶魔战士》中的莫莉卡·安斯兰");
    }
    public void setImage(String t) {
        this.t = t;
    }
    public void display() {
        this.setLayout(new FlowLayout());
        JLabel l1 = new JLabel(new ImageIcon("src/decorator/" + t));
        this.add(l1);
        this.pack();
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisible(true);
    }
}
//抽象装饰角色：变形
class Changer implements Morrigan {
    Morrigan m;
    public Changer(Morrigan m) {
        this.m = m;
    }
    public void display() {
        m.display();
    }
}
//具体装饰角色：女妖
class Succubus extends Changer {
    public Succubus(Morrigan m) {
        super(m);
    }
    public void display() {
        setChanger();
        super.display();
    }
    public void setChanger() {
        ((original) super.m).setImage("Morrigan1.jpg");
    }
}
//具体装饰角色：少女
class Girl extends Changer {
    public Girl(Morrigan m) {
        super(m);
    }
    public void display() {
        setChanger();
        super.display();
    }
    public void setChanger() {
        ((original) super.m).setImage("Morrigan2.jpg");
    }
}
```

![动图](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-fa2f2bf34fc86801bbc53ce18c9a7600_b.webp)

##### 桥接模式

- 定义：就是把抽象和实现分离出来，然后中间通过**组合**来搭建他们之间的桥梁。
- 桥接模式可以取代多重继承的方案，多重继承违背了单一职责原则，复用性较差
- 当一个类存在两个独立变化的维度，且这两个维度都需要进行扩展时，可以使用桥接模式可以解耦这些变化的维度

```java
// 有 电脑品牌 和 电脑类型 两个维度需要扩展
// 电脑品牌
public interface IComputerBrand {
 void open(); //都能开机
 void close(); //都能关机
 void playGame(); //都能玩游戏
}
// 戴尔品牌
public class Dell implements IComputerBrand{
 @Override
 public void close() {
  System.out.print("戴尔电脑关机");
 }
 @Override
 public void open() {
  System.out.print("戴尔电脑开机");
 }
 @Override
 public void playGame() {
  System.out.print("戴尔电脑玩游戏");
 }
}
// 电脑类型
public abstract class IComputerType {
 private IComputerBrand icb; //聚合 电脑品牌
 public IComputerType(IComputerBrand icb) {
     this.icb = icb;
 } 
 public void close() {
  icb.close();
 }
 public void open() {
  icb.open();
 }
 public void playGame() {
  icb.playGame();
 }
}
// 台式机类型
public class TaiSComputer extends IComputerType{
 private String name;
 public TaiSComputer(IComputerBrand icb) {
  super(icb);
  this.name = "台式机";
 }
 @Override
 public void close() {
  super.close();
  System.out.println("-"+name);
 }
 @Override
 public void open() {
  super.open();
  System.out.println("-"+name);
 }
 @Override
 public void playGame() {
  super.playGame();
  System.out.println("-"+name);
 }
}
main{ //使用
  IComputerType tsj1 = new TaiSComputer(new Dell());
  tsj1.open();
  tsj1.playGame();
  tsj1.close();
  System.out.println("--------");
  IComputerType tsj2 = new BjbComputer(new Apple());
  tsj2.open();
  tsj2.playGame();
  tsj2.close();
}
// 输出：
  戴尔电脑开机-台式机
  戴尔电脑玩游戏-台式机
  戴尔电脑关机-台式机
  --------
  苹果电脑开机-笔记本
  苹果电脑玩游戏-笔记本
  苹果电脑关机-笔记本
```

##### 代理模式

代理对象为另一个对象提供替代或占位符，代理控制对原始对象的访问，允许您在请求到达原始对象之前或之后执行某些操作。这样可以在不修改原目标对象的前提下，提供额外的功能操作，扩展目标对象的功能。

**静态代理：**在编译时就已经实现，编译完成后代理类是一个实际的class文件。

```
创建一个接口，然后创建被代理的类实现该接口并且实现该接口中的抽象方法。之后再创建一个代理类，同时使其也实现这个接口。在代理类中持有一个被代理对象的引用，而后在代理类方法中调用该对象的方法。
```

```java
public interface ChinaMobileCompany { //公共接口
 public boolean handleSIMCard();
}
/**
 * 移动营业厅
 */
public class MobileBusinessHall implements ChinaMobileCompany{ // 委托类
 private String name;
 public MobileBusinessHall(String name){
  this.name = name;
 }
 @Override
 public boolean handleSIMCard() {
        System.out.println(name+"办理SIM卡");
  return true;  
 }
}
public class MobileProxy implements ChinaMobileCompany{ //代理商
 private MobileBusinessHall mbh;  // 持有 实际对象 并隐藏
 public MobileProxy(String name){
  if(mbh==null){
   mbh = new MobileBusinessHall(name);
  }
 }
 @Override
 public boolean handleSIMCard() {
  return mbh.handleSIMCard();
 }
}
main{ //使用
  MobileProxy mbp = new MobileProxy("中山路代理店");
  mbp.rechargeCallFee();
  mbp.handleSIMCard();
  mbp.numberLogout();
}
输出：
  中山路代理店充值话费
  中山路代理店办理sIM卡
  请到移动营业大厅办理:注销手机号业务
```

缺点：那会产生很多的代理类，如果实际类中新增了功能，那么所有的代理都得跟着改。

**动态代理：**在运行时生成"虚拟"的代理类，被ClassLoader加载

- 通过反射机制获得代理类的构造方法，方法签名为`getConstructor(InvocationHandler.class)`；

- 通过构造函数获得代理对象并将自定义的`InvocationHandler`实例对象传为参数传入；

```java
public class JDKMobileProxy{ //JDK代理
 
 private Object bean;
 public JDKMobileProxy(Object obj){
  this.bean = obj;
 }
 public Object getProxy(){
        //newProxyInstance的三个参数：
        //第一个参数：ClassLoader loader，定义了由哪个ClassLoader来对生成的代理对象进行加载
        //第二个参数：Class[] interfaces，需要代理的类的接口，就像我们自己写静态代理类一样
        //第三个参数：InvocationHandler h，它也是一个接口，动态代理对象在调用方法的时候，决定关联到哪一个InvocationHandler对象上的invoke方法
        //byte[] proxyClassFile = ProxyGenerator.generateProxyClass(proxyName, interfaces),生成字节码，然后通过这些字节码实现代理类的生成。
  return Proxy.newProxyInstance(this.getClass().getClassLoader(), bean.getClass().getInterfaces(), new InvocationHandler() {
   @Override
   public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    if("numberLogout".equals(method.getName())){
     throw new UndeclaredThrowableException(null, "不能代理注销");
    }
    Object invoke = null;
    try {
     invoke = method.invoke(bean, args);
    } catch (InvocationTargetException e) {
     throw e.getCause();
    }
    return invoke;
   }
  });
 }
}
main{
  ChinaMobileCompany cm = new MobileBusinessHall("中山路代理店");
  JDKMobileProxy jdkProxy = new JDKMobileProxy(cm);
        //获取代理对象
  ChinaMobileCompany yddl = (ChinaMobileCompany) jdkProxy.getProxy();
  yddl.handleSIMCard();
}
```

JDK代理：只能对接口进行代理。如果要代理的类为一个普通类、没有接口，那么Java动态代理就没法使用了，解决方案：   使用 CGLIB，Code Generation Library是一个基于ASM的字节码生成库。

##### 适配器模式

类适配器：Adapter 类，通过继承 src 类，实现 dst  类接口，完成 src->dst 的适配。

```java
 // src
public class Voltage220V{
    public int output220V() {
        int src = 220;
        System.out.println("电压：" + src);
        return src;
    }
}
// dst
public interface IVoltage5V {
    int output5V();
}
// adapter
public class VoltageAdapter extends Voltage220V implements IVoltage5V{
    @Override
    public int output5V() {
        int srcV = output220V();
        int dstV = srcV / 44;
        return dstV;
    }  
}
// 使用
public class Client {

    public static void main(String[] args) {
        System.out.println("适配器模式");
        Phone phone = new Phone();
        phone.charging(new VoltageAdapter());
    }
}
```

1) Java 是单继承机制，所以类适配器需要继承 src 类这一点算是一个缺点, 因为这要求 dst 必须是接口，有一定局限性;

2) src 类的方法在 Adapter 中都会暴露出来，也增加了使用的成本。

对象适配器：基本思路和类的适配器模式相同，只是将 Adapter 类作修改，不是继承 src 类，而是持有 src 类的实例，以解决兼容性的问题。 即：持有 src 类，实现 dst  类接口，完成 src->dst 的适配

```java
public class Client {
    public static void main(String[] args) {
        System.out.println("适配器模式");
        Phone phone = new Phone();
        phone.charging(new VoltageAdapter(new Voltage220V()));
    }
}
```

##### 观察者模式

**观察者模式**是一种行为设计模式，指多个对象间存在一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。 这种模式和发布-订阅模式很像，不同的是，发布-订阅模式，一般有一个调度中心，调度中心有拉模式和推模式来推送消息。

```java
//抽象观察者
public interface Observer {
 void response(); //反应
}
public class ConcreteStudentA implements Observer{
 @Override
 public void response() {
  System.out.println("学生A,收起自己的零食");
 }
}
//抽象主题，提供保存观察者的类，可以新增或移除观察者
public abstract class Subject {
  protected List<Observer> observers = new ArrayList<Observer>();
 //增加观察者方法
    public void add(Observer observer) {
        observers.add(observer);
    }
    //删除观察者方法
    public void remove(Observer observer) {
        observers.remove(observer);
    }
    public abstract void notifyObserver(); //通知观察者方法
}
//具体主题
public class ConcreteSubject extends Subject{

 @Override
 public void notifyObserver() {
  System.out.println("被观察者看到班主任来了，喊了一句：老师来了！");
  for (Object obs : observers) {
   ((Observer)obs).response(); //观察者做出反应
  }
 }
}
```

##### 策略模式

对象有某个行为，但是在不同的场景中，该行为有不同的实现算法.。比如每个人都要“交个人所得税”，但是“在美国交个人所得税”和“在中国交个人所得税”就有不同的算税方法.定义一组算法，将每个算法都封装起来，并且使它们之间可以互换。

思想：在`运行时`(**非编译时**)改变软件的算法行为，定义一个通用的问题，使用不同的算法来实现，然后将这些算法都封装在一个统一接口。

```java
Strategy bySubway = new BySubwayStrategy();
//乘地铁
Context ctx = new Context(bySubway);
ctx.goToPark();
//乘公交
Strategy byBus = new ByBusStrategy();
ctx.setStrategy(byBus);
ctx.goToPark();
```

应用：当有大量条件语句在同一算法的不同变体之间切换时，可将每个条件分支移入它们各自的策略类中替代这些条件语句，来消除这种条件。

##### 责任链模式

在面向对象的开发中，往往力求对象之间保持松耦合，确保对象各自的责任最小化、具体化。经常用Filter做拦截器，即把所有的Filter都放在FilterChain中，然后依次执行每个过滤器，有符合条件的就处理，直到没有其他过滤器，就访问请求的资源。

```java
public interface LeaderChain {
 public abstract void setNextChain(LeaderChain nextChain);
 public abstract void handleRequest(int days);
}
//组长
public class GroupLeader implements LeaderChain{
 private LeaderChain nextChain;
    //这部分可以提取BaseHandle
 @Override
 public void setNextChain(LeaderChain nextChain) {
  this.nextChain=nextChain;
 }

 @Override
 public void handleRequest(int days) {
  if(1==days){
   System.out.println("组长同意");
  }else{
   //交给上一级处理
   this.nextChain.handleRequest(days);
  }
 }
}
//...部门经理，部门总监，总经理
public static void Leaveapplication(int days){
  LeaderChain gl = new GroupLeader();
  LeaderChain dp = new DpManager();
  LeaderChain cf = new CfInspector();
  LeaderChain gm = new GeneralManager();
  gl.setNextChain(dp);
  dp.setNextChain(cf);
  cf.setNextChain(gm);
  //提交请求
  gl.handleRequest(days);
 }
}
```

##### 备忘录模式

​    备忘录模式是一种行为设计模式，又叫快照模式，是指在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，以便以后需要时能将该对象恢复到原先保存的状态。具体采用哪种方法来存储对象状态，取决于对象需要保存时间的长短。

- ​    1）发起人（Originator）角色：记录当前时刻的内部状态，负责定义哪些属于备份范围的状态，负责创建和恢复备忘录数据，它可以访问备忘录里所有的信息；
- ​    2）备忘录（Memento）角色：负责存储发起人的内部状态，在需要的时候提供这些内部状态给发起人；
- ​    3）管理者（Caretaker）角色：对备忘录进行管理、保存和提供备忘录，但其不能对备忘录的内容进行访问与修改。

```java
public class LolOriginator {  // 发起者
  public LolOriginator(String name,String type,Integer fightingNum){
    this.name = name;
    this.type = type;
    this.fightingNum = fightingNum;
   }

   //创建一个备忘录，进行备忘操作
   public LolMemento createMemento(){
    return new LolMemento(this);
   }

   //恢复成指定备忘录
   public void restoreMemento(LolMemento memento){
    this.name = memento.getName();
    this.type = memento.getType();
    this.fightingNum = memento.getFightingNum();
   }
 }
//备忘录
public class LolMemento {

 private String name; //英雄名字
 private String type; //英雄类型
 private Integer  fightingNum; //英雄战斗力
 
 public LolMemento(LolOriginator lol){
  this.name=lol.getName();
  this.type=lol.getType();
  this.fightingNum = lol.getFightingNum();
 }
}
//管理者，负责管理备忘录对象
public class LolCaretaker {
 private LolMemento memento;
 //如果有一连串的状态加成，可以保存多个状态
 //private List<LolMemento> mementoList = new ArrayList<LolMemento>();
 public LolMemento getMemento() {
  return memento;
 }
 public void setMemento(LolMemento memento) {
  this.memento = memento;
 }
}
//使用
 public static void main(String[] args) {
  //备忘录管理者
  LolCaretaker taker = new LolCaretaker();
  //创建一个英雄
  System.out.println("当前的英雄信息：");
  LolOriginator lol = new LolOriginator("德玛西亚之力", "战士", 666);
  System.out.println("  "+lol.toString());
  //进行一次备忘
  taker.setMemento(lol.createMemento());

  System.out.println("吃了一个红buff：");
  lol.setFightingNum(700);
  System.out.println("  "+lol.toString());
  
  System.out.println("红buff已经失效：");
  lol.restoreMemento(taker.getMemento());  //恢复到之前的状态
 }
}
```

##### 迭代器模式

提供一个对象来顺序访问聚合对象中的一系列数据，而不暴露**聚合对象**的内部表示（列表、堆栈、树...）的情况下遍历集合的元素。    所有实现了Iterator接口的都包含一个`Iterator<E> iterator()`方法，它使得调用者并不知道迭代的对象具体由哪个类来实例化的。

- ​    1）如果希望提供一种标准方法来迭代集合并隐藏客户端程序的实现逻辑时，可以使用迭代器模式；
- ​    2）当需要为遍历不同的聚合结构提供一个统一的接口时，可以使用迭代器模式。

```java
public interface Iterator<E> {
    boolean hasNext();
    E next();
    void remove();
}
```

```java
//先声明我们的频道枚举
public enum ChannelTypeEnum {
 CCTV1, CCTV2, CCTV3, JXTV1, JXTV2 ,ALL;
}

//Channel是一个简单的 POJO 类,包含频道号码和频道
public class Channel {
 private Integer number;
 private ChannelTypeEnum TYPE;
 public Channel(Integer number,ChannelTypeEnum TYPE){
  this.number = number;
  this.TYPE = TYPE;
 }
 public Integer getNumber() {
  return number;
 }
 public void setNumber(Integer number) {
  this.number = number;
 }
 public ChannelTypeEnum getTYPE() {
  return TYPE;
 }
 public void setTYPE(ChannelTypeEnum tYPE) {
  TYPE = tYPE;
 }
 @Override
 public String toString() {
  return this.number+"频道是"+this.TYPE;
 }
}
public interface ChannelIterator {
 //判断是否还有元素
 public boolean hasNext();
 //下个元素
 public Channel next();
 //是否是最后一个
 public boolean isLast();
}
public interface ChannelCollection { // 抽象聚合（Aggregate）角色
 public void addChannel(Channel c);
 public void removeChannel(Channel c);
 public ChannelIterator iterator(ChannelTypeEnum type);
}
public class ChannelCollectionImpl implements ChannelCollection {

 //频道列表
 private List<Channel> channelsList;

 public ChannelCollectionImpl() {
  channelsList = new ArrayList();
 }

 //添加频道
 public void addChannel(Channel c) {
  this.channelsList.add(c);
 }

 //移除频道
 public void removeChannel(Channel c) {
  this.channelsList.remove(c);
 }

 //遍历频道
 @Override
 public ChannelIterator iterator(ChannelTypeEnum type) {
  //通过一个内部类实现，以便该实现不被其他类修改（继承重写等）所影响
  return new ChannelIteratorImpl(type, this.channelsList);
 }

 //具体聚合（ConcreteAggregate）角色
 private class ChannelIteratorImpl implements ChannelIterator {

  private ChannelTypeEnum type;
  private List<Channel> channels;
  private int cursor; //定义游标
  public ChannelIteratorImpl(ChannelTypeEnum ty,
    List<Channel> channelsList) {
   this.type = ty;
   this.channels = channelsList;
  }
  @Override
  public boolean hasNext() {
   //通过比较大小来实现
   while (cursor < channels.size()) {
    Channel c = channels.get(cursor);
    if (c.getTYPE().equals(type) || type.equals(ChannelTypeEnum.ALL)) {
     return true;
    } else
     cursor++;
   }
   return false;
  }
  @Override
  public Channel next() {
   Channel c = channels.get(cursor);
   cursor++;
   return c;
  }
  @Override
  public boolean isLast() {
   if(cursor==(channels.size()-1)){
    return true;
   }
   return false;
  }
 }
}
// 使用
public class Client {

 public static void main(String[] args) {
  //新建集合，并添加频道（1-6频道）
  ChannelCollection channels = new ChannelCollectionImpl();
  channels.addChannel(new Channel(1,ChannelTypeEnum.CCTV1));
  channels.addChannel(new Channel(2,ChannelTypeEnum.CCTV2));
  channels.addChannel(new Channel(3,ChannelTypeEnum.CCTV3));
  channels.addChannel(new Channel(4,ChannelTypeEnum.JXTV1));
  channels.addChannel(new Channel(5,ChannelTypeEnum.JXTV2));
  channels.addChannel(new Channel(6,ChannelTypeEnum.JXTV2));
  //遍历所有频道
  ChannelIterator iterator = channels.iterator(ChannelTypeEnum.ALL);
  while(iterator.hasNext()){
   Channel channel = iterator.next();
   System.out.println(channel.toString());
   if(iterator.isLast()){
    System.out.println("我是最后一个了---------------");
   }
  }
  System.out.println("遍历指定的频道---------------");
  //发现有两个JXTV1，只遍历JXTV2
  ChannelIterator jxtv1 = channels.iterator(ChannelTypeEnum.JXTV2);
  while(jxtv1.hasNext()){
   Channel channel = jxtv1.next();
   System.out.println(channel.toString());
  }
  
 }
 
}
```

##### 访问者模式

系统在运行时先确定第一个对象的实际类型，然后再确定第二个对象的实际类型，从而决定执行哪个版本的方法。

```java
// 图形接口
interface Shape {
    void accept(ShapeVisitor visitor);
}

// 圆形类
class Circle implements Shape {
    @Override
    public void accept(ShapeVisitor visitor) {
        visitor.visitCircle(this);
    }
}

// 矩形类
class Rectangle implements Shape {
    @Override
    public void accept(ShapeVisitor visitor) {
        visitor.visitRectangle(this);
    }
}
```

```java
// 访问者接口
interface ShapeVisitor {
    void visitCircle(Circle circle);
    void visitRectangle(Rectangle rectangle);
}

// 渲染访问者
class RenderShapeVisitor implements ShapeVisitor {
    @Override
    public void visitCircle(Circle circle) {
        System.out.println("渲染圆形");
    }

    @Override
    public void visitRectangle(Rectangle rectangle) {
        System.out.println("渲染矩形");
    }
}

// 导出访问者
class ExportShapeVisitor implements ShapeVisitor {
    @Override
    public void visitCircle(Circle circle) {
        System.out.println("导出圆形数据");
    }

    @Override
    public void visitRectangle(Rectangle rectangle) {
        System.out.println("导出矩形数据");
    }
}
```

```java
public class DoubleDispatchExample {
    public static void main(String[] args) {
        // 创建一些图形对象
        Shape circle = new Circle();
        Shape rectangle = new Rectangle();

        // 创建一个渲染访问者并应用到图形对象上
        ShapeVisitor renderVisitor = new RenderShapeVisitor();
        circle.accept(renderVisitor);      // 输出 "渲染圆形"
        rectangle.accept(renderVisitor);  // 输出 "渲染矩形"

        // 创建一个导出访问者并应用到图形对象上
        ShapeVisitor exportVisitor = new ExportShapeVisitor();
        circle.accept(exportVisitor);     // 输出 "导出圆形数据"
        rectangle.accept(exportVisitor); // 输出 "导出矩形数据"
    }
}
```

##### 门面模式

门面（Facade）模式是一种常用的软件设计模式，其主要目标是提供一个高层次的接口，用于简化底层或复杂子系统的访问。在面向对象编程中，它通常通过创建一个类来实现，这个类为客户端代码提供简洁的访问界面，隐藏了系统的复杂性。

- **简化接口**：为复杂的子系统提供统一、简单的接口。

- **隔离**：减少客户端和子系统之间的直接依赖，从而使子系统更容易被独立地开发和演变，而不影响到使用它的客户端代码。

  ```java
  public class HomeTheaterFacade {
      private Amplifier amp;
      private DvdPlayer dvd;
      private Projector projector;
  
      public HomeTheaterFacade(Amplifier amp, DvdPlayer dvd, Projector projector) {
          this.amp = amp;
          this.dvd = dvd;
          this.projector = projector;
      }
  
      public void watchMovie(String movie) {
          System.out.println("Get ready to watch a movie...");
          projector.on();
          projector.wideScreenMode();
          amp.on();
          amp.setDvd(dvd);
          amp.setSurroundSound();
          amp.setVolume(5);
          dvd.on();
          dvd.play(movie);
      }
  
      public void endMovie() {
          System.out.println("Shutting down movie theater...");
          projector.off();
          amp.off();
          dvd.stop();
          dvd.eject();
          dvd.off();
      }
  }
  
  // 客户端代码只需要调用门面提供的方法
  public class Main {
      public static void main(String[] args) {
          HomeTheaterFacade homeTheater = new HomeTheaterFacade(new Amplifier(), new DvdPlayer(), new Projector());
          homeTheater.watchMovie("Raiders of the Lost Ark");
          homeTheater.endMovie();
      }
  }
  ```

#### 1.接口和抽象类的区别

| 参数           | 抽象类                           | 接口                                                         |
| -------------- | -------------------------------- | ------------------------------------------------------------ |
| 构造器         | 有                               | 无                                                           |
| 抽象方法修饰符 | 可以有public、protected和default | 默认修饰符是public，你不可以使用其它修饰符                   |
| main方法       | 抽象方法可以有main方法，可以运行 | java8以后接口可以有default和static方法，所以可以运行main方法 |
| 速度           | 比接口速度要快                   | 慢，因为它需要时间去寻找在类中实现的方法                     |

（1）抽象类有构造方法，而接口没有（所有的类包括抽象类都至少有一个构造器）

（2）抽象类可以有抽象方法和具体方法、静态方法，接口只能有抽象方法public abstract（在jdk1.8以后，接口里可以有静态方法和默认方法）

（3）抽象类的成员4种权限修饰符都可以修饰，接口只能用public（接口中定义的属性隐式地被认为是 `public`, `static`, 和 `final` 的）

（4）类只能继承一个抽象类，可以实现多个接口。

```
接口可以继承接口。抽象类可以实现(implements)接口，抽象类可以继承具体类。抽象类中可以有静态的main方法。
抽象类与普通类的唯一区别就是不能创建实例对象和允许有abstract方法。
```

#### 2.重载和重写的区别

重载发生在同一个类中，方法名相同、参数列表、返回类型、权限修饰符可以不同

重写发生在子类中，方法名相、参数列表、返回类型都相同，权限修饰符要大于父类方法；声明异常范围要小于父类方法；final和private修饰的方法不可重写。

#### 3.==和equals的区别

-  ==可用于基本类型和引用类型：当用于基本类型时候，是比较值是否相同；当用于引用类型的时候，是比较对象是否相同。
- 基本类型没有equals方法，equals只比较值（对象中的内容）是否相同（相同返回true）。
- 一个类如果没有定义equals方法，它将默认继承Object中的equals方法，返回值与==方法相同。

```java
boolean equals(Object o){
	return this==o;
}
```

#### 4.异常处理机制

try只适合处理运行时异常，try+catch适合处理运行时异常+普通异常

（1）使用try、catch、finaly捕获异常，finaly中的代码一定会执行，捕获异常后程序会继续执行（任何执行try 或者catch中的return语句之前,都会先执行finally语句,如果finally存在的话）

（2）使用throws声明该方法可能会抛出的异常类型，出现异常后，程序终止

#### 8.String、StringBuffer、StringBuilder的区别

- 都是final修饰的类，都不允许继承
- String 由 char[] 数组构成，长度不可变，对 String 进行改变时每次都会新生成一个 String 对象，然后把指针指向新的引用对象，如果string在编译时就可以确定是常量，则自动拼接。

- StringBuffer线程安全，StringBuilder线程不安全，它们所有方法都相同，前者添加了synchronized修饰，后者有更好的性能。

- 操作少量字符数据用 String；单线程操作大量数据用 StringBuilder；多线程操作大量数据用 StringBuffer

```markdown
## String不可变的好处
- 提高效率:比如一个字符串Stirngs1=“abc”,“abc”被放到常量池里面去了，我再String s2="abc"并不会复制字符串“abc”,只会多个引用指向原来那个常量，这样就提高了效率，而这一前提是string不可变，如果可变，那么多个引用指向同一个字符串常量，我就可以通过一个引用改变字符串，然后其他引用就被影响了
- 安全:string常被用来表示url，文件路径，如果string可变，或存在安全隐患
- string不可变，那么他的hashcode就一样，不用每次重新计算了
## 字符串常量池
字符串常量池（String Pool）保存着所有字符串字面量（literal strings），这些字面量在编译时期就确定。不仅如此，还可以使用 String 的 intern() 方法在运行过程中将字符串添加到 String Pool 中。
当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等（使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。

## new String("abc")
使用这种方式一共会创建两个字符串对象（前提是 String Pool 中还没有 "abc" 字符串对象）。
    "abc" 属于字符串字面量，因此编译时期会在 String Pool 中创建一个字符串对象，指向这个 "abc" 字符串字面量；
    而使用 new 的方式会在堆中创建一个字符串对象。
String重写了Object类的hashcode和toString方法

## String + 的原理
String c = "xx" + "yy " + a + "zz" + "mm" + b; 实质上的实现过程是： String c = new StringBuilder("xxyy ").append(a).append("zz").append("mm").append(b).toString();
底层通过 StringBuilder实现
```



#### 9.JAVA权限修饰符

![点击查看图片来源](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAR19GaXNoMDMxMw==,size_20,color_FFFFFF,t_70,g_se,x_16.png)

#### 10.JAVA反射机制

反射(Reflection)，是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。由于这种动态性，可以极大的增强程序的灵活性，程序不用在编译期就完成确定，在运行期仍然可以扩展。

##### 实现方式

```java
//方法一，需要导入类的包，依赖太强
Class<CarEntity> carEntityClass0 = CarEntity.class;
//方法二，对象都有了还要反射干什么
CarEntity carEntity =new CarEntity();
Class carEntityClass1 =carEntity.getClass();
//方法三，常用方法
Class carEntityClass2 = Class.forName("com.example.demo3.Entity.CarEntity");
// 三种方法得到的都是同一个引用，在运行期间，一个类，只有一个Class对象产生
```

##### 使用方法

| 操作                                           | 说明                                                         |
| ---------------------------------------------- | :----------------------------------------------------------- |
| getName()                                      | 获得类的完整名字                                             |
| getFields()                                    | 获得类的public类型的属性                                     |
| getDeclaredFields()                            | 获得类的所有属性。包括private 声明的和继承类                 |
| getMethod(String name, Class[] parameterTypes) | 获得类的特定方法，name参数指定方法的名字，parameterTypes 参数指定方法的参数类型。 |
| newInstance()                                  | 通过类的不带参数的构造方法创建这个类的一个对象               |
| getSuperClass()                                | 用于返回表示该 Class 表示的任何类、接口、原始类型或任何 void 类型的超类的Class(即父类)。 |

```
Field[] field = carEntityClass.getFields();
Field[] field01 = carEntityClass.getDeclaredFields();
Constructor[] constructors01 = carEntityClass.getDeclaredConstructors();
```

**只需要一个类的全路径，我们就可以掌握这个类的所有情况！**

##### 应用场景

1. 框架中：Hibernate、Spring等框架中都有使用反射技术。比如，在Hibernate中对象持久化就需要将对象转化为实体类，然后存储到数据库中，就需要通过反射来动态创建实体类对象、获取类的属性和方法等。
2. 动态代理：在Java中，代理模式非常常见。如果要在程序运行期间动态的创建代理对象，就需要通过反射获取类、方法等信息。
3. 序列化和反序列化：在Java中，序列化和反序列化都需要使用到反射技术。序列化会将对象转化成字节流，反序列化则将字节流还原为对象。在这个过程中，需要借助反射技术来获取对象的属性信息。
4. 单元测试：在JUnit等测试框架中，测试类会通过反射获取被测试类的信息并执行测试方法。
5. 动态的加载类：Java中的ClassLoader就是通过反射实现的。ClassLoader可以在程序运行期间动态的加载类，从而扩展程序功能。

#### 11.java 中 stream api 和 channel api 的区别

- stream 不会自动缓冲数据，channel 会利用系统提供的发送缓冲区、接收缓冲区（更为底层）
- stream 仅支持阻塞 API，channel 同时支持阻塞、非阻塞 API，网络 channel 可配合 selector 实现多路复用

#### 12.ArrayList和LinkedList区别

- ArrayList：基于动态数组，连续内存存储，适合下标访问（随机访问），扩容机制：因为数组长度固 定，超出长度存数据时需要新建数组，然后将老数组的数据拷贝到新数组，如果不是尾部插入数据还会  涉及到元素的移动
- LinkedList：基于链表，可以存储在分散的内存中，适合做数据插入及删除操作，不适合查询：需要逐一遍历
- 遍历LinkedList必须使用iterator不能使用for循环，因为每次for循环体内通过get(i)取得某一元素时都需 要对list重新进行遍历，性能消耗极大。

#### 13HashMap和HashTable有什么区别？其底层实现是什么？

- （1） HashMap方法没有synchronized修饰，线程非安全，HashTable线程安全；
- （2） HashMap允许key和value为null，而HashTable不允许

##### 底层实现：数组+链表实现

jdk8开始链表高度到8、数组长度超过64，链表转变为红黑树，元素以内部类Node节点存在

- 创建HashMap：创建一个空的HashMap实例 它并不会立即分配内存来初始化数组。如果在创建HashMap时未定义任何参数时，将使用默认的初始容量（16）和默认的加载系数（0.75）。
- 当第一次使用`put(key, value)`方法插入一个键值对时，首先判断哈希表是否为空,如果为空它才会根据初始容量创建一个具有相应大小的数组。这个大小就是默认的初始容量(16)

- 计算key的hash值（通常会对键进行一系列的位运算和散列算法处理，以生成一个整数作为哈希值），二次hash然后对数组长度取模，对应到数组下标，
- 如果没有产生hash冲突(下标位置没有元素)，则直接创建Node存入数组
- 如果产生hash冲突，先进行equal比较，相同则取代该元素，不同，则判断链表高度插入链表（（JDK7采用头插法，JDK8采用尾插法）），链表高度达到8，并且数组长度到64则转变为红黑树，长度低于6则将红黑树转回链表
- key为null，存在下标0的位置
- 动态扩容：如果数组的负载因子（元素数量除以数组长度）超过了阈值，就会触发动态扩容操作。HashMap会创建一个新的更大的数组，通常会将数组的大小扩大为原来的两倍

```markdown
## 为什么将数组的大小扩大为原来的两倍？
这样可以在保持数组长度为2的幂次方的同时，降低哈希碰撞的概率
## 为什么HashMap的长度或者扩容是是2的幂次方？
因为计算索引时，&跟取模值一样。因为 & 效率高于取模
能保证 索引值 肯定在 capacity 中，不会超出数组长度
## 为什么JDK8要使用尾插法 替换 JDK7的头插法?
多线程情况下，可能会造成环形链表
## 扩容时，是如何迁移元素？
如果链表长度超过1，会通过高低算法，把老数组下标对应的链表进行拆分，一半放新数组对应老数组原下标的位置，另一半放新数组下标【老数组下标 + 老数组长度】
```

#### 14.Java中的异常体系

Java中的所有异常都来自顶级父类Throwable(万物即可抛)，有两个子类:Error、 Exception 

- Error是程序无法处理的错误（Out OfMemoryεror、 Thread Death），一旦出现这个错误，大多数情况下程序将被迫停止运行。
- Exception不会导致程序停止，又分为两个部分RunTimeException运行时异常和CheckedException检查异常：

```
RunTimeException常常发生在程序运行过程中，具有不确症性,主要是由于程序的逻辑引起的,难以排查，如除0的产生的异常,会导致程序当前线程执行失败。
CheckedException常常发生在程序编译过程中，必须要使用try- catch(或者 throws)否则编译不通过,会导致程序编译不通过。
```

Try Catch

- 当 try 块中没有发生异常时，finally 块会在 try 块执行完毕后立即执行。
- 当 try 块中发生了异常，并且该异常被 catch 块捕获到时，catch 块会先执行，然后再执行 finally 块。
- 当 try 块中发生了异常，但没有对应的 catch 块来捕获该异常时，finally 块会在异常被抛出之前执行。

```
如果在程序中不捕获异常的话，异常会沿着调用栈向上传播，直到它被捕获并处理，或者达到了调用栈的顶层，会导致：
程序终止：未捕获的异常将导致程序立即终止。
打印异常信息：Java虚拟机(JVM)会将异常的堆栈跟踪信息打印到标准错误流（通常是控制台）。
退出代码：当程序因为未捕获的异常而终止时，程序会以非零值的退出代码结束，表示出现了错误。
```

#### 15.Java中final, finally, finalize关键字

根据修饰变量的作用范围，final会有不同的特性：
 ● **final修饰局部变量时**，在使用之前必须被赋值一次才能使用；（无final也要赋值，只是可以更改）
 ● **final修饰成员变量时**，如果在声明时没有赋值，则叫做“空白final变量”，空白final变量必须在构造方法或静态代码块中进行初始化。
根据修饰变量的数据类型，比如在修饰基本类型和引用类型的变量时，final也有不同的特性：
 ● **final修饰基本类型的变量时**，不能把基本类型的值重新赋值，因此基本类型的变量值不能被改变。
 ● **final修饰引用类型的变量时**，final只会保证引用类型的变量所引用的地址不会改变，即保证该变量会一直引用同一个对象。因为引用类型的变量保存的仅仅是一个引用地址，所以final修饰引用类型的变量时，该变量会一直引用同一个对象，**但这个对象本身的成员和数据是完全可以发生改变的。**

```markdown
- java.lang.String类是final类型的,不可以继承
- final 用于声明属性，方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。
- 内部类要访问局部变量，局部变量必须定义成final类型。
        原因有二
        一：因为jvm编译后，内部类和外部类是两个独立的calss文件，两者的生命周期不一致，机局部方法所在类运行完后被销毁了，此时内部类还在运行中，会出现内部类访问一个不存在的局部变量，jvm采用在内部类中新增一个成员变量和构造函数（编译时由编译器自动生成），将外部类的局部变量赋值到本类中的成员变量，解决这个问题。
        二：若不使用final修饰，会导致局部变量和内部类中引用变量内容不一致，Java设计人员考虑到一致性的问题，强制要求修饰成final类型。
    如果定义为final，java会将这个变量复制一份作为成员变量内置于内部类中
    *但是内部类引用外部类成员变量时就没这个问题，因为外部类的成员变量和内部类的生命周期是一致的。都是成员级的，而局部变量的生命周期时方法体级别。
```

```
finally是异常处理语句结构的一部分，表示总是执行。

finalize是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等。JVM不保证此方法总被调用，finalize 除了执行“不稳定”之外，还有一定的性能问题。
```

```java
// 内部类 编译抛出异常
public class ThreadTest {
    public void function(String a) {
        new Thread(){
            @Override
            public void run() {
                System.out.println(a);
            }
        }.start();
    }
    public static void main(String[] args) {
        new ThreadTest().function("a");
    }
}
```

#### 16.静态变量和实例变量、static和final

```
成员变量就是类里面的变量，包括类变量和实例变量
没有static的成员变量叫实例变量
加了static就叫类变量，也称全局变量(public 修饰)、静态变量都行
```

在语法定义上的区别：静态变量前要加static关键字，而实例变量前则不加。

在程序运行时的区别：实例变量属于某个对象的属性，必须创建了实例对象，其中的实例变量才会被分配空间，才能使用这个实例变量。静态变量不属于某个实例对象，而是属于类，所以也称为类变量，只要程序加载了类的字节码，不用创建任何实例对象，静态变量就会被分配空间，静态变量就可以被使用了。总之，实例变量必须创建对象后才可以通过这个对象来使用，静态变量则可以直接使用类名来引用。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/687474703a2f2f7777312e73696e61696d672e636e2f6c617267652f3030377338484a556c7931673578627562303663646a33306d6e30386c676f742e6a7067.jpeg)

```markdown
## final的好处
1.final关键字提高了性能。JVM和Java应用都会缓存final变量。 
2.final变量可以安全的在多线程环境下进行共享，而不需要额外的同步开销。 
3.使用final关键字，JVM会对方法、变量及类进行优化。
```

#### 17.ArrayList和Vector的区别

（1）同步性：

Vector是线程安全的，也就是说是它的方法之间是线程同步的，而ArrayList是线程序不安全的，它的方法之间是线程不同步的。如果只有一个线程会访问到集合，那最好是使用ArrayList，因为它不考虑线程安全，效率会高些；如果有多个线程会访问到集合，那最好是使用Vector，因为不需要我们自己再去考虑和编写线程安全的代码。

备注：对于Vector&ArrayList、Hashtable&HashMap，要记住线程安全的问题，记住Vector与Hashtable是旧的，是java一诞生就提供了的，它们是线程安全的，ArrayList与HashMap是java2时才提供的，它们是线程不安全的。

（2）数据增长：

即Vector增长原来的一倍，ArrayList增加原来的0.5倍，ArrayList与Vector都可以设置初始的空间大小，Vector还可以设置增长的空间大小，而ArrayList没有提供设置增长空间的方法。

```markdown
## List和Map区别
一个是存储单列数据的集合，另一个是存储键和值这样的双列数据的集合
List中存储的数据是有顺序，并且允许重复；
Map中存储的数据是没有顺序的，其键是不能重复的，它的值是可以有重复的。
List，Set继承Collection接口
```

#### 18.Java创建对象的4种方式

- 1.使用new创建对象 使用new关键字创建对象应该是最常见的一种方式，但我们应该知道，使用new创建对象会增加耦合度。无论使用什么框架，都要减少new的使用以降低耦合度。
- 2.使用反射的机制创建对象 使用Class类的newInstance方法
- 3.采用clone clone时，需要已经有一个分配了内存的源对象，创建新对象时，首先应该分配一个和源对象一样大的内存空间。要调用clone方法需要实现Cloneable接口
- 4.采用序列化机制 使用序列化时，要实现实现Serializable接口，将一个对象序列化到磁盘上，而采用反序列化可以将磁盘上的对象信息转化到内存中。

#### 20.JAVA浅拷贝与深拷贝

- 浅拷贝：创建一个新对象，对对象的基本数据类型的成员变量进行拷贝，但对引用类型的成员变量只进行引用的传递，当对引用类型的内容修改会影响被拷贝的对象。
- 深拷贝：创建一个新对象，除了对基本数据类型的成员变量进行拷贝，对引用类型的成员变量进行拷贝时，创建一个新的对象来保存引用类型的成员变量。

（1）浅拷贝实现方式：类实现Cloneable接口，并且重写clone()方法。BeanUtils.cloneBean(Object obj)

（2）深拷贝：主要是针对类内部的复杂引用变量。两种方式：

1）复杂引用也实现Cloneable，并重写clone（）方法，然后在父对象重写的clone方法中将该复杂引用指向克隆后的引用  2）直接将需要克隆的对象进行序列化，然后反序列化就可以得到一个深拷贝的对象。SerializationUtils.clone(T object)

#### 21.成员变量和局部变量的区别

**语法形式**：从语法形式上看，成员变量是属于类的，而局部变量是在代码块或方法中定义的变量或是方法的参数；成员变量可以被 `public`,`private`,`static` 等修饰符所修饰，而局部变量不能被访问控制修饰符及 `static` 所修饰；但是，成员变量和局部变量都能被 `final` 所修饰。

**存储方式**：从变量在内存中的存储方式来看，如果成员变量是使用 `static` 修饰的，那么这个成员变量是属于类的，如果没有使用 `static` 修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。

**生存时间**：从变量在内存中的生存时间上看，成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动生成，随着方法的调用结束而消亡。

**默认值**：从变量是否有默认值来看，成员变量如果没有被赋初始值，则会自动以类型的默认值而赋值（一种情况例外:被 `final` 修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。

```markdown
## 为什么成员变量有默认值，局部变量必须手动赋值？
如果没有默认值，变量存储的是内存地址对应的任意随机值，程序读取该值运行会出现意外。
成员变量在运行时可借助反射等方法手动赋值，而局部变量不行。
成员变量可能是运行时赋值，无法判断，误报“没默认值”又会影响用户体验，所以采用自动赋默认值。

##  静态方法为什么不能调用非静态成员?
1. 静态方法是属于类的，在类加载的时候就会分配内存，可以通过类名直接访问。而非静态成员属于实例对象，只有在对象实例化之后才存在，需要通过类的实例对象去访问。
2. 在类的非静态成员不存在的时候静态方法就已经存在了，此时调用在内存中还不存在的非静态成员，属于非法操作。
```

#### 22.Java的代理

Java代理模式是一种结构型设计模式，它允许通过创建一个代理对象来间接访问另一个对象，从而控制对原始对象的访问。

包括两个主要组件：代理类和实际的对象。当客户端调用代理对象的方法时，代理对象会将请求转发到实际对象，并在必要时添加额外的功能。这些额外的功能可以是日志记录、安全性检查、缓存等等。

```markdown
静态代理：需要手动编写代理类，代理对象和原始对象都需要实现相同的接口。
动态代理：使用Java反射机制自动生成代理类，在运行时绑定原始对象和代理对象，无需手动编写代理类，但原始对象必须实现接口。
动态代理更加灵活，代理类在程序运行时创建。
## 动态代理应用场景
常见的各种开源框架如：Spring AOP、retrofit（create() 方法）、注解（如wmrouter）、加事务、加权限、加日志
```

```java
// 静态代理
public interface Subject { //接口
    void request();
}
public class RealSubject implements Subject { // 真实类
    @Override
    public void request() {
        System.out.println("RealSubject: Handling request.");
    }
}
public class ProxySubject implements Subject { // 代理类
    private RealSubject realSubject;

    public ProxySubject(RealSubject realSubject) {
        this.realSubject = realSubject;
    }

    @Override
    public void request() {
        System.out.println("ProxySubject: Doing some operation before invoking request.");
        
        // 调用真实对象的方法
        realSubject.request();

        System.out.println("ProxySubject: Doing some operation after invoking request.");
    }
}

```

```java
// JDK动态代理
public interface Developer {
    void code();
    void debug();
}
public class JavaDeveloper implements Developer {
    @Override
    public void code() {
        System.out.println("java code");
    }
    @Override
    public void debug() {
        System.out.println("java debug");
    }
}

public class JavaDynamicProxy {
    public static void main(String[] args) {
        JavaDeveloper jDeveloper = new JavaDeveloper();
        Developer developer = (Developer) 	Proxy.newProxyInstance(jDeveloper.getClass().getClassLoader(), jDeveloper.getClass().getInterfaces(), (proxy, method, params) -> {
            if (method.getName().equals("code")) {
                System.out.println("我是一个特殊的人，code之前先分析问题");
                return method.invoke(jDeveloper, params);
            }
            if (method.getName().equals("debug")) {
                System.out.println("我没有bug");
            }
            return null;
        });
        developer.code();
        developer.debug();
    }
}
//输出：
//我是一个特殊的人，code之前先分析问题
//java code
//我没有bug
```

原理：

> ### 2. Proxy类
>
> Java的`java.lang.reflect.Proxy`类是实现动态代理的关键。它有一个名为`newProxyInstance`的静态方法，此方法用于在运行时动态创建一个实现了指定接口的代理对象。
>
> ### 3. 动态生成字节码
>
> 当你使用`Proxy.newProxyInstance`方法时，JVM会在运行时动态地在内存中生成一个新的类文件（而非物理文件）。这个新生成的类会实现指定的接口，并将所有方法的调用转发到`InvocationHandler`实例。
>
> ### 4. 类加载
>
> 生成的类文件是二进制内容，JVM会使用系统类加载器将其加载到JVM中。加载后的类是一个有效的Java类，因此能够创建相应的实例。
>
> ### 5. 方法转发
>
> 生成的代理类的每一个方法实现基本上都只做了一件事情：将调用转发到与之关联的`InvocationHandler`实例的`invoke`方法。这意味着无论你调用代理实例上的什么方法，最终都会调用`invoke`方法。
>
> ### 6. 自定义逻辑
>
> 你可以在`InvocationHandler`的`invoke`方法中实现自定义的逻辑，比如在目标方法执行前后添加日志记录、权限校验等。`invoke`方法接收三个参数： - 代理类实例 - 被调用的方法对象 - 方法调用的参数数组
>
> ### 7. 方法调用
>
> 如果`invoke`方法决定继续进行方法调用，那么它可以通过反射调用原始对象的对应方法，使用`Method.invoke`。

1. 减少代码量

   静态代理要求开发者为每个需要代理的目标类或接口手动编写一个代理类。随着代理的目标接口数量增加，这会导致大量的样板代码。动态代理使用反射在运行时生成代理对象，从而避免了为每个目标类或接口编写重复代码的需求。

2. 提高可维护性

   由于不需要为每个业务接口编写对应的代理实现，动态代理简化了代码的维护。当业务接口发生变化时（如新增、删除或修改方法），静态代理类可能也需要相应地进行更新，而动态代理则可以自动适应这些变更，无需额外的修改。

3. 支持多种代理策略

4. 实施AOP

   动态代理是实现面向方面编程(AOP)的基础之一。因为它能够在运行时动态地插入横切逻辑，而不改变实际业务对象的代码，所以非常适合实现日志记录、性能监控、事务处理等AOP的场景。

```java
//CGLIB动态代理（Code Generation Library）
public class HelloService {//实际
    public HelloService() {
        System.out.println("HelloService构造");
    }
    final public String sayHello(String name) {
        System.out.println("HelloService:sayOthers>>"+name);
        return null;
    }
    public void sayHello() {
        System.out.println("HelloService:sayHello");
    }
}
public class MyMethodInterceptor implements MethodInterceptor { //拦截器
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        System.out.println("======插入前置通知======");
        Object object = methodProxy.invokeSuper(o, objects);
        System.out.println("======插入后者通知======");
        return object;
    }
}
public static void main(String[] args) {
    //代理类class文件存入本地磁盘方便我们反编译查看源代码
    System.setProperty(DebuggingClassWriter.DEBUG_LOCATION_PROPERTY, "/root/code");
    //通过CGLIB动态代理获取代理对象过程
    Enhancer enhancer = new Enhancer(); // 没有实现任何接口的普通类
    //设置enhancer对象的父类
    enhancer.setSuperclass(HelloService.class);
    // 设置enhancer的回调对象
    enhancer.setCallback(new MyMethodInterceptor());
    //创建代理对象
    HelloService helloService = (HelloService) enhancer.create();
    //通过代理对象调用目标方法
    helloService.sayHello();
}
```

- 优点：CGLIB 动态代理强大的特性之一是它不需要代理类实现任何接口，因此能够代理那些没有实现接口的类，这是 JDK 动态代理所不能做到的。
- 缺点：CGLIB 通过在运行时生成目标类的子类来实现代理功能，这也意味着不能代理标记为 `final` 的类和方法。

#### 23.Java迭代器

Java迭代器（Iterator）是 Java 集合框架中的一种机制，是一种用于遍历集合（如列表、集合和映射等）的接口。

它提供了一种统一的方式来访问集合中的元素，而不需要了解底层集合的具体实现细节。

- **next()** - 返回迭代器的下一个元素，并将迭代器的指针移到下一个位置。
- **hasNext()** -  用于判断集合中是否还有下一个元素可以访问。
- **remove()** - 从集合中删除迭代器最后访问的元素（可选操作）。

```markdown
## Iterator 和 ListIterator 的区别
Iterator可以迭代所有集合;ListIterator只能用于List及其子类
ListIterator有add方法，可以向List中添加对象;Iterator不能
ListIterator有hasPrevious()和previous()方法，可以实现逆向遍历;Iterator不可以
ListIterator有 nextIndex() 和previousIndex()方法，可定位当前索引的位置;Iterator不可以
ListIterator有set()方法，可以实现对List的修改;Iterator仅能遍历，不能修改。
```

#### 24.写时复制

Copy On Write（写时复制）是一种延迟复制的技术，用于在多个线程（或进程）之间共享资源时减少内存复制成本的。它的基本思想是，在创建拷贝或修改资源之前，不会真正的进行复制操作（懒汉模式），而是共享同一份资源的只读副本，直到某个线程（或进程）试图修改资源时，才会对资源进行复制操作。 这种延迟复制的策略可以减少资源复制的开销，一定程度提高了性能和效率。

CopyOnWrite容器即写时复制的容器。通俗的理解是当我们往一个容器添加元素的时候，不直接往当前容器添加，而是先将当前容器进行Copy，复制出一个新的容器，然后新的容器里添加元素，添加完元素之后，再将原容器的引用指向新的容器。

注意：使用批量添加。因为每次添加，容器每次都会进行复制，所以减少添加次数，可以减少容器的复制次数。如使用上面代码里的addBlackList方法。

缺点：内存占用、一致性差

应用：比如白名单，黑名单，商品类目的访问和更新场景，每天晚上更新一次。当用户搜索时，会产生大量读操作检查当前关键字。

#### 25.Java中是否可以覆盖一个private或者是static的方法？

重写，是子类对父类总的方法进行重新编写，即外壳不变，核心重写，要求子类和父类方法的返回值、方法名、参数值、参数类型必须一致。
private方法是类的私有方法，只能类的内部被访问，不参与继承，所以不能被重写。
static方法，不能被重写分为两方面原因：一方面重写是为了实现子类实例的特殊化，但静态方法属于类本身的，不属于实例，所以不能被重写；另一方面重写的前提是实例调用方法，由其实际类型所决定，是***动态绑定***，但static方法的调用却是由声明类型所决定，是***静态绑定***，所以不能被重写。

#### 26.常见的 native 方法

```java
public class Thread implements Runnable {
	public static native Thread currentThread();
	public static native void yield();
	public static native void sleep(long millis) throws InterruptedException;
	private native void start0();
	private native boolean isInterrupted(boolean ClearInterrupted);
    /* Some private helper methods */
    private native void setPriority0(int newPriority);
    private native void stop0(Object o);
    private native void suspend0();
    private native void resume0();
    private native void interrupt0();
    private native void setNativeName(String name);
}
public class Object {
	public final native Class<?> getClass();
	public native int hashCode();
	public final native void notify();
	public final native void notifyAll();
	public final native void wait(long timeoutMillis) throws InterruptedException;
}
```

#### 27.单分派和双分派

Java是一种支持双分派的单分派语言。分派是指确定要执行哪个对象的哪个方法。可分为单分派、双分派和多分派，其中Java不支持多分派。
```
单分派
定义：调用哪个对象（多态）的方法，在运行期确定，调用对象的哪个方法，编译期确定。
单分派根据方法参数的静态类型发生，方法重载就是单分派
```

双分派

定义：调用哪个对象的方法，在运行期（多态）确定；调用对象的哪个方法（方法重载），在运行期确定。

发生在运行时期，动态分派动态地置换掉某个方法。Java通过方法的重写支持动态分派。也就是说即使是方法重载也可以确定要调用的方法，而不是根据参数的静态类型来调用。

如果要实现双分派的机制那么就要用到访问者模式。访问者模式属于行为型设计模式。

#### 28.如何实现并发读写？

①使用分段锁：只对要操作的部分进行加锁

②使用读写锁：写锁是互斥的，只要写锁没被占用，读锁就可以被获取

③缓存系统：如Redis内存中的键值存储，它们固有地支持高速且大规模的并发读取操作。

#### 29.Java如何解决线程之间的冲突？

Synchronized同步代码块和同步方法

Lock类更灵活的控制——可公平，可中断

原子类确保不会冲突——AutomicInteger、AutomicLong

ThreadLocal关键字让线程创建自己的副本：使用 `ThreadLocal` 变量通常是为了避免同步问题，而不是为了共享数据，因此在大多数使用场景中，并不需要考虑不同线程之间的变量副本的一致性问题。

java.util.concurrent包下的高级并发工具

#### 30.Java的String为什么不可变？

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    // 字节数组，用于存储字符串数据。
    private final byte[] value;

    // 编码标志，表示使用的编码方式
    private final byte coder;

    // Hash码缓存，仍然存在
    private int hash;

    // 字符串类是不可变的，所以它可以被安全地共享。
    private static final long serialVersionUID = -6849794470754667710L;

    // ...其他方法和构造器...
}
```

`String` 是基于字符数组(`char[]`)实现的，这个数组以及`String`对象本身都是私有的且final的，并且没有提供任何修改这个数组内容的方法

1. **安全性**：
   - `String` 被广泛用作参数传递给各种Java类库和应用程序，比如网络连接、打开文件等。如果`String`可变，它的值可能在传递过程中被改变，导致安全漏洞。
   - 不可变对象可以安全地共享，因为它们的状态无法被其它对象或线程改变。
2. **线程安全**：
   - 由于`String`对象不可变，它天生是线程安全的，因为Java内存模型保证了不可变对象在构造后状态不会改变。
   - 这意味着字符串可以在多个线程之间安全地共享而无需额外同步。
3. **性能优化：字符串常量池**：
   - Java虚拟机(JVM)内部维护了一个特殊的存储区域称为“字符串常量池”。当编译器遇到字符串字面量时，JVM会首先检查常量池是否已经包含相同值的字符串对象，如果存在，则直接返回引用；否则，新的字符串对象被创建并放入池中。这种方法节省了内存，因为相同内容的字符串对象可以共享。
   - 如果`String`是可变的，那么字符串常量池就不能实现，因为对象的改变可能影响到使用相同引用的其他地方。
4. **哈希值缓存**：
   - 不可变性使得`String`对象在创建时可以计算其哈希码并缓存。由于`String`内容不会改变，所以它的哈希码也永远不会改变。
   - 这使得`String`非常适合作为键值对中的键，如在`HashMap`或`HashSet`中，因为不需要每次都重新计算哈希值。

#### 31.子类重写问题

- 子类不可以重写final方法，final的意义就是不被改变，确保该行为不会被改变，同理final类也不能被继承

- 子类不可以重写private方法，因为private对类外部不可见，子类只是重新实现了一个新方法

- 子类不可以重写static方法，因为static是和类绑定的，子类重写后只是在子类中用新方法替换了原来的方法

  综上，final、private、static修饰的方法都是静态绑定的，因为它们通过限制类的继承关系，减少了动态分派的需要，静态绑定可以提高性能，因为方法调用在编译时就已经确定了，无需在运行时再做查找。

### 二、Java多线程

#### 线程池

##### 线程池原理

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/c83d70cf3bc79f3d57910653b60ae21d738b299c.jpeg)

创建方式：newFixedThreadPool (固定数目线程的线程池)、newCachedThreadPool(可缓存线程的线程池)、newSingleThreadExecutor(单线程的线程池)、newScheduledThreadPool(定时及周期执行的线程池)、new ThreadPoolExecutor() （自定义的方式创建）

```markdown
## 线程池七大参数
- corePoolSize（核心线程数）
线程池当中线程数最基本上的数量：只有当工作任务队列满了才会有新的线程被创建出来，此时线程数才会大于该值

- maximumPoolSize（最大线程数）
线程池中允许的最大线程数：当前任务队列满了并且小于该值的时候线程才会被创建，否则交给拒绝策略

- 最大线程的存活时间：如果当前线程空闲且线程数量大于核心数则线程销毁的超时时间

- unit 时间单位

- 阻塞队列：当核心线程满后，后面来的任务都进入阻塞队列

- 线程工厂：用于生产线程

- 任务拒绝策略：阻塞队列满后，拒绝任务，有四种策略（1）抛异常（2）丢弃任务不抛异常（3）打回任务（4）尝试与最老的线程竞争

## 线程池的好处
1⃣️降低资源消耗
2⃣️提高响应速度
3⃣️使线程便于管理
```

##### 线程提交后的执行过程

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/060828381f30e924a34e6fad47a3410a1f95f7ff.jpeg)

a.如果正在运行的线程数量小于 corePoolSize，那么马上创建线程运行这个任务!
b.如果正在运行的线程数量大于或等于 corePoolSize，那么将这个任务放入队列。
c.如果这时候队列满了，而且正在运行的线程数量小于 maximumPoolSize，那么还是要创建线程运行这个任务;
d.如果队列满了，而且正在运行的线程数量大于或等于 maximumPoolSize，那么线程池会抛出异常，告诉调用者"我不能再接受任务了"

##### 五种阻塞队列

- ArrayBlockingQueue（有界队列）是一个用数组实现的有界阻塞队列，按FIFO排序量。
- LinkedBlockingQueue（可设置容量队列）基于链表结构的阻塞队列，按FIFO排序任务，容量可以选择进行设置，不设置的话，将是一个无边界的阻塞队列，最大长度为Integer.MAX_VALUE，吞吐量通常要高于ArrayBlockingQuene；newFixedThreadPool线程池使用了这个队列
- DelayQueue（延迟队列）是一个任务定时周期的延迟执行的队列。根据指定的执行时间从小到大排序，否则根据插入到队列的先后排序。newScheduledThreadPool线程池使用了这个队列。
- PriorityBlockingQueue（优先级队列）是具有优先级的无界阻塞队列；
- SynchronousQueue（同步队列）一个不存储元素的阻塞队列，每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于LinkedBlockingQuene，newCachedThreadPool线程池使用了这个队列。

##### 线程创建方式

- (1)继承 Tread 类（拥有run方法和start方法）
- (2)实现 Runnable 接口（只有一个run方法）
- (3)实现 Callable 接口：拥有一个call方法，带有返回值（在Runnable中，我们无法对run()方法抛出的异常进行任何处理，因为其没有返回值，但在Callable中，自定义的call()方法可以抛出一个checked Exception，并由其执行者Handler进行捕获并处理。）

run和start方法的区别？

```markdown
1. 定义位置不同
run()方法是Thread类中的一个普通方法，它是线程中实际运行的代码，线程的代码逻辑主要就是在run()方法中实现的。
start()方法是Thread类中的一个启动方法，它会启动一个新的线程，并在新的线程中调用run()方法。

2. 执行方式不同
直接调用run()方法，会像普通方法一样在当前线程中顺序执行run()方法的内容，这并不会启动一个新的线程。
调用start()方法会创建一个新的线程，并在新的线程中并行执行run()方法的内容。

3. 线程状态不同
当我们调用start()方法启动一个新线程时，该线程会进入就绪状态，等待JVM调度它和其他线程的执行顺序。而当我们直接调用run()方法时，则会在当前线程中执行，不会产生新的线程。
```

##### 线程池的五种状态

```markdown
1. RUNNING：线程池一旦被创建，就处于 RUNNING 状态，任务数为 0，能够接收新任务，对已排队的任务进行处理。
2. SHUTDOWN：不接收新任务，但能处理已排队的任务。调用线程池的 shutdown() 方法，线程池由 RUNNING 转变为 SHUTDOWN 状态。
3. STOP：不接收新任务，不处理已排队的任务，并且会中断正在处理的任务。调用线程池的 shutdownNow() 方法，线程池由(RUNNING 或 SHUTDOWN ) 转变为 STOP 状态。
4. TIDYING：
    SHUTDOWN 状态下，任务数为 0， 其他所有任务已终止，线程池会变为 TIDYING 状态，会执行 terminated() 方法。线程池中的 terminated() 方法是空实现，可以重写该方法进行相应的处理。
    线程池在 SHUTDOWN 状态，任务队列为空且执行中任务为空，线程池就会由 SHUTDOWN 转变为 TIDYING 状态。
    线程池在 STOP 状态，线程池中执行中任务为空时，就会由 STOP 转变为 TIDYING 状态。
5. TERMINATED：线程池彻底终止。线程池在 TIDYING 状态执行完 terminated() 方法就会由 TIDYING 转变为 TERMINATED 状态。
```

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/1408183-20191016162255593-1404242897.jpg)

线程池execute和submit方法的区别？

- 方法返回不同： execute(Runnable command) 接受一个Runnable类型的参数，而 submit(Runnable task) 接受一个Runnable或者Callable类型的参数，因此 submit() 方法可以返回一个结果，如果任务在执行过程中抛出异常， execute()方法不会显示抛出异常，而是将其捕获并记录，而 submit()方法则会将异常包装在Future对象中返回。
- 阻塞行为不同：execute()方法一旦提交任务就立即返回，无法阻塞；而 submit()方法可以选择传递一个超时时间作为参数，如果在指定时间内任务没有完成，则取消任务并抛出异常。

```java
import java.util.concurrent.*;
public class ThreadPoolExample {
    public static void main(String[] args) {
        // 创建线程工厂
        ThreadFactory threadFactory = Executors.defaultThreadFactory();
        // 创建拒绝策略
        RejectedExecutionHandler rejectedExecutionHandler = new ThreadPoolExecutor.AbortPolicy();
        // 创建线程池，设置参数
        int corePoolSize = 5;
        int maxPoolSize = 10;
        long keepAliveTime = 60; // 线程空闲时间
        TimeUnit unit = TimeUnit.SECONDS; // 时间单位
        int queueCapacity = 100; // 任务队列大小
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                corePoolSize,
                maxPoolSize,
                keepAliveTime,
                unit,
                new LinkedBlockingQueue<>(queueCapacity),
                threadFactory,
                rejectedExecutionHandler
        );
        // 提交任务给线程池
        for (int i = 0; i < 10; i++) {
            final int taskId = i; // 任务ID（仅用于示例）
            executor.execute(new Runnable() {
                public void run() {
                    System.out.println("Task " + taskId + " is executing by " +
                            Thread.currentThread().getName());
                    // 执行任务的具体逻辑
                    // ...
                }
            });
        }
        // 关闭线程池
        executor.shutdown();
    }
}
```

##### 守护线程

守护线程（daemon thread）是在计算机程序中运行的一种特殊线程。它的主要特点是当所有非守护线程结束时，守护线程会自动退出，而不会等待任务的完成。

守护线程通常被用于执行一些后台任务，如垃圾回收、日志记录等。它们在程序运行过程中默默地执行任务，不会阻塞主线程或其他非守护线程的执行。

与普通线程不同，守护线程的生命周期并不影响整个程序的生命周期。当所有非守护线程结束时，守护线程会被强制退出，无论它的任务是否完成。

需要注意的是，守护线程不能用于执行一些重要的任务，因为它们可能随时被强制退出。此外，守护线程也无法捕获或处理异常。

```java
thread1.setDaemon(true); //设置守护线程
```

##### 线程结束

结束线程有以下三种方法： （1）设置退出标志，使线程正常退出。 （2）使用interrupt()方法中断线程。 （3）使用stop方法强行终止线程（不推荐使用Thread.stop, 这种终止线程运行的方法已经被废弃，使用它们是极端不安全的！）

##### 线程优先级

线程分为1-10级，其中10最高，默认值为5，优先级的高低不代表线程优先执行，JVM不一定采纳，需要看CPU的情况，一般情况下优先级高的先执行。

##### 多线程并发的3个特性

- 原子性 ：即一个操作或者多个操作 要么全部执行并且执行的过程不会被任何因素打断，要 么就都不执行（使用synchronized或者Lock悲观锁，、AtomicXXX原子操作）
- 可见性：当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即 看得到修改的值 （volitale、synchronized）
- 有序性：程序执行的顺序按照代码的先后顺序执行，（单线程时，为了提高执行效率，程序在编译或者运行的时候会对代码进行指令重排，保证最终一致性，指令重排并不会产生问题，但是在多线程的情况下便可能产生不希望看到的结果。使用volatile修饰的内存，不可以重排序，对volatile修饰变量的读写访问，都不可以换顺序）

##### ThreadLocal关键字

![图片.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/353e79a598324ca99b08a82c35c7a4c4~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp)

- 主要用于线程本地化存储，即只有本线程才能对该变量进行查看或者修改
- 使用时需要注意，在不使用该变量的时候，一定要调用`remove`方法删除变量，否则可能会造成内存泄露的问题
- ThreadLocal 是由 ThreadLocalMap 实现
-  k 为弱引用，在没有强引用的情况下，经可达性分析算法发现当前对象不可达，经一次 GC 之后 k 就为 null

```markdown
## Thread、ThreadLocal、ThreadLocalMap的关系
Thread 与 ThreadLocalMap 是 has a 的关系。初始时，Thread 中的 threadLocals 为空，只有在当前线程中创建了 ThreadLocal 变量并且设置了变量值，才会创建 ThreadLocalMap 实例。
    ThreadLocal.ThreadLocalMap threadLocals = null;
ThreadLocal 类中定义了静态内部类 ThreadLocalMap，把它当作伪 Map 即可，就是存储key-value 键值对
ThreadLocalMap 类中又定义了 Entry 静态内部类，该类定义了一个 Entry 类型的数组 table。为什么要自定义一个 Entry 呢，因为现有不满足要定制，Entry 的 k 为 ThreaLocal 实例，v 为变量值
```

```java
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

public class ThreadLocalExample {
    // 创建一个 ThreadLocal 实例，并提供一个初始化方法
    private static final ThreadLocal<DateFormat> dateFormatThreadLocal = ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"));

    public static void main(String[] args) {
        // 线程 1 使用 ThreadLocal
        Thread thread1 = new Thread(ThreadLocalExample::printCurrentDate, "Thread-1");
        // 线程 2 使用 ThreadLocal
        Thread thread2 = new Thread(ThreadLocalExample::printCurrentDate, "Thread-2");

        thread1.start();
        thread2.start();
    }

    // 这个方法将打印当前日期和时间
    public static void printCurrentDate() {
        DateFormat formatter = dateFormatThreadLocal.get();
        String formattedDate = formatter.format(new Date());
        System.out.println(Thread.currentThread().getName() + " : " + formattedDate);
        // 很关键一步：当不再需要使用 ThreadLocal 存储的对象时，
        // 要调用 remove，防止内存泄漏，特别是在使用线程池的场合。
        dateFormatThreadLocal.remove();
    }
}
```



使用注意：

![图片.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/65a291903a04401cbdfd2f17422f493e~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp)

##### volatile关键字

```markdown
为什么需要？
1. 缓存一致性问题：现代多核处理器普遍采用缓存系统来提高处理速度，每个核心可能有自己的本地缓存。如果线程A更新了一个变量但只是写入了本地缓存，其他处理器的缓存中该变量的副本就会过时，而线程B如果在另一个核心执行并且读取了同一个变量，就可能读到旧值。

2. 编译器优化：为了提高程序效率，编译器可能会重新排序代码执行顺序，使得编写时序列上相邻的操作在实际执行时并不相邻。

3. 处理器优化：处理器也可能为了提高程序执行效率而做出指令重排。
```

1. 可见性：使用volatile关键字会强制将修改的值立即写入主存；

原理：

```markdown
## 内存屏障（Memory Barriers）：
当编写一个volatile变量时，会在写操作后插入一种称为“存储屏障”（Store Barrier）的CPU指令，当读取一个volatile变量时，会在读操作前插入另一种称为“加载屏障”（Load Barrier）的CPU指令。这些屏障告诉CPU不要执行那些可能会影响到这些操作的重排序，并确保在屏障之前的所有操作都在屏障之后的操作之前完成。

## 禁止缓存行优化：
声明为volatile的变量不能被缓存行优化，这样就避免了线程在自己的工作内存中拥有变量的私有副本，每次访问变量时都是直接对主内存进行操作。这意味着对volatile变量的修改能够立即更新到主内存中，而且读取操作都是直接从主内存读取的，因此保证了不同线程看到该变量的值是一致的。
```

```java
A = 1;      // 1号操作
B = 2;      // 2号操作
V = A + B;  // 3号操作，其中V是volatile变量
// 写入V的操作（3号操作）发生在对A和B读写的操作（1号和2号操作）之后，这意味着这些操作不会被重排到写入V之后。这样读取V的线程就能看到A和B的更新结果。
```

2. 确保有序性：利用volatile的变量保证线程的执行顺序

```java
volatile boolean inited = false;
//线程1:
context = loadContext(); 
inited = true; 
//线程2:
while(!inited ){
	sleep()
}
doSomethingwithconfig(context);
```

```markdown
## 问题：volatile 能够保证线程安全问题吗？为什么？
不能，volatile 只能保证可见性和顺序性，不能保证原子性。例如，自增操作count++（包含读取-修改-写入三个步骤）即使在volatile变量上进行，也不是原子操作，因此不能保证线程安全。
```

#### 线程状态

Java线程具有以下几种基本状态：

1. NEW（新建）：线程刚刚被创建，但尚未调用 `start()` 方法以开始其执行。此时，线程已分配内存空间并已完成初始化，但它还没有开始执行代码。
2. RUNNABLE（可运行）：线程已被调用 `start()` 方法，正处于就绪状态，它已经准备好运行。
3. BLOCKED（阻塞）：线程因等待某个锁、被其他线程调用 `wait()` 方法、等待 I/O 操作完成等原因而暂时无法继续执行。
4. TIMED_WAITING（计时等待）：与普通等待状态相似，但在计时等待状态下，如果等待时间超过设定的超时时间，线程将自动转换为就绪状态。这通常发生在使用了 `wait()` 方法且设置了超时参数的情况下。(**无限等待状态**(植物人状态):线程调用了wait()方法并且没有传参数 → 自己醒不过来,只能调用notify()方法来唤醒无限等待状态)
5. TERMINATED（终止）：线程完成了所有预定的任务并且已经结束了。这种情况下，线程不再存在于系统中，不能被再次激活。(run()方法执行结束,或者调用了stop(),或者发生了异常,线程就死亡了.)

此外，还有两种特殊状态：

- WAITING（等待）：线程正在等待其他线程的通知以便它可以继续执行。这可能是因为等待 `notifyAll()` 或 `join()` 方法的执行结果。
- DEAD（死亡）：线程的 `run()` 方法已经执行完毕，或者被中断或异常退出，导致线程进入死亡状态。在这种状态下，线程不能再作为独立执行的线程对待。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/069fbe7bdd864e6fad4854ab620f7178.png)

```java
/*顾客去买包子,包子可能没有要现做,
但是不知道要多久才能做好,因为不知道顾客要等待的时间,所以调用了wait()方法,此时顾客属于无限等待状态
包子做好了,老板通知顾客包子好了,notify()一下*/
public class WaitAndNotify {
    public static void main(String[] args) {
        //锁对象必须是唯一的
        final Object obj = new Object();
        //创建一个顾客线程
        new Thread(){
            @Override
            public void run() {
                while (true){
 
                    //老板和顾客线程要用同步代码块包裹,保证只能实行其中的一个
                    synchronized (obj){
                        System.out.println("顾客说要1个素馅的1个肉馅的包子");
                        //无限等待
                        try {
                            obj.wait();
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        //唤醒之后的代码
                        System.out.println("包子已经做好了,开吃");
                        System.out.println("______________________________");
 
                    }
                }
            }
        }.start();
        //创建一个老板线程
        new Thread(){
            @Override
            public void run() {
                while(true){
                    //花了3S做包子
                    try {
                        Thread.sleep(3000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    //老板和顾客线程要用同步代码块包裹,保证只能实行其中的一个
                    synchronized (obj){
                        System.out.println("老板花3S做好了包子");
                        obj.notify();
                    }
 
                }
            }
        }.start();
    }
}
```

##### wait、notify和notifyAll的作用

1. wait方法

    wait方法就是使当前线程等待该对象的锁，当前线程必须是该对象锁的拥有者，wait()方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，如果在规定时间内没有获得锁就返回
    调用该方法后当前线程进入睡眠状态，直到以下事件发生
    
    1. **超时**：如果指定的等待时间到了，那么线程将自动醒来并尝试重新获取锁。一旦获取了锁，从 `wait()` 方法返回后，线程将继续执行。
    2. **通知**：如果其他线程在相同的对象上调用了 `notify()` 或 `notifyAll()`，则等待的线程中的某一个或全部将被唤醒。这些线程之后也必须重新获取锁才能继续执行。
    3. **中断**：如果其他线程中断了正在等待的线程，它将抛出 `InterruptedException` 并从 `wait()` 调用中提前返回。
    
2. notify方法

    该方法唤醒在该对象上等待的某个线程

3. notifyAll方法

    该方法唤醒在该对象上等待的所有线程
##### notify和notifyAll的区别？

notify()是随机唤醒一个线程,notifyAll()唤醒所有线程，都是让线程变为就绪状态

滥用 `notifyAll()` 可能导致性能问题，因为多个线程会被唤醒然后竞争锁；但是不正确地使用 `notify()` 可能导致某些线程永远得不到唤醒，特别是在等待条件不止一个时

##### sleep和wait的区别？

- 语法和使用：`sleep` 直接作用于 `Thread` 类，不需要与 `synchronized` 关键字一起使用。 `wait` 则是 `Object` 类的方法，通常需要配合 `synchronized` 使用以确保正确性。
- 唤醒机制：`sleep` 会自动在指定的时间后唤醒线程，, `wait` 不一定需要传递超时时间参数。如果不传递任何参数，表示永久休眠；若传递超时时间，则在超时后唤醒。wait()要调用notify()或notifyall()唤醒，sleep()自动唤醒
- 锁释放：`sleep` 不会释放任何锁资源。 `wait` 会释放所持锁资源，以便其他线程能够访问同步控制块或方法。
- 使用：`sleep` 通常用于使整个应用程序暂停执行，而不是特定的同步控制块。 `wait` 更适合于在特定同步控制块内部暂停线程，以便其他线程有机会处理该控制块的资源。

#### 线程安全

##### 概念

指某个方法、函数或者类在多线程环境下被同时访问时，依然能够表现出正确的行为。需要满足以下条件：

1. 原子性 (Atomicity)

   原子性意味着一系列操作要么完全执行，要么完全不执行。在多线程环境中，一个被认为是原子的操作在执行过程中不会被其他线程中断。

2. 可见性 (Visibility)

   可见性确保当一个线程修改了共享变量的值时，这个更改对于其他线程是可见的。在没有适当同步的情况下，由于缓存和编译器优化等原因，一个线程中的改动可能对其他线程不可见。

3. 有序性 (Ordering)

   有序性意味着程序执行的顺序按照代码的先后顺序来进行，以防止“指令重排”影响到多线程之间的交互。

如果java线程在执行过程中遇到了未捕获的异常，如何处理：

1. **异常传播**：如果异常没有在其发生点被捕捉处理，它会向上通过调用栈传播，直到找到相应的异常处理器。
2. **查找默认的异常处理器**：如果整个调用栈中都没有合适的处理器来处理这个异常，那么 Java 虚拟机 (JVM) 将会寻找线程的默认未捕获异常处理器。
3. **执行异常处理器**：如果存在默认的未捕获异常处理器（可以由应用程序通过 `Thread.setDefaultUncaughtExceptionHandler` 方法全局地设置，或者通过 `Thread.setUncaughtExceptionHandler` 方法为特定线程设置），它将被调用，并将当前线程和异常作为参数传递。处理器可以记录日志、输出错误信息到控制台、发送错误报告、执行清理操作等。
4. **线程终止**：在未捕获异常处理器处理完异常之后，该线程将停止执行并终止。如果这个线程是 JVM 内的唯一用户线程，那么 JVM 通常会随之关闭。如果还有其他用户线程或守护线程在运行，JVM 将继续执行这些线程。

##### 优先级

Java线程优先级 取值范围通常是从 `Thread.MIN_PRIORITY` (1) 到 `Thread.MAX_PRIORITY` (10)，标准优先级是 `Thread.NORM_PRIORITY` (5)可以影响线程获取 CPU 时间片的频率

即使在 Java 中设置了线程的优先级，具体如何影响线程调度仍然取决于操作系统的具体实现，因为 Java 应用中的线程优先级最终会映射到宿主操作系统的线程优先级上。大多数现代操作系统都会尽力根据这些优先级来进行调度，但并不一定保证总是严格按照 Java 程序设定的优先级来执行。

原则：应该编写不依赖于线程调度或执行顺序的代码，因为这样做会导致程序的行为变得不可预测，而是使用适当的同步原语（如锁、信号量、屏障等）来控制线程间的交互和协作。

##### 与操作系统线程的区别

Java 线程是一个高级抽象，让 Java 程序员能够编写出可在多种操作系统上运行的线程代码，而无需关心每种操作系统具体的线程实现和调度机制。操作系统线程是 Java 线程最终映射到的**底层实体**。

```
对于开发者来说，直接使用操作系统线程意味着需要处理更多的平台相关性问题。
例如 Windows 上的 Win32 线程、Linux 上的 POSIX 线程（pthreads）。
```

- 是通过 Java 的 `Thread` 类来表示和控制的。可以通过创建 `Thread` 类的实例或实现 `Runnable` 接口来定义线程的行为。
- 提供了一组丰富的 API 来管理线程的生命周期，如启动 (`start()` 方法)、中断 (`interrupt()` 方法)、设置优先级 (`setPriority()` 方法)等。
- 在 JVM 上运行，其线程调度由 JVM 处理，可能受到底层操作系统线程调度策略的影响。

#### JAVA的锁

##### CAS锁（Compare and Swap）

CAS是一种无锁算法 （乐观锁），本质是利用了cpu对原子操作的支持，CAS有3个操作数，内存值V，旧的预期值A，要修改的新值B。当且仅当预期值A和内存值V相同时，将内存值V修改为B，否则什么都不做。

1. 首先，每个线程都会先获取当前的值。接着走一个原子的CAS操作，原子的意思就是这个CAS操作一定是自己完整执行完的，不会被别人打断。
2. 然后CAS操作里，会比较一下，现在你的值是不是刚才我获取到的那个值。如果是，说明没人改过这个值，那你给我设置成累加1之后的一个值。
3. 同理，如果有人在执行CAS的时候，发现自己之前获取的值跟当前的值不一样，会导致CAS失败，失败之后，进入一个无限循环，再次获取值，接着执行CAS操作。

![1583024085533.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/216fe7729e6745d4907861bd43795cff~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp)

```
缺点：
1.可能cas 会一直失败，然后自旋
2.如果一个值原来是A，变成了B，然后又变成了A，那么在CAS检查的 时候会发现没有改变，但是实质上它已经发生了改变，这就是所谓的ABA问题。 对于ABA问题其解决方案是加上版本号，即在每个变量都加上一个版本号，每次 改变时加1，即A —> B —> A，变成1A —> 2B —> 3A。
3.CAS操作可以有效的减少使用锁所带来的开销，但是这是使用cpu资源做交换的
```

##### AQS同步机制（AbstractQueuedSynchronizer）

AQS的核心思想是：通过一个**volatile修饰的int**属性state代表同步状态，例如0是无锁状态，1是上锁状态。多线程竞争资源时，通过**CAS的方式来修改state**，例如从0修改为1，修改成功的线程即为资源竞争成功的线程，将其设为exclusiveOwnerThread，也称【工作线程】，资源竞争失败的线程会被放入一个**FIFO的队列**中并挂起休眠，当exclusiveOwnerThread线程释放资源后，会从队列中唤醒线程继续工作，循环往复。

#### Synchronized 锁

在Java中，synchronized是一种关键字，用于实现线程同步。它可以用于方法或代码块，用于保证同一时间只有一个线程可以执行被synchronized修饰的代码。

synchronized的锁机制有两种使用方式：

1. 同步方法：可以在方法声明中使用synchronized关键字。当一个线程调用同步方法时，会自动获取该方法所属对象的锁，其他线程将被阻塞，直到该线程释放锁为止。
2. 同步代码块：使用synchronized关键字可以修饰一段代码块。当一个线程进入synchronized代码块时，它会尝试获取锁，如果锁已经被其他线程获取，那么该线程将被阻塞，直到锁被释放。

```markdown
synchronized锁是基于对象的，每个对象都有一个关联的锁。当多个线程同时访问某个对象的同步方法或同步代码块时，它们会竞争该对象的锁。
    1.如果synchronized锁加在实例方法上，则默认使用的是this锁
    2.如果synchronized锁加载静态方法上，则默认使用的是  类名.class  锁（Java反射技术中说到一个class文件只会在jvm中存在一份）

另外，要注意避免过多地使用synchronized，因为过多的同步操作可能会导致性能下降。在某些情况下，可以考虑使用更灵活的并发工具，如Lock和Condition接口
```

对象头：

![835de3c09bc24730bf761524461dd19a](https://gitee.com/xu_zuyun/picgo/raw/master/img/835de3c09bc24730bf761524461dd19a.png)

自旋：**线程会一直循环检查该锁是否被释放，直到获取到该锁为止。这个循环等待的过程被称为自旋**

1）偏向锁

只有一个线程争抢锁资源的时候.将线程拥有者标识为当前线程。引入了偏向锁目的是来**尽可能减少无竞争情况下的同步操作开销**。当一个线程访问同步块并获取对象的锁时，会将锁的标记记录在线程的栈帧中，并将对象头中的Thread ID设置为当前线程的ID。此后，当这个线程再次请求相同对象的锁时，虚拟机会使用已经记录的锁标记，而不需要再次进入同步块。

```
偏向锁（Biased Locking）就是为了在无竞争的情况下减少同步操作的开销。它通过记录线程ID来避免对锁的加锁和解锁操作，提高了单线程访问同步代码块时的性能。
```

2）轻量级锁（自旋锁）

一个或多个线程通过CAS去争抢锁,如果抢不到则一直自旋。虚拟机会将对象的Mark Word复制到线程的栈帧中作为锁记录，并尝试使用CAS操作尝试获取锁。如果CAS成功，则表示线程获取了轻量级锁，并继续执行同步块。如果CAS失败，说明有竞争，虚拟机会通过自旋（spinning）等待其他线程释放锁

```
轻量级锁是为了减少线程切换的开销。它使用CAS（Compare and Set）操作来尝试获取锁，如果成功则可以继续执行同步块，无需线程切换；如果失败，则会进行自旋操作等待锁的释放。自旋操作避免了线程挂起和切换的开销，提高了多线程竞争时的性能。
使用对象头中的一部分位来存储线程ID和锁标记，不需要额外的内存存储锁的状态。相对于传统的重量级锁，它能够节省内存消耗。
```

3）重量级锁

如果自旋等待不成功，虚拟机会将轻量级锁升级为重量级锁。在这种状态下，虚拟机会将线程阻塞，并使用操作系统的互斥量来实现锁的释放和获取。

```
每个重量级锁都关联着一个操作系统层面的互斥量。当一个线程请求一个已经被其它线程持有的重量级锁时，请求的线程会被阻塞。操作系统负责挂起线程和在锁被释放时唤醒线程，这涉及到线程从用户态到核心态的切换，因此开销比轻量级锁大得多，故名为“重量级”。

使用重量级锁，当锁被释放时，所有等待这个锁的线程都会被唤醒，并在操作系统层面上进行竞争，最终只有一个线程能够获取锁，其余线程则回到阻塞状态。
```

需要注意的是，锁的升级是逐级升级的过程，而不会存在降级。换句话说，一旦锁升级到更高级别，就不会回到低级别。

##### 升级过程：

- 1）当只有一个线程去争抢锁的时候,会先使用偏向锁,就是给一个标识,说明现在这个锁被线程a占有.
- 2）后来又来了线程b,线程c,说凭什么你占有锁,需要公平的竞争,于是将标识去掉,也就是撤销偏向锁,升级为轻量级锁,三个线程通过CAS自旋进行锁的争抢(其实这个抢锁过程还是偏向于原来的持有偏向锁的线程).
- 3）现在线程a占有了锁,线程b,线程c一直在循环尝试获取锁,后来又来了十个线程,一直在自旋,那这样等着也是干耗费CPU资源,所以就将锁升级为重量级锁,向内核申请资源,直接将等待的线程进行阻塞.

##### 优化方法

```markdown
## 1、适应性自旋
解决这个问题最简单的办法就是指定自旋的次数，例如让其循环10次，如果还没获取到锁就进入阻塞状态。但是JDK采用了更聪明的方式——适应性自旋，简单来说就是线程如果自旋成功了，则下次自旋的次数会更多，如果自旋失败了，则自旋的次数就会减少。
## 2、锁粗化（Lock Coarsening）
锁粗化的概念应该比较好理解，就是将多次连接在一起的加锁、解锁操作合并为一次，将多个连续的锁扩展成一个范围更大的锁。
          stringBuffer.append("a");
          stringBuffer.append("b");
          stringBuffer.append("c");
这里每次调用stringBuffer.append方法都需要加锁和解锁，如果虚拟机检测到有一系列连串的对同一个对象加锁和解锁操作，就会将其合并成一次范围更大的加锁和解锁操作，即在第一次append方法时进行加锁，最后一次append方法结束后进行解锁。
## 3、锁消除
锁消除即删除不必要的加锁操作。根据代码逃逸技术，如果判断到一段代码中，堆上的数据不会逃逸出当前线程，那么可以认为这段代码是线程安全的，不必要加锁。看下面这段程序：
public static void main(String[] args) {
          SynchronizedTest02 test02 = new SynchronizedTest02();
          //启动预热
          for (int i = 0; i < 10000; i++) {
              i++;
         }
         long start = System.currentTimeMillis();
         for (int i = 0; i < 100000000; i++) {
             test02.append("abc", "def");
         }
虽然StringBuffer的append是一个同步方法，但是这段程序中的StringBuffer属于一个局部变量，并且不会从该方法中逃逸出去，所以其实这过程是线程安全的，可以将锁消除。
```

##### 对比

| 锁       | 优点                                                         | 缺点                                             | 应用场景                              |
| -------- | ------------------------------------------------------------ | ------------------------------------------------ | ------------------------------------- |
| 偏向锁   | 加锁和解锁不需要额外的消耗，和执行非同步方法比仅存在纳秒级的差距。 | 如果线程间存在锁竞争，会带来额外的锁撤销的消耗。 | 适用于只有一个线程访问同步块场景。    |
| 轻量级锁 | 竞争的线程不会阻塞，提高了程序的响应速度。                   | 如果始终得不到锁竞争的线程使用自旋会消耗CPU。    | 追求响应时间。 同步块执行速度非常快。 |
| 重量级锁 | 线程竞争不使用自旋，不会消耗CPU。                            | 线程阻塞，响应时间缓慢。                         | 追求吞吐量。 同步块执行速度较长。     |

##### 底层实现原理

synchronized 是 JVM 的内置锁，基于 Monitor 机制实现。每一个对象都有一个与之关联的监视器  (Monitor)，这个监视器充当了一种互斥锁的角色。当一个线程想要访问某个对象的 synchronized 代码块，首先需要获取该对象的  Monitor。如果该 Monitor 已经被其他线程持有，则当前线程将会被阻塞，直至 Monitor 变为可用状态。当线程完成  synchronized 块的代码执行后，它会释放 Monitor，并把 Monitor 返还给对象池，这样其他线程才能获取 Monitor  并进入 synchronized 代码块。

```
每个Java对象都有一个与之关联的Monitor。这个Monitor的实现是在JVM的内部完成的，它采用了一些底层的同步原语，用以实现线程间的等待和唤醒机制，这也是为什么等待（wait）和通知（notify）方法是属于Object类的原因。这两个方法实际上是通过操纵与对象关联的Monitor，以完成线程的等待和唤醒操作，从而实现线程之间的同步。
```

#### Synchrpnized和lock的区别

（1）synchronized是关键字，lock是一个类，synchronize是在JVM层面实现的，发生异常后jvm会释放锁。lock是JDK代码实现的，需要手动释放，在finally块中释放，容易死锁。

（2） 用法不一样：synchronize可以用在代码块上，方法上。lock只能写在代码里，不能直接修改方法。synchronized在发生异常时会自动释放锁，lock需要手动释放锁

（3）synchronized是非公平锁（每个线程获取锁的顺序不是按照线程访问锁的先后顺序获取）、可重入锁（一个线程已经持有了某个锁，那么它可以再次请求并获得这个锁而不会被自身所阻塞）不可中断锁（等待获取锁的线程必须等到锁被释放，无法被中途interrupt()），lock是可公平锁、可中断锁、可重入锁。

（4）synchronized适用于少量同步，lock适用于大量同步。

（5）锁状态是否可以判断：synchronized 不可以，lock可以。

```markdown
## Synchronized
优点：实现简单，语义清晰，便于JVM堆栈跟踪，加锁解锁过程由JVM自动控制，提供了多种优化方案，使用更广泛
缺点：悲观的排他锁，不能进行高级功能

## Lock
优点：可定时的、可轮询的与可中断的锁获取操作，提供了读写锁、公平锁和非公平锁　　
缺点：需手动释放锁unlock，不适合JVM进行堆栈跟踪
```

```markdown
## 可重入锁
可重入锁和不可重入锁的最大区别在于，可重入锁允许同一个线程在获得锁之后再次获得该锁，而不可重入锁不允许。
如果一个线程已经获得锁，那么在该线程释放该锁之前，它可以再次获得该锁而不会被阻塞。
    实现原理：
    每次获得锁时，计数器加1，每次释放锁时，计数器减1。只有当计数器为0时，其他线程才有机会获得该锁。

## ReentrantLock (用于替代synchronized)
ReentrantLock提供了可轮询的锁请求。它会尝试着去获取锁，如果成功则继续，否则可以等到下次运行时处理，而synchronized则一旦进入锁请求要么成功要么阻塞，所以相比synchronized而言，ReentrantLock会不容易产生死锁些。

## 公平锁
    按先来后到的顺序获取锁

## 读写锁 ReentrantReadWriteLock （乐观锁）
读写锁维护着一对锁，一个读锁和一个写锁。通过分离读锁和写锁，使得并发性比一般的互斥锁有了较大的提升：在同一时间可以允许多个读线程同时访问，但是在写线程访问时，所有读线程和写线程都会被阻塞。
```

#### 保证多线程的安全性？

1. 使用synchronized关键字：synchronized关键字可以将某些代码块或方法设为同步代码，确保同一时刻只有一个线程可以访问。这种方式需要注意锁的粒度，使得锁住的代码块尽可能的短，以避免影响程序性能。

2. 使用Volatile关键字：Volatile关键字可以用于修饰变量，确保多线程之间的可见性，即当一个线程修改了共享变量的值，其他线程会立即查询最新的值。

3. 使用Lock对象：Lock是JDK提供的同步机制，Lock提供的Lock()和Unlock()方法可以在同一个时刻，只允许一个线程进入执行Lock()和Unlock()方法之间的代码块，其他线程必须等待。

4. 使用原子类：Java提供了很多原子类，包括AtomicInteger、AtomicLong和AtomicBoolean等等，这些类可以保证特定操作的原子性，避免多线程同时访问一个共享资源所造成的数据安全问题。(atomic是通过CAS实现的)

5. 使用ThreadLocal类：ThreadLocal类可以在多线程中为每个线程创建一个独立的实例，避免多线程对同一资源的争夺，从而保证了数据安全性。

#### yield()和join()区别

- yield暂停当前正在执行的线程，并执行其他线程。（可能没有效果），如果成功则调用后线程进入就绪状态，告诉当前线程把机会交给其他高优先级的线程
- join将两个交替执行的线程合并为顺序执行的线程，A线程中调用B线程的join() ,则B执行完前A进入阻塞状态，使并行的程序串行化执行

#### Thread、Runable的区别

Thread和Runnable的实质是继承关系（`Thread`类实现了`Runnable`接口），没有可比性。无论使用Runnable还是Thread，都会new Thread，然后执行run方法。用法上，如果有复杂的线程操作需求，那就选择继承Thread，如果只是简单的执行一个任务，那就实现runnable。

- 通过继承 `Thread` 类，你可以获得对线程行为的更直接控制。直接调用 `interrupt` 或者检查线程状态的方法。

- 可能需要定制化的线程类，可以通过扩展 `Thread` 类来更容易地添加属性和行为。

  

- 实现 `Runnable` 接口允许多个线程共享同一个实例对象的资源，从而更容易实现多个线程之间的资源共享。

- 如果将来想要将代码改为使用 Executor 框架或其他并发工具，实现 `Runnable` 接口会更加方便。

#### java的线程和操作系统的线程有什么区别

- **Java线程**： Java线程是由Java虚拟机（JVM）提供的抽象概念，它们在Java语言级别上通过`java.lang.Thread`类以及相关API来创建和管理。Java线程的行为和生命周期由JVM控制，并且在所有支持Java的平台上表现一致。

- **操作系统线程**： OS线程是操作系统提供的原生线程功能，通常通过低级语言如C或C++使用系统调用直接访问。操作系统负责其线程的创建、调度和管理。OS线程的调度由内核进行

  

- Java线程提供了一组高级API，允许程序员在高层次上控制线程的行为，例如设置线程优先级、状态查询、同步机制等。但是，对底层线程的细粒度控制有限。

- **操作系统线程**： OS线程通常提供更多的底层控制选项，包括线程亲和性设置、更详细的状态信息和调度参数等。这使得在操作系统级别可以进行更细致的性能优化。

### 三、JAVA虚拟机（JVM）

#### JAVA内存模型

![java-runtime-data-areas-jdk1.7](https://gitee.com/xu_zuyun/picgo/raw/master/img/java-runtime-data-areas-jdk1.7.png)

所有变量都存在主存中，主存是线程共享区域；每个线程都有自己独有的工作内存，线程想要操作变量必须从主从中copy变量到自己的工作区，每个线程的工作内存是相互隔离的

```markdown
## 栈帧的压入
1. 局部变量表 主要存放了编译期可知的各种数据类型、对象引用。
2. 操作数栈 主要作为方法调用的中转站使用，用于存放方法执行过程中产生的中间计算结果。另外，计算过程中产生的临时变量也会放在操作数栈中。
- 复杂表达式或多步骤操作
- 当调用方法时，参数会被推送到调用者的操作数栈上，然后传递给被调用的方法。当方法执行结束并返回结果时，返回值会被推送到操作数栈上，供调用者使用。
3. 动态链接 主要服务一个方法需要调用其他方法的场景。当一个方法要调用其他方法，需要将常量池中指向方法的符号引用转化为其在内存地址中的直接引用。动态链接的作用就是为了将符号引用转换为调用方法的直接引用，这个过程也被称为 动态连接
- 一个指向运行时常量池的引用，运行时常量池包含了所有的符号引用，动态链接过程就是在运行时将符号引用转换为直接引用的过程。


## 栈帧的弹出
Java 方法有两种返回方式，一种是 return 语句正常返回，一种是抛出异常。不管哪种返回方式，都会导致栈帧被弹出。也就是说， 栈帧随着方法调用而创建，随着方法结束而销毁。无论方法正常完成还是异常完成都算作方法结束。
```

#### JDK、JRE、JVM的关系

JDK > JRE = Java虚拟机 + Java核心类库

```
为什么jre要有核心类库？
可移植性：只要那个系统上安装了相应版本的 JRE，就能提供一致的运行时环境。
简化部署和分发：不需要打包整个 Java 核心库，可以减少应用程序包的体积。用户只需安装一份 JRE，即运行任何Java程序。
抽象底层实现：核心类库隐藏了与底层系统的直接交互，使得 Java 程序员不需要关注操作系统的具体细节就能进行开发。例如，文件操作和网络通信等都被抽象化
```

```markdown
## JDK: JAVA开发工具包
    bin:最主要的是编译器(javac.exe) 
    include:java和JVM交互用的头文件 
    lib：类库 
    jre:java运行环境
jdk主要面向开发者，具有java的编译功能。
jre主要面向用户，主要是class文件的运行，假如我们只有编译好的class文件和jre，那么就可以运行class了。
```

JVM组成结构：

- （1）类加载器
- （2）运行时数据区
- （3）执行引擎
- （4）本地库接口：与本地系统进行交互，例如图形和数据库操作等服务
- （5）垃圾回收器

#### 类加载器

Java程序运行的时候，编译器将Java文件编译成平台无关的Java字节码文件（.class）,接下来对应平台JVM对字节码文件进行解释，翻译成对应平台匹配的机器指令并运行.

对于任意一个类，都需要由加载它的类加载器和这个类本身一同确立在 JVM 中的唯一性，每一个类加载器，都有一个独立的类名称空间。类加载器就是根据指定全限定名称将 class 文件加载到JVM 内存，然后再转化为 class 对象。

具体过程为：**加载、链接、初始化**

```markdown
## 加载
        将class文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时数据结构(InstanceKlass)，然后生成一个代表这个类的java.lang.Class对象。
## 链接
        验证：确保加载的类信息符合JVM规范，没有安全方面的问题。
        准备：正式为类变量（static）分配内存并设置类变量默认值的阶段，这些内存都将在方法区中进行分配。(静态变量的定义使用final关键字，这类变量会在此阶段直接进行初始化)
        解析：虚拟机常量池内的符号引用（类、接口、字段和方法）替换为直接引用（地址）的过程。符号引用就是一组符号来描述所引用的目标，直接引用就是直接指向目标的指针、相对偏移量或者是一个能直接定位到目标的句柄。 

## 初始化
        执行类构造器<clinit>()方法的过程，类构造器<clinit>()方法是由编译器自动收集类中所有类变量的赋值动作和静态代码块的语句合并产生的。（直接访问父类的静态变量，不会触发子类的初始化。子类的初始化cinit调用之前，会先调用父类的cinit初始化方法。）
```

```java
public class ZeroTest {
    int i;  
    public void testMethod() {
        int j;  
        System.out.println(i);  
        // Variable 'j' might not have been initialized
        System.out.println(j);  
    }
}
// 因为i在初始化时有分配0，所有可以正常输出。但是j是局部变量，没有初始化就会报错。
```

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/5cbffbed7a074cef99db5bc1ecfd117f.png)

##### 类加载机制

- **全盘负责** ：就是当一个类加载器负责加载某 `个Class` 时，该 `Class` 所依赖的和引用的其他 `Class` 也将由该类加载器负责载入，除非显示使用另外一个类加载器来载入
- **父类委托** ：就是当一个类加载器负责加载某个Class时，先让父类加载器试图加载该 `Class` ，只有在父类加载器无法加载该类时才尝试从自己的类路径中加载该类
- **缓存机制** ：保证所有加载过的 `Class` 都会被缓存，当程序需要使用某个 `Class` 对象时，类加载器先从缓存区中搜索该 `Class` ，只有当缓存区中不存在该 `Class对象` 时，系统才会读取该类对应的二进制数据，并将其转换成 `Class对象` ，存储到缓存区

##### 类加载器有哪些？

- 启动类加载器(Bootstrap ClassLoader)用来加载java核心类库，C++编写，（只加载包为java、javax、sun等开头的类。）无法被java程序直接引用。
- 扩展类加载器(extensions class loader):它用来加载 Java 的扩展库。Java 虚拟机的实现会提供一个扩展库目录（java.ext.dirs属性指定目录、jre/lib/ext、如果用户创建的jar包放在此目录下，也会自动由扩展类加载器加载）。该类加载器在此目录里面查找并加载 Java 类。
- 系统类加载器（system class loader）：它根据 Java 应用的类路径（CLASSPATH）来加载Java 类。一般来说，Java 应用的类都是由它来完成加载的。
- 用户自定义类加载器 (user class loader)，用户通过继承 java.lang.ClassLoader类的方式自行实现的类加载器（可以直接继承URLClassLoader类，这样可以避免自己去编写findClass()方法以及获取字节码流的方式）。

##### 双亲委派模型

双亲委派模型的工作过程：如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到最顶层的启动类加载器中，只有当父加载器反馈自己无法完成这个加载请求时，子加载器才会尝试自己去完成加载。

一般情况下，由jvm指定的类加载器就是应用类加载器，jvm会自动调用其loadClass(String name)方法来开启类的加载过程。

- **‎loadClass（字符串名称，布尔解析）：‎**‎此方法用于加载 JVM 引用的类。它将类的名称作为参数。这是 loadClass（String， boolean） 的类型。‎
- **‎defineClass（）‎**‎：defineClass（） 方法是‎*‎最终‎*‎方法，不能重写。此方法用于将字节数组定义为类的实例。如果该类无效，则它抛出 ‎**‎ClassFormatError‎**‎。‎
- **‎findClass（字符串名称）：‎**‎此方法用于查找指定的类。此方法仅查找但不加载类。‎
- **‎findLoadedClass（字符串名称）：‎**‎此方法用于验证 JVM 引用的类以前是否被加载过。‎
- **‎Class.forName（字符串名称，布尔值初始化，类加载器加载器）：‎**‎此方法用于加载类以及初始化类。此方法还提供了选择任何一个 ClassLoaders 的选项。如果类装入器参数为 NULL，则使用 Bootstrap 类装入器。‎

```markdown
## 双亲委派的优点
    沙箱安全机制：自己写的java.lang.String.class类不会被加载，这样便可以防止核心API库被随意篡改
    避免类的重复加载：当父亲已经加载了该类时，就没有必要子ClassLoader再加载一次，保证被加载类的唯一性
作用：保证应用程序的稳定有序。
```

##### 父子类的加载顺序

- (1) 父类静态代码块(包括静态初始化块，静态属性，但不包括静态方法)
- (2) 子类静态代码块(包括静态初始化块，静态属性，但不包括静态方法 )
- (3) 父类非静态代码块( 包括非静态初始化块，非静态属性 )
- (4) 父类构造函数
- (5) 子类非静态代码块 ( 包括非静态初始化块，非静态属性 )
- (6) 子类构造函数

#### 运行时数据区

![JVM内存模型](https://gitee.com/xu_zuyun/picgo/raw/master/img/JVM内存模型(2).png)

```markdown
## 虚拟机存储结构
（1）虚拟机栈：在方法调用和返回中发挥重要作用。每个线程都独享自己的虚拟机栈，栈中的存储单元是栈帧(Frames)。每次调用方法都会在虚拟机栈中产生一个栈帧，每个栈帧中都有方法的参数、局部变量、方法出口等信息，方法执行完毕后释放栈帧
（2）本地方法栈：为native修饰的本地方法提供的空间，在HotSpot中与虚拟机合二为一
（3）程序计数器：用于记录当前线程执行到哪里，保存指令执行的地址，方便线程切回后能继续执行代码
线程共享区：
（4）堆内存：Jvm进行垃圾回收的主要区域，存放对象信息，分为新生代和老年代
（5）方法区：用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译后的代码等数据。

## JVM内存和直接内存
JVM内存和本地内存都属于(物理)内存的一部分，为什么要把它们分开讨论呢？因为目标不同，JVM是由JVM进程管理的一块内存空间，它可以对其中的内存进行自动垃圾收集。而本地内存是不受JVM管理，而且不受JVM内存设置的限制。

## 永久代(方法区)为什么要被元空间替换？
1. 永久代的限制
永久代有固定的大小，需要在JVM启动时指定，并且其最大值限制了可以加载的类的数量。随着应用程序规模的扩大尤其是在很多框架和动态生成类的场景下，经常会出现永久代内存溢出的问题（OutOfMemoryError: PermGen space错误）。调整和优化永久代大小成为了一项复杂且困难的任务。

2. 内存管理改进
永久代的垃圾收集（GC）与Java堆的垃圾收集是分开进行的，而且永久代的垃圾收集效率通常比较低。将方法区移到直接在本地内存中的元空间可以简化垃圾回收器的代码并提高性能。利用本地内存作为元空间，可以利用操作系统更有效地进行内存管理。对类加载器解除引用的类的元数据可以立即被释放，从而提高了内存回收的机会。

3. 方便未来的发展
使用本地内存解除了方法区大小的限制，使得Java类及元数据的扩展更加灵活。这样也方便HotSpot JVM与其他JVM如JRockit的融合，因为JRockit没有永久代的概念。
```

##### 示例

```java
public class MyClass {
    // 静态变量存储在方法区
    private static int staticVar = 10;

    // 常量也存储在方法区
    private static final String CONSTANT = "ConstantValue";

    // 实例变量存储在堆中
    private int instanceVar;

    // 构造方法
    public MyClass(int value) {
        // 局部变量存储在栈中，因为它们随方法调用而创建
        this.instanceVar = value;
    }

    // 实例方法存储在方法区，但调用时的参数和局部变量存储在栈中
    public void setInstanceVar(int value) {
        // 局部变量value存放在栈中
        this.instanceVar = value;
    }

    // 静态方法也是存储在方法区
    public static int getStaticVar() {
        return staticVar;
    }

    // main 方法作为程序入口点也是存储在方法区
    public static void main(String[] args) {
        // 局部变量myClass存储在栈中
        MyClass myClass = new MyClass(20); // MyClass 对象实例存储在堆中
    }
}
```

#### 本地方法栈

一个Native Method是一个Java调用非Java代码的接囗，该方法的实现由非Java语言实现，比如C。现在已经比较少见。因为现在的异构领域间的通信很发达，比如可以使用Socket通信。

Java虚拟机栈于管理Java方法的调用，而本地方法栈用于管理本地方法的调用。当某个线程调用一个本地方法时，它就进入了一个全新的并且不再受虚拟机限制的世界：它和虚拟机拥有同样的权限，并不是所有的JVM都支持本地方法。因为Java虚拟机规范并没有明确要求本地方法栈的使用语言、具体实现方式、数据结构等在Hotspot JVM中，直接将本地方法栈和虚拟机栈合二为一。

![图片](https://gitee.com/xu_zuyun/picgo/raw/master/img/7770edaf3e44939cb967a7120d2f21a7.png)

Java是编译与解释结合的语言，因为 Java 程序要经过先编译，后解释两个步骤，由 Java 编写的程序需要先经过编译步骤，生成字节码（`.class` 文件），这种字节码必须由 Java 解释器来解释执行。

6. 

```markdown
ThreadLocal 提供了线程本地变量，它可以保证访问到的变量属于当前线程，每个线程都保存有一个变量副本，每个线程的变量都不同。一旦线程不在存在，ThreadLocal 就应该被垃圾收集，但线程池有线程重用的功能，因此线程就不会被垃圾回收器回收，变量副本一直在内存中

## 解决方法
用完ThreadLocal一定要记得使用remove方法来进行清除。
```

#### 执行引擎

JVM的主要任务是负责**装载字节码到其内部**，执行引擎（Execution Engine）的任务就是**将字节码指令解释/编译为对应平台上的本地机器指令**，与JVM运行时数据区一起完成代码执行流程。
  JVM的执行引擎有两种：

- 解释器： 解释器响应速度快，对字节码采用逐行解释翻译成本地机器语言的方法去执行java代码
- JIT： JIT是直接将字节码翻译成本地机器相关（Windwos对应Windows、linux对应linux）的机器语言

  解释器与JIT相辅相成，一般而言首先都是它发挥作用，不必等待JIT编译器全部编译后再执行，省去不必要的编译时间。并且随着程序的运行，JIT编译器会逐渐发挥作用，根据热点探测功能（即根据方法调用计数器或者回边计数器来确定热点代码， 一般而言for循环内的循环体会被确认为热点代码），将有价值的字节码编译成本地机器指令存储在方法区的JIT代码缓存中，换取更高的程序执行效率。

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/41865a3b2d694f708369017b4f4e783b.png)

##### Runtime

运行时，是一个封装了JVM的类。每一个JAVA程序实际上都是启动了一个JVM进程，每一个JVM进程都对应一个Runtime实例，此实例是由JVM为其实例化的。所以我们不能实例化一个Runtime对象，应用程序也不能创建自己的 Runtime 类实例，但可以通过 getRuntime  方法获取当前Runtime运行时对象的引用。一旦得到了一个当前的Runtime对象的引用，就可以调用Runtime对象的方法去控制Java虚拟机的状态和行为。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk0MzU0OA==,size_16,color_FFFFFF,t_70.png)

在完成初始化后，类就可以被应用程序正常使用了。当你**调用一个方法时**，JVM会为这个方法创建一个新的栈帧，并压入到当前线程的Java栈中。Java栈是线程私有的内存区域，用于存储每个方法调用的状态，包括局部变量、操作数栈、动态链接等信息。

##### 局部变量表

- 局部变量表，它本质是一个数组，最基本的存储单元是slot，其大小在编译期间确认下来，局部变量表中的变量只在当前方法调用中有效，方法调用结束后局部变量表也会随之销毁，一个slot大小为32bit，主要**用于存储方法的入参和方法中定义的局部变量**
- 如果方法中创建的是引用类型的局部变量，存储的其实这个对象的指针，占用1个slot，如果当前帧是由实例方法创建的，那么该对象引用this将会存放在index为0的slot处，即数组的第一个元素的位置，其余的参数按照参数表顺序继续排列。如果该方法是抽象的，那么this会指向真正执行抽象类非抽象方法的子类的对象
- 如果是基本数据类型的局部变量，那么这个数值直接存储在局部变量表中，其中byte、short、char 在存储前被转换为int，占用1个slot，long或double占用2个slot
- returnAddress类型的变量

当一个方法开始执行后，只有两种方式可以退出，一是遇到任意一个方法返回的字节码指令（正常完成出口）；二是在方法执行过程中遇到异常且没有被被捕获（异常完成出口）。

当方法正常退出时，将该 PC 计数器的值会作为返回地址，返回给调用者。在方法异常退出时，返回地址是通过异常处理器表来确定的。

方法退出的过程实际上就等于把当前栈帧出栈，一般过程为：

- 恢复上层方法的局部变量表和操作数栈
- 把返回值压入调用者栈帧的操作数栈中
- 调整 PC 计数器的值，以指向方法调用指令后面的一条指令

##### 操作数栈

在方法执行过程中，根据字节码指令，将一系列操作数入栈，随后进行一系列操作如运算、复制、交换等…、运算结束后操作数出栈，结果再运算，然后将计算结果再存入局部变量表中

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/9fd09b22305d470193dbe368626031b4.png)

##### 流程

> **字节码执行：** Java 程序会被编译成字节码（.class 文件），JVM 在运行时通过解释器或即时编译器执行这些字节码。
>
> **栈帧创建：** 当 JVM 需要调用一个方法时，它会在当前线程的 Java 栈中为该方法调用创建一个新的栈帧。栈帧主要包含局部变量数组、操作数栈、动态链接信息和方法返回地址等部分。
>
> **局部变量和参数传递：** 调用方法时传入的参数以及方法内定义的局部变量会被存放在栈帧的局部变量数组中。
>
> **动态链接：**  当一个方法需要调用另一个方法时，JVM 可以通过动态链接找到这个方法的字节码并执行它。
>
> **方法执行：** 确定好要调用的具体方法后，JVM 执行该方法的字节码指令。如果是第一次调用，JVM 还可能对方法进行即时编译，将字节码转换为本地机器码以提高性能。
>
> **返回值处理：** 方法完成执行后，如果有返回值，通常会被压入调用者的操作数栈顶。
>
> **栈帧出栈：** 方法执行完毕后，当前方法的栈帧被退出（pop）出 Java 栈，控制流返回到方法被调用的地方，之前调用者的栈帧重新成为活动栈帧。

##### 实例

先看这段代码：

```java
public class Building {
    private static final int CONSTRUCTION_YEAR = 1998;

    public int calculateAge(int currentYear) {
        return currentYear - CONSTRUCTION_YEAR;
    }
}
public static void main(String[] args) {
    Building building = new Building();
    int age = building.calculateAge(2023);
}
```

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/299a0735cde649f4a05f1c98d92f6338~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp)

1. 方法调用：当Java代码执行到`building.calculateAge(2023)`时，首先JVM会通过对象引用（即`building`）查找到类`Building`，然后在类中查找`calculateAge`方法的符号引用。
2. 动态链接：JVM会根据`Building`类中的符号引用找到`calculateAge`方法在**运行时常量池**中的**直接引用**，获取改方法的内存地址。
3. 创建新的栈帧：JVM为调用的方法创建一个新的栈帧，并推入当前线程的Java栈顶。这个栈帧包含**局部变量表、操作数栈、动态链接和方法出口**。
4. 初始化局部变量表：JVM将方法调用的参数（即`currentYear`和`this`）存储到新栈帧的局部变量表中。
5. 更新程序计数器：JVM的程序计数器更新为`calculateAge`方法的第一条字节码指令。
6. 执行方法体： JVM开始执行`calculateAge`方法的字节码。当执行到`currentYear - CONSTRUCTION_YEAR`时，它会将`currentYear`和`CONSTRUCTION_YEAR`推入操作数栈，然后执行减法操作，并将结果推入操作数栈顶。
7. 方法返回：执行完`calculateAge`方法后，JVM将操作数栈顶的结果（即年龄）作为方法返回值，并将`calculateAge`方法的栈帧从Java栈中弹出。
8. 接收返回值：`calculateAge`方法的返回值被推入调用者（即`main`方法）的操作数栈中，并赋值给局部变量`age`。
9. 更新程序计数器：JVM的程序计数器更新为`main`方法的下一条指令。

```markdown
1. 局部变量表： 局部变量表被定义为一个数字数组， 保存返回地址参数、方法参数类型和方法体内的局部变量
注意： 基本数据类型（如byte/short/char）在存储前会被转换为int，boolean类型也会，对应的是0为false，1为true

2. 方法返回地址： 保存了PC寄存器的地址值，也就是调用者的PC计数器的值作为返回地址

3. 动态链接： 指向运行时常量池的方法引用。比如，描述一个方法调用了另外的其他方法时，就是通过常量池中指向方法的符号引用来表示的，动态链接的作用就是为了将这些符号引用转换为调用方法的直接引用

4. 操作数栈： 根据字节码指令，往栈中写入数据或提取数据。主要用于保存计算过程的中间结果，同时作为计算过程中临时变量的存储空间
```

```markdown
## 符号引用 和 直接引用
符号引用（Symbolic Reference）是一种用来描述引用目标的一组符号，可以是任何形式的字面量，比如类和接口的全限定名、字段的名称和描述符、方法的名称和描述符等。符号引用是在编译期或者运行期间生成的，它不依赖于具体的内存地址，而是在运行时根据上下文信息来定位目标。符号引用的作用是为了在程序运行时能够找到对应的目标。

直接引用（Direct Reference）则是一种直接指向目标的内存地址或者偏移量，它可以是指向对象实例的指针、指向类的静态变量的指针、指向类的方法的指针等。直接引用是在程序运行时生成的，它依赖于具体的内存地址，可以直接被 CPU 所执行。

## 静态链接
在解析过程中，Java 虚拟机会将符号引用转换成直接引用，从而能够正确地执行程序。

## 动态链接
动态链接是指在程序运行时，根据需要动态地加载和链接代码。在Java中，我们可以使用类加载器来动态加载新的类，也可以使用反射机制来动态获取和调用类的方法和字段。如果直接使用直接引用，我们通常需要在编译时就确定所有的引用，而不能在运行时动态地加载和链接代码。

我们可以使用继承和多态来实现运行时多态。比如，如果一个子类重写了父类的方法，那么在程序运行时，如果我们调用该方法，就会根据实际的对象类型来选择调用子类的方法还是父类的方法。

这些特性都需要在程序运行时根据实际情况来动态地选择和加载代码，这就需要Java虚拟机能够动态地解析符号引用，找到对应的直接引用，从而实现动态链接和运行时多态。
```

静态链接和动态链接对应的方法的绑定机制为：早期绑定（Early Binding，在编译期可知，且运行期保持不变）和晚期绑定（Late Binding，在编译期无法被确定下来，只能够在程序运行期根据实际的类型绑定相关的方法）。绑定是一个字段、方法或者类在符号引用被替换为直接引用的过程，这仅仅发生一次。

```java
public class StaticLinkingExample {
    public static void main(String[] args) {
        MyClass.staticMethod(); // 静态方法的调用，在类加载时会解析为直接引用
    }
}

class MyClass {
    public static void staticMethod() {
        System.out.println("This is a static method.");
    }
}
```

非静态的和非final的方法都是虚方法。动态链接允许Java实现动态方法派发:

通过虚方法表（vtable）支持动态链接。每个类都有一个虚方法表，其中列出了所有的虚方法。当调用一个虚方法时，JVM会根据对象实际类型的虚方法表来确定应该执行哪个方法实现。

```java
public class DynamicLinkingExample {
    public static void main(String[] args) {
        Parent parent = new Child();
        parent.virtualMethod(); // 运行时才确定调用Child还是Parent类中的方法
    }
}

class Parent {
    public void virtualMethod() {
        System.out.println("This is a virtual method from Parent.");
    }
}

class Child extends Parent {
    @Override
    public void virtualMethod() {
        System.out.println("This is a virtual method from Child.");
    }
}

```

```markdown
jvm中的常量池分为三种
    1.类文件常量池(Class Constant Pool) 也称静态常量池
    2.运行时常量池(Runtime Constant Pool)
    3.字符串常量池(String Constant Pool)
1. 类文件常量池
  我们写的每一个Java类被编译后，就会形成一份class文件（每个class文件都有一个class常量池）。 class文件中除了包含类的版本、字段、方法、接口等描述信息外，还有一项信息就是常量池(constant pool table)，用于存放编译器生成的各种字面量(Literal)和符号引用(Symbolic References)。
字面量包括：1.文本字符串 2.八种基本类型的值 3.被声明为final的常量等;
符号引用包括：1.类和方法的全限定名 2.字段的名称和描述符 3.方法的名称和描述符。
  这些常量池现在是静态信息，只有到运行时被加载到内存后，这些符号才有对应的内存地址信息，这些常量池一旦被装入内存就变成运行时常量池，对应的符号引用在程序加载（解析过程）中变为直接引用，或运行时会被转变变为被加载到内存区域的代码的直接引用（即动态链接）。

2. 运行时常量池
  运行时常量池（Runtime Constant Pool）是方法区的一部分。jdk1.8以前存在于永久代，jdk1.8之后存在于元空间。静态常量池中的内容，在类加载后会被存放到方法区的运行时常量池中。运行时常量池相对于Class文件常量池的另外一个重要特征是具备动态性，Java语言并不要求常量一定只有编译期才能产生，也就是说，并非预置入Class文件中静态常量池的内容才能进入方法区运行时常量池，运行期间也可以将新的常量放入池中（反射）。
```

```markdown
3. 字符串常量池
  字符串常量池存在运行时常量池之中（在JDK7之前存在运行时常量池之中，在JDK7已经将其转移到堆中）。字符串常量池的存在使JVM提高了性能和减少了内存开销。
  String类型的**静态变量**会被放到堆的**字符串常量池**中。它的目的就是为了减少相同字符串初始化带来的开销。
## 触发条件
String str1 = "hello";
String str4 = str3.intern();

字符串常量表达式，如 "Hello" + " World!"，在Java代码编译时就会被解析为单个字面量（"Hello World!"），并在类加载时加入字符串常量池。
## 机制：
当你在Java代码中创建一个字符串字面量，如String s = "hello";时，JVM会首先检查字符串常量池中是否已经存在内容为"hello"的字符串对象。
    如果存在，JVM就不会创建新的字符串对象，而是将引用指向已有的对象。
    如果不存在，JVM会在字符串常量池中创建一个新的字符串对象，并返回其引用。

String s1 = "Building";
String s2 = new String("Building");
System.out.println(s1 == s2); // False
System.out.println(s1 == s2.intern()); // True
## 为什么要单独放到堆中，不和运行时常量池放一起？
1. 垃圾回收
将字符串常量池放到堆内存中使得垃圾收集器能够更容易地管理常量池中不再使用的字符串。当字符串常量池位于永久代（PermGen，Java 8 之前的方法区实现）时，由于永久代的垃圾收集频率较低，字符串常量池更容易导致内存溢出错误（OutOfMemoryError: PermGen space）。而堆空间通常比永久代大得多，有更高效的垃圾回收策略。
2. 内存节省
在堆内存中，字符串常量池可以与堆上的其他字符串对象共享相同的字符数组，这样就可以节省内存，因为重复的字符串字面量不需要存储多份相同的字符数组。
```

##### 运行时常量池、Class常量池、字符串常量池的区别与联系

虚拟机启动过程中，会将各个Class文件中的常量池（存的是字面量和符号引用）载入到运行时常量池中。所以，  Class常量池只是一个媒介场所。在JVM真的运行时，需要把常量池中的常量加载到内存中，进入到运行时常量池，由此可知，运行时常量池也是每个类都有一个。字符串常量池可以理解为运行时常量池分出来的部分。加载时，对于class的静态常量池，如果字符串会被装到字符串常量池中。

##### JAVA什么情况下会内存溢出？

堆内存溢出：（1）当对象一直创建而不被回收时（2）加载的类越来越多时（3)虚拟机栈的线程越来越多时

栈溢出：方法调用次数过多，一般是递归不当造成

##### 类初始化时机

如果我们写一个**懒加载**，在使用时才初始化，那么我们的内存就会减少很多

```java
public class ConfigManager {
    private Map<String, Supplier<Config>> allConfigs = new HashMap<>();

    public ConfigManager() {
        // 在初始化阶段，只是将配置类的构造函数注册到map中
        allConfigs.put("config1", Config1::new);
        allConfigs.put("config2", Config2::new);
        // ...
        allConfigs.put("configN", ConfigN::new);
    }

    public Config getConfig(String name) {
        return allConfigs.get(name).get();
    }
}

```
##### JAVA什么情况下会内存泄漏？

1. 单例模式

   ```
       1、单例类只能有一个实例。
       2、单例类必须自己创建自己的唯一实例。
       3、单例类必须给所有其他对象提供这一实例。
   ```

2. 静态集合类

   ```
   (HashMap、Vector 等集合生命周期和应用程序一致),长生命周期的对象持有短生命周期对象的引用，尽管短生命周期的对象不再使用，但是因为长生命周期对象持有它的引用而导致不能被回收
   ```

3. 连接（I/O）未释放

   ```
   程序中创建或者打开一个流或者是新建一个网络连接的时候，JVM 都会为这些资源类分配内存做缓存，常见的资源类有网络连接，数据库连接以及 IO 流。如果忘记关闭这些资源，会阻塞内存，从而导致 GC 无法进行清理。
   ```

4. 变量作用域过大

   ```
   一个变量的定义作用域大于其使用范围，很可能存在内存泄漏；或不再使用对象没有及时将对象设置为 null，很可能导致内存泄漏的发生。
   ```

5. hash值发生改变

```
在HashMap和HashSet这种集合中，常常用到equal()和hashCode()来比较对象，如果重写不合理，会认为每次创建的对象都是新的对象，从而导致内存不断的增长.
```

6. ThreadLocal使用不当

   ```
   在线程未结束（如线程池中的线程）的情况下，如果没有显式地调用 ThreadLocal 的 remove() 方法，那么 ThreadLocal 变量持有的对象就不会被回收
   ThreadLocal 对象作为键使用弱引用(这种设计的主要目的是允许 ThreadLocal 对象在没有其他强引用时可以被垃圾收集器回收)，而其存储的线程局部变量作为值使用强引用。一旦 ThreadLocal 对象被垃圾收集器回收，没有及时从 ThreadLocalMap 中清理对应的值，就会导致这些值无法被回收
   ```

   ```java
   public class ThreadLocalMemoryLeakExample {
   
       private static final ThreadLocal<Object> threadLocal = new ThreadLocal<>();
   
       public static void main(String[] args) throws InterruptedException {
           // 创建一个线程
           Thread thread = new Thread(() -> {
               // 在 ThreadLocal 中放入一个大对象
               threadLocal.set(new BigObject());
               // 不调用 remove() 方法，模拟遗忘的场景
               // 如果这里调用了 remove()，则不会发生内存泄漏
               // threadLocal.remove();
           });
   
           // 启动线程
           thread.start();
           // 等待线程执行完毕
           thread.join();
   
           // 这时候原线程的引用已经消失，但是由于没有调用 threadLocal.remove()
           // 方法，BigObject 对象仍然被 ThreadLocalMap 引用着，即使它已经不再需要了
   
           // 提示 JVM 进行垃圾回收，实际上不保证立即进行回收
           System.gc();
   
           // 休眠一段时间，增加GC完成的机会
           Thread.sleep(10000);
   
           // 此时，虽然 thread 已经结束了生命周期，
           // 但是 BigObject 实例由于 ThreadLocal 的引用仍然在内存中
           // 直到 thread 对象被 GC 回收，且重新有其他线程使用这个 ThreadLocal 变量
           // 才能清除 BigObject 实例，如果 thread 是线程池中的线程，那么
           // BigObject 实例可能会更长时间地留在内存中
       }
   
       // 模拟一个占用大量内存的对象类
       static class BigObject {
           // 假设这里占用了大量内存
           private byte[] bigSize = new byte[1024 * 1024 * 100]; // 约 100MB
       }
   }
   ```

#### JVM中的对象

类元信息：

- **类的全限定名**：完整的类名，包括包含它的包名。

  **父类信息**：除了 `java.lang.Object`（所有类的根类），每个类都有一个父类。

  **接口信息**：类实现的所有接口的列表。

  **字段信息**：类及其父类的所有字段，包括字段名称、类型、修饰符（如 public、private、protected、static、final 等）以及是否被标记为 volatile 或 transient。

  **方法信息**：类定义的方法，包括方法名称、返回类型、参数类型列表、修饰符、抛出的异常等。

  **构造函数信息**：用于创建类实例的构造函数，包含与普通方法类似的信息。

  **常量池**：编译期间生成的一种表结构，用于存储文本字符串、类名、接口名、字段名和其他常量。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/a0625eba9a084ae2811b810244325944.png)

##### 对象的内存布局

- **对象头**主要由两部分组成：

​	第一部分存储对象自身的运行时数据：哈希码、GC分代年龄、锁状态标志、线程持有的锁、偏向线程ID、偏向时间戳等，官方称它为Mark Word，它是个动态的结构，随着对象状态变化。第二部分是类型指针，指向对象的类元数据类型（即对象代表哪个类）。此外，如果对象是一个Java数组，那还应该有一块用于记录数组长度的数据。

- **实例数据**用来存储对象真正的有效信息，也就是我们在程序代码里所定义的各种类型的字段内容，无论是从父类继承的，还是自己定义的。
- **对齐填充**不是必须的，没有特别含义，仅仅起着占位符的作用。是让字段只出现在同一CPU的缓存行中。如果字段不是对齐的，那么就有可能出现跨缓存行的字段。也就是说，该字段的读取可能需要替换两个缓存行，而该字段的存储也会同时污染两个缓存行。这两种情况对程序的执行效率而言都是不利的。其实对其填充的最终目的是为了计算机高效寻址。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/3ce17201325a4cba952d62a3bfb530d8.png)

##### 对象怎么访问定位？

Java程序会通过栈上的reference数据（指向对象的引用）来操作堆上的具体对象，对象访问方式也是由虚拟机实现而定的，HotSpot虚拟机主要使用直接指针来进行对象访问，主流的访问方式主要有使用句柄和直接指针两种：

- 对象类型数据就是被虚拟机加载的类信息（对象的类型、父类、实现的接口、方法等）
- 对象实例数据就是被new出来的对象信息（对象中各个实例字段的数据）

![图片](https://gitee.com/xu_zuyun/picgo/raw/master/img/020803b8cf052a6770cb51200db753e9.png)

```
句柄访问的话，Java堆中将可能会划分出一块内存来作为句柄池，reference中存储的就是对象的句柄地址，而句柄中包含了对象实例数据与类型数据各自具体的地址信息

优点：
reference中存储的是稳定句柄地址，在对象被移动（垃圾收集时移动对象是非常普遍的行为）时只会改变句柄中的实例数据指针，而reference本身不需要被修改。
```

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FSZWFsbHlNYW4=,size_16,color_FFFFFF,t_70.png)

```
指针访问的话，Java堆中对象的内存布局就必须考虑如何放置访问类型数据的相关信息，reference中存储的直接就是对象地址，如果只是访问对象本身的话，就不需要多一次间接访问的开销

优点：
速度更快，它节省了一次指针定位的时间开销
```

##### 对象调用方法的过程

1、编译器通过对象的声明类型和和方法名，在此类型和其超类中寻找权限为public的方法。
 2、编译器根据方法调用时传入的参数类型和个数判断调用哪一个1步骤中寻找到的方法
 3、若是调用的方法是[private](https://so.csdn.net/so/search?q=private&spm=1001.2101.3001.7020)、static、final声明的话，那么编译器将会清楚的知道调用哪一个方法，这一种调用方式称之为静态绑定。相反若不是上述声明则编译器根据对象声明的类型，动态的去寻找要调用的方法，这称之为动态绑定。
 4、动态绑定时，编译器会根据声明的对象所引用的实际的对象类型，来寻找与声明对象最为合适的方法 比如在对象的多态中，声明为父类的对象，实际引用的是其子类，父类对象调用的方法应为子类中所覆写的方法。

每次调用方法都要进行方法搜索，时间开销相当大。因此，虚拟机预先为每个类创建一个方法表，其中列举了所有方法的签名和实际调用的方法。这样一来，在真正调用方法的时候，虚拟机仅查找这个表就行了。

#### 方法调用

Java中方法调用唯一目的就是确定要调用哪一个方法

![img](https://img-blog.csdnimg.cn/img_convert/fa9b9d924624f32c9a8c68b902ccd766.webp?x-oss-process=image/format,png)

##### 非虚方法与虚方法

非虚方法: 静态方法，私有方法，父类中的方法，被final修饰的方法，实例构造器，非虚方法的特点就是没有重写方法，适合在类加载阶段就进行解析(符号引用->直接引用) 【编译时就能够确定】

其他不是非虚方法的方法就是虚方法

```java
 public class Father {
     public static void staticMethod(){
         System.out.println("father static method");
     }
     public final void finalMethod(){
         System.out.println("father final method");
     }
     public Father() {
         System.out.println("father init method");
     }
     public void overrideMethod(){
         System.out.println("father override method");
     }
 }
 public interface TestInterfaceMethod {
     void testInterfaceMethod();
 }
 public class Son extends Father{
     public Son() {
         //invokespecial 调用父类init 非虚方法
         super();
         //invokestatic 调用父类静态方法 非虚方法
         staticMethod();
         //invokespecial 调用子类私有方法 特殊的非虚方法
         privateMethod();
         //invokevirtual 调用子类的重写方法 虚方法
         overrideMethod();
         //invokespecial 调用父类方法 非虚方法
         super.overrideMethod();
         //invokespecial 调用父类final方法 非虚方法
         super.finalMethod();
         //invokedynamic 动态生成接口的实现类 动态调用
         TestInterfaceMethod test = ()->{
             System.out.println("testInterfaceMethod");
         };
         //invokeinterface 调用接口方法 虚方法
         test.testInterfaceMethod();
     }
     @Override
     public void overrideMethod(){
         System.out.println("son override method");
     }
     private void privateMethod(){
         System.out.println("son private method");
     }
     public static void main(String[] args) {
         new Son();
     }
 }
```

#### JVM配置

- **指定垃圾回收器**：使用`-XX:+UseSerialGC`、`-XX:+UseParallelGC`、`-XX:+UseConcMarkSweepGC`、`-XX:+UseG1GC`或`-XX:+UseZGC`来选择特定的垃圾回收器。
- **堆大小**：使用`-Xms`和`-Xmx`来设置堆的初始大小和最大大小。
- **新生代大小**：使用`-Xmn`来设置新生代的大小。
- **详细的GC日志**：`-Xlog:gc*`可以启用详细的GC日志，这对于性能分析和问题诊断非常有用。
- **一些其他的优化参数**：如`-XX:SurvivorRatio`、`-XX:PermSize`和`-XX:MaxPermSize`等。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlbjM2NTg4MA==,size_16,color_FFFFFF,t_70.png)

### 四、Java垃圾回收机制

Java中的每个对象都经历了创建、使用和最终被回收的过程。从**对象**实例化开始，它可能被程序的多个部分**引用**，直到最后一个**引用消失**，对象成为垃圾，等待回收。

#### JVM垃圾查找算法

（1）引用计数法：已淘汰，为每个对象添加引用计数器，引用为0时判定可以回收，会有两个对象相互引用无法回收的问题
（2）可达性分析法：从GCRoot开始往下搜索，搜索过的路径称为引用链，若一个对象GCRoot没有任何的引用链，则判定可以回收（三色法：未标记、标记中、已标记）

```markdown
可以作为GC Roots的主要有四种对象：
    虚拟机栈(栈帧中的本地变量表)中引用的对象
    方法区中类静态属性引用的对象
    方法区中常量引用的对象
    本地方法栈中JNI引用的对象
```

#### 三色标记法

三色标记法是一种用于垃圾回收（GC）的算法，特别是在“标记-清除”类型的垃圾回收器中。这种方法使用三种颜色（通常为白色、灰色和黑色）来追踪对象（即内存中分配的数据结构）的状态。每种颜色代表了对象在垃圾回收过程中的不同阶段：

1. **白色**：表示对象尚未被访问。在垃圾回收开始时，所有的对象都被标记为白色。
2. **灰色**：表示对象已被标记为活动（即被引用），但其引用的对象还没有全部检查。换句话说，灰色对象至少有一个引用，但是它引用的其他对象是否可达尚未完全确定。
3. **黑色**：表示对象本身及其所有可以通过该对象访问到的对象都已经被扫描，确认为活动对象，不会在当前的垃圾回收周期中被清理。

垃圾回收过程使用三色标记法的一般步骤如下：

1. **初始化**：所有对象最初都标记为白色。
2. **根对象标记**：从“根集合”（通常包括全局变量和活跃线程的堆栈上的变量）开始扫描，所有根对象标记为灰色，并放入待处理队列中。
3. **扫描过程**：从灰色对象的队列中取出一个对象，将其标记为黑色，并检查所有它引用的对象：
   - 如果发现任何白色对象，将其标记为灰色并加入队列以供后续处理。
   - 继续这个过程，直到没有更多的灰色对象为止。
4. **清理**：此时所有仍然保持为白色的对象都是不可达的（即无法从根集合通过引用路径访问的对象），因此可以安全地回收其占用的内存。

三色标记法的关键优点之一是它支持增量和并发垃圾回收。由于整个扫描过程可能很耗时间，允许应用程序在垃圾回收器运行标记阶段的同时执行，可以减少导致应用程序暂停的情况（避免长时间的Stop-The-World事件）。然而，由于应用程序在运行的过程中可能会修改对象的引用关系，因此需要使用某些策略（例如写屏障）来确保新产生的或消失的引用关系能够被垃圾回收器正确处理。

#### JVM的垃圾回收算法

（1）标记清除算法： 标记不需要回收的对象，然后清除没有标记的对象，会造成许多内存碎片。

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlbjM2NTg4MA==,size_16,color_FFFFFF,t_70-20240103162947320.png)

（2）复制算法： 将内存分为两块，只使用一块，进行垃圾回收时，先将存活的对象复制到另一块区域，然后清空之前的区域。用在新生代

```
优点：解决碎片化问题，顺序分配内存简单高效
缺点：只适用于存活率低的场景，如果极端情况下如果对象面上的对象全部存活，就要浪费一半的存储空间。
```

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlbjM2NTg4MA==,size_16,color_FFFFFF,t_70-20240103163017798.png)

（3）标记整理算法： 与标记清除算法类似，但是在标记之后，将存活对象向一端移动，然后清除边界外的垃圾对象。用在老年代

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3JlbjM2NTg4MA==,size_16,color_FFFFFF,t_70-20240103163205587.png)

（4）分代收集算法：一般将 java 堆分为新生代和老年代，比如在新生代中，选择复制算法，只需要付出少量对象的复制成本就可以完成每次垃圾收集。而老年代的对象存活几率是比较高的，选择“标记-清除”或“标记-整理”算法进行垃圾收集。

#### 分代回收机制

- 在年轻代，对象的存活率相对较低，因此采用如标记-复制算法会更为高效。
- 老年代中的对象已经证明了自己的存活能力，所以此处的GC会比年轻代更加稀少，可以使用如标记-清除-整理算法进行处理。

![图片](https://gitee.com/xu_zuyun/picgo/raw/master/img/bd3ee6277775ac9f0021a50ed77787aa.png)

- 年轻代主要分为`Edan`区、Survivor区（`from/S1`区、`to/S2`区），这三者的区域大小划分比例为`8:1:1`
- 年轻代中对象被GC清理在S1与S2轮转`15`次之后会被移到老年代（这个值可以通过JVM参数：–XX:SurvivorRatio 设定，默认值为8）
- 年轻代与老年代区域大小划分比例为`1:2`（该值可以通过JVM参数：-XX:NewRatio 设定）

```markdown
当GC确定一些对象为“不可达”时，GC就有责任回收这些内存空间。可以。程序员可以手动执行System.gc()，通知GC运行，但是Java语言规范并不保证GC一定会执行。

## 第一步：
大对象直接进入老年代
长期存活对象将进入老年代

1. Eden 区
Java 新对象的出生地（如果新创建的对象占用内存很大，则直接分配到老年代）。当 Eden 区内存不够的时候就会触发 MinorGC，对新生代区进行一次垃圾回收。
2. ServivorFrom
上一次 GC 的幸存者，作为这一次 GC 的被扫描者。
3. ServivorTo
保留了一次 MinorGC 过程中的幸存者。
```

![3ff6f9cc5dc34c3ca68b3585212174e2](https://gitee.com/xu_zuyun/picgo/raw/master/img/3ff6f9cc5dc34c3ca68b3585212174e2.png)

```markdown
## 第二步：
老年代的对象比较稳定，所以 MajorGC 不会频繁执行。当无法找到足够大的连续空间分配给新创建的较大对象时也会提前触发一次 MajorGC 进行垃圾回收腾出空间。
MajorGC 采用标记清除算法：
首先扫描一次所有老年代，标记出存活的对象，然后回收没有标记的对象。MajorGC的耗时比较长，因为要扫描再回收。MajorGC 会产生内存碎片，为了减少内存损耗，我们一般需要进行合并或者标记出来方便下次直接分配。

## 第三步：
当老年代也满了装不下的时候，就会抛出 OOM（Out of Memory）异常.

永久代：主要存放 Class 和 Meta（元数据）的信息,Class 在被加载的时候被放入永久区域，它和和存放实例的区域不同,GC 不会在主程序运行期对永久区域进行清理。所以这也导致了永久代的区域会随着加载的 Class 的增多而胀满，最终抛出 OOM 异常。
```

#### 强引用、软引用、弱引用、虚引用?

在 Java 中，`Reference` 类及其子类是用于实现引用对象的基类。`Reference` 类主要有三个子类：`SoftReference`、`WeakReference`、和 `PhantomReference`，它们分别代表软引用、弱引用和虚引用。

![无标题.png](https://gitee.com/xu_zuyun/picgo/raw/master/img/07617826516f438a85cbd836bb85bfdc~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp)

1. 强引用：是我们经常写的普通对象引用，把一个对象赋给一个引用变量（在 C/C++ 中的名词称为指针变量），这个引用变量就是一个强引用。当一个对象被强引用变量引用时，它处于`可达状态`，它是不可能被垃圾回收机制回收的，因此强引用是造成 Java 内存泄漏的主要原因之一。
2. 软引用：``软引用通常用在对内存敏感的程序中，比如高速缓存就有用到软引用，内存够用的时候就保留，不够用就回收
3. 弱引用：当一个对象只被弱引用引用时，在下一次`垃圾回收（GC）时，即使内存空间足够，也会被回收`
4. 虚引用：形同虚设，虚引用并不会决定对象的生命周期，`作用是跟踪对象被垃圾回收的状态以及引用对象被放入与之关联的引用队列中，允许程序员在适当的时机进行一些额外的操作`。

#### finalize()方法？

垃圾回收器(garbage colector)决定回收某对象时，就会运行该对象的finalize()方法：

- 第一次被标记后，对象在在finalize()中成功拯救自己，只要重新与引用链上的任何一个对象建立关联即可
- 回收特殊渠道申请的内存，一般内存不需要亲自回收，但JNI(Java Native Interface)调用non-Java程序（C或C++）产生的无法被gc自动回收。

#### 收集器种类

- Serial收集器： 新生代采用复制算法，老年代采用标记-整理算法。单线程，所以在发生GC时候需要暂停所有线程的工作（"Stop-The-World"）。
- Parallel Scavenge收集器：（相比Serial GC只是垃圾回收线程变多而已）适用于**吞吐量比较高的场景**，一些计算场景并不在意停顿时间的长短，还存在STW现象（"Stop-The-World"）
- ParNew收集器：（jdk8默认设置的垃圾收集器）新生代采用复制算法，老年代采用标记-整理算法。和Parallel 相似主要用于配合CMS收集器使用。
- CMS收集器：**低延迟**的**并发型**垃圾收集器。适用于希望**减少暂停时间**的应用。(用户和垃圾回收线程可以同时工作，当然还需要少量的STW用于清除**浮动垃圾**)，采用“标记-清除”算法，是老年代的垃圾收集器，只能配合ParNew或Serial使用。没有整理过程，会导致**内存碎片化**。
- G1 GC：从JDK9开始，它作为默认的垃圾回收器。它将堆分为多个区域（Reigon）并发地标记、复制和清除这些区域。限制垃圾回收的暂停时间，并提供高吞吐量。
- ZGC：ZGC的目标是在任何堆大小下都能实现不到10毫秒的暂停时间，同时还能提供与其他垃圾回收器相似的吞吐量。

```markdown
## 如何选择：
**响应时间要求**：如果应用对延迟非常敏感，那么选择如ZGC或CMS这样的暂停时间短的垃圾回收器会更合适。
**吞吐量要求**：高吞吐量的应用，如批处理作业或某些后端任务，可能更适合使用Parallel GC或G1 GC。
**内存资源**：如果内存资源有限，Serial GC可能是一个好选择。
```

写屏障

1. **前向（Pre-Write）屏障**

   在对字段赋新值之前执行。它通常用于记忆集（Remembered Set），即一个记录从老年代指向年轻代对象的指针集合。在赋新值前，原有的引用关系会被添加到记忆集，这样可以确保后续的垃圾收集过程能够考虑到这些变化。

2. **后向（Post-Write）屏障**

   在对字段赋新值之后执行。例如，在三色标记和并发标记的场景下，一旦一个黑色对象被更新，指向一个白色对象的新引用就必须被记录下来，以避免漏标。这可以通过将该黑色对象重新标记为灰色来实现，确保垃圾收集器会再次扫描该对象。

#### CMS(Concurrent Mark-Sweep):

1. **初始标记（Initial Mark）**

   这个阶段需要停止所有的应用线程（Stop-The-World），仅标记GC Roots能直接关联到的对象，并且修正所有并发标记期间因用户程序继续运行而导致标记记录不准确的部分。标记完这些对象后，初始标记阶段就结束了。由于只是标记一小部分对象，所以此阶段通常很快。

2. **并发标记（Concurrent Marking）**

   在这个阶段，CMS收集器遍历对象图的过程中，并发地进行，不需要暂停用户应用线程。它从初始标记阶段找到的根对象开始遍历整个对象图，标记所有存活的对象。由于是和用户线程一起并发执行的，所以可能存在对象引用关系改变的情况，需要通过写屏障来记录发生变化的对象。

3. **重新标记（Remark）**

   为了修正并发标记期间由于用户程序继续运行可能带来的标记遗漏或错误，这个阶段会再次停止所有应用线程（Stop-The-World）。这个阶段完成后，垃圾收集器会得到所有存活对象的准确集合。

4. **并发清除（Concurrent Sweeping）**

   在这个阶段，CMS收集器删除不再被使用的对象，释放出空间。同样，这个过程是并发进行的，不需要暂停应用线程。

#### 监控垃圾回收

**GC日志**: JVM可以配置为输出GC日志，这些日志详细记录了垃圾回收的过程和结果。通过分析这些日志，开发者可以获取关于内存使用情况、垃圾收集的频率和持续时间等重要信息。

**监控工具**: 工具如JVisualVM和JConsole不仅可以实时显示JVM的性能指标，还提供了丰富的图形界面，帮助开发者直观地了解垃圾回收的行为。

频繁发生YOUNG GC

### 五、JAVA 网络/IO

- **BIO**是阻塞I/O，NIO是非阻塞I/O，AIO是异步I/O。BIO每个连接对应一个线程，NIO多个连接共享少量线程，AIO允许应用程序异步地处理多个操作。
- **NIO**，通过Selector，只需要一个线程便可以管理多个客户端连接，当客户端数据到了之后，才会为其服务
- **AIO**是适合高吞吐量的应用程序，异步 IO 基于时间和回调机制实现的：也就是应用操作之后会直接返回，不会阻塞在那里，后台处理完成后，操作系统会通知相应的线程进行后续的操作。但AIO在Java中的支持相对有限，不是所有操作系统都支持。

#### JAVA BIO

![](https://gitee.com/xu_zuyun/picgo/raw/master/img/68747470733a2f2f70332d6a75656a696e2e62797465696d672e636f6d2f746f732d636e2d692d6b3375316662706663702f34333833623962363361636534363539626631336631633861346335356365307e74706c762d6b3375316662706663702d7a6f6f6d2d696e2d63726f70.webp)

服务端会有个ServerSocket，每一个ServerSocket都有一个与之对应ClientSocket，每一个连接都需要有一个线程来维护

#### JAVA NIO

1.Java NIO 全称 Java non-blocking IO，是指 JDK 提供的新 API。从 JDK1.4 开始，Java 提供了一系列改进的输入/输出的新特性，被统称为 NIO(即 NewIO)，是[同步非阻塞](https://www.zhihu.com/search?q=同步非阻塞&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2835854418})的。

Java NIO 的[非阻塞模式](https://www.zhihu.com/search?q=非阻塞模式&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2835854418})，使一个线程从某通道发送请求或者读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取，而不是保持[线程阻塞](https://www.zhihu.com/search?q=线程阻塞&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2835854418})，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。非阻塞写也是如此，一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。

![](https://gitee.com/xu_zuyun/picgo/raw/master/img/68747470733a2f2f70332d6a75656a696e2e62797465696d672e636f6d2f746f732d636e2d692d6b3375316662706663702f61323064353862613839366434326334616162326338373162623266346437397e74706c762d6b3375316662706663702d7a6f6f6d2d696e2d63726f70-20240110230024721.webp)

##### ByteBuffer

缓冲区(ByteBuffer)：缓冲区本质上是一个**可以读写数据的内存块**，可以理解成是一个**容器对象(含数组)**，该对象提供了一组方法，可以更轻松地使用内存块，缓冲区对象内置了一些机制，能够跟踪和记录缓冲区的状态变化情况。ByteBuffer的指针有postion

![img](https://gitee.com/xu_zuyun/picgo/raw/master/img/v2-e689586b7689bcdb1aeab4abd45272ef_1440w.webp)

##### Channel

NIO 的通道类似于流，主要的区别是：通道可以同时进行读写，而流只能读或者只能写。通道可以实现异步读写数据。通道可以从缓冲读数据，也可以写数据到缓冲。BIO的流是单向的，NIO的通道是双向的，可以读也可以写操作。

##### Selector

Java 的 NIO，用非阻塞的 IO 方式。可以用一个线程，处理多个的客户端连接，就会使用到 Selector(选择器)。Selector 能够检测多个注册的通道上是否有事件发生(注意：多个 Channel 以事件的方式可以注册到同一个 Selector)，如果有事件发生，便获取事件然后针对每个事件进行相应的处理。

这样就可以只用一个单线程去管理多个通道，也就是管理多个连接和请求。只有在连接/通道真正有读写事件发生时，才会进行读写，就大大地减少了系统开销，并且不必为每个连接都创建一个线程，不用去维护多个线程。避免了多线程之间的上下文切换导致的开销。

##### NIO示例

```java
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Set;

public class NioServerExample {

    public static void main(String[] args) {
        try {
            // 打开服务器套接字通道
            ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
            
            // 服务器绑定到指定端口
            serverSocketChannel.socket().bind(new InetSocketAddress(8080));
            
            // 配置为非阻塞模式
            serverSocketChannel.configureBlocking(false);
            
            // 打开选择器
            Selector selector = Selector.open();
            
            // 向selector注册通道监听接受事件
            serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

            while (true) {
                // 非阻塞地等待通道上的事件
                selector.select();

                // 获取选择键集合
                Set<SelectionKey> selectedKeys = selector.selectedKeys(); //通过selectedKeys()方法获取已就绪的事件集合
                Iterator<SelectionKey> iter = selectedKeys.iterator();

                while (iter.hasNext()) {
                    SelectionKey key = iter.next();

                    if (key.isAcceptable()) {// 事件是接受新的客户端连接
                        // 接受客户端连接
                        SocketChannel client = serverSocketChannel.accept();
                        client.configureBlocking(false);
                        client.register(selector, SelectionKey.OP_READ);
                        System.out.println("Accepted connection from " + client);
                    }

                    if (key.isReadable()) {// 事件是可读事件
                        // 读取数据
                        SocketChannel client = (SocketChannel) key.channel();
                        ByteBuffer buffer = ByteBuffer.allocate(1024);
                        int bytesRead = client.read(buffer);
                        if (bytesRead == -1) {
                            client.close();
                        } else if (bytesRead > 0) {
                            // 切换缓冲区至读模式
                            buffer.flip();
                            // 回写数据
                            client.write(buffer);
                            buffer.clear();
                        }
                    }

                    iter.remove();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### 流

- 流不能通过索引读写数据
- 流中的数据只能顺序读取

```java
// 将流整合起来以便实现更高级的输入和输出操作。如可以把InputStream包装到BufferedInputStream中以实现缓冲，即从磁盘中一次读取一大块数据
InputStream input = new BufferedInputStream(new FileInputStream("c:\data\input-file.txt"));
OutputStream output = new BufferedOutputStream(new FileOutputStream("c:\data\output-file.txt"));
```

Java的IO流共涉及40多个类，实际上非常规则，都是从以上4个[抽象基类](https://so.csdn.net/so/search?q=抽象基类&spm=1001.2101.3001.7020)派生的

- java.io.InputStream

  ```java
  int read()
  	从输入流中读取数据的下一个字节。返回 0 到 255 范围内的 int 字节值。如果因为已经到达流末尾而没有可用的字节，则返回值 -1。
  int read(byte[] b)
  	从此输入流中将最多 b.length 个字节的数据读入一个 byte 数组中。如果因为已经到达流末尾而没有可用的字节，则返回值 -1。否则以整数形式返回实际读取的字节数。
  int read(byte[] b, int off, int len)
  	将输入流中最多 len 个数据字节读入 byte 数组。尝试读取 len 个字节，但读取的字节也可能小于该值。
  ```

- java.io.OutputStream

  ```java
  void write(int b)
  将指定的字节写入此输出流。write 的常规协定是：向输出流写入一个字节。要写入的字节是参数 b 的八个低位。b 的 24 个高位将被忽略。 即写入0~255范围的。
  void write(byte[] b)
  将 b.length 个字节从指定的 byte 数组写入此输出流。
  void write(byte[] b,int off,int len)
  将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此输出流。
  public void flush()throws IOException
  刷新此输出流并强制写出所有缓冲的输出字节，调用此方法指示应将这些字节立即写入它们预期的目标。
  ```

- java.io.Reader

  ```java
  int read()
  	读取单个字符。作为整数读取的字符，范围在 0 到 65535 之间 (0x00-0xffff)（2个字节的Unicode码），如果已到达流的末尾，则返回 -1
  int read(char[] cbuf)
    将字符读入数组。如果已到达流的末尾，则返回 -1。否则返回本次读取的字符数。
  int read(char[] cbuf,int off,int len)
  	将字符读入数组的某一部分。存到数组cbuf中，从off处开始存储，最多读len个字符。如果已到达流的末尾，则返回 -1。否则返回本次读取的字符数。
  ```

- java.io.Writer

  ```java
  void write(int c)
  写入单个字符。要写入的字符包含在给定整数值的 16 个低位中，16 高位被忽略。 即写入0 到 65535 之间的Unicode码。
  void write(char[] cbuf)
  写入字符数组。
  void write(char[] cbuf,int off,int len)
  写入字符数组的某一部分。从off开始，写入len个字符
  void write(String str)
  写入字符串。
  void write(String str,int off,int len)
  写入字符串的某一部分。
  void flush()
  刷新该流的缓冲，则立即将它们写入预期目标
  ```

![在这里插入图片描述](https://gitee.com/xu_zuyun/picgo/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NwYWRlX0t3bw==,size_16,color_FFFFFF,t_70.png)

1. ##### JavaIO 文件：

用来将文件转换为二进制字节流或字符流

- **FileInputStream（二进制型数据）：按顺序地读取文件中的字节，每次读取一个字节**
- **FileReader（字符型数据）：按顺序地读取文件中的字符，每次读取一个字符**
- **FileOutputStream（二进制型数据）：每次写入一个字节，数据按照写入顺序存储在文件当中**
- **FileWriter（字符型数据）：每次写入一个字符，数据按照写入顺序存储在文件当中**

```java
//二进制流范例，打开一个文件的输入流，读取到字节数组，再写入另一个文件的输出流
public void test1() {
  try {
    FileInputStream fileInputStream = new FileInputStream(new File("a.txt"));
    FileOutputStream fileOutputStream = new FileOutputStream(new File("b.txt"));
    byte []buffer = new byte[128];
    while (fileInputStream.read(buffer) != -1) {
      fileOutputStream.write(buffer);
    }
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  }
}
```

```java
// 字符流范例
public static void main(String[] args) throws IOException {
  FileReader fileReader = new FileReader("file.txt");
  FileWriter fileWriter = new FileWriter("file2.txt");
  //一次拷贝4个字节
  char[] chars = new char[1024*1024];
  int read;
  while ((read = fileReader.read(chars)) != -1) {
  	fileWriter.write(chars, 0, read);
  }
  fileWriter.flush();
  if (fileReader != null) {
  	fileReader.close();
  }
  if (fileWriter != null) {
  	fileWriter.close();
  }

}
```

2. ##### JavaIO管道

Java IO中的管道为运行在同一个JVM中的两个线程提供通信的能力，所以管道也可以作为数据源以及目标媒介。**不同的JVM中线程不能利用管道进行通信**

```java
//使用管道来完成两个线程间的数据点对点传递
@Test
public void test2() throws IOException {
  PipedInputStream pipedInputStream = new PipedInputStream();
  PipedOutputStream pipedOutputStream = new PipedOutputStream(pipedInputStream);
  new Thread(new Runnable() {
    @Override
    public void run() {
      try {
        pipedOutputStream.write("hello input".getBytes());
        pipedOutputStream.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }).start();
  new Thread(new Runnable() {
    @Override
    public void run() {
      try {
        byte []arr = new byte[128];
        while (pipedInputStream.read(arr) != -1) {
          System.out.println(Arrays.toString(arr));
        }
        pipedInputStream.close();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }).start();
```

3. ##### JavaIO 字节和字符数组

不仅可以用来做缓存数据的临时存储空间，同时也可以作为数据来源或者写入目的

```java
//字符数组和字节数组在io过程中的作用
public void test4() {
  //arr和brr分别作为数据源
  char []arr = {'a','c','d'};
  CharArrayReader charArrayReader = new CharArrayReader(arr);
  byte []brr = {1,2,3,4,5};
  ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(brr);
}
```

4. ##### 序列化和反序列化

通过ObjectInputStream转换InputStream为Object

```java
ObjectInputStream input = new ObjectInputStream(new FileInputStream("object.data"));
MyClass object = (MyClass) input.readObject();
input.close();
```

5. ##### 流的缓冲

- BufferedReader：为字符输入流提供缓冲区，可以提高许多IO处理的速度（**默认为8KB**）
- BufferedInputStream：为原始字节输入流提供缓冲区，可以提高许多IO处理的速度

```java
 @Test
    public void BufferedStreamTest() throws FileNotFoundException {
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
            //1.造文件
            File srcFile = new File("爱情与友情.jpg");
            File destFile = new File("爱情与友情3.jpg");
            //2.造流
            //2.1 造节点流
            FileInputStream fis = new FileInputStream((srcFile));
            FileOutputStream fos = new FileOutputStream(destFile);
            //2.2 造缓冲流
            bis = new BufferedInputStream(fis);
            bos = new BufferedOutputStream(fos);

            //3.复制的细节：读取、写入
            byte[] buffer = new byte[10];
            int len;
            while((len = bis.read(buffer)) != -1){
                bos.write(buffer,0,len);

//                bos.flush();//刷新缓冲区
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.资源关闭
            //要求：先关闭外层的流，再关闭内层的流
            if(bos != null){
                try {
                    bos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if(bis != null){
                try {
                    bis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            //说明：关闭外层流的同时，内层流也会自动的进行关闭。关于内层流的关闭，我们可以省略.
//        fos.close();
//        fis.close();
        }
    }
```

6. ##### 流的转换

通过InputStreamReader、OutputStreamWriter

```java
 @Test
    public void test1() throws IOException {

        FileInputStream fis = new FileInputStream("dbcp.txt");
//        InputStreamReader isr = new InputStreamReader(fis);//使用系统默认的字符集
        //参数2指明了字符集，具体使用哪个字符集，取决于文件dbcp.txt保存时使用的字符集
        InputStreamReader isr = new InputStreamReader(fis,"UTF-8");//使用系统默认的字符集

        char[] cbuf = new char[20];
        int len;
        while((len = isr.read(cbuf)) != -1){
            String str = new String(cbuf,0,len);
            System.out.print(str);
        }

        isr.close();

    }
```

7. ##### 标准输入输出流

System.in和System.out分别代表了系统标准的输入和输出设备
默认输入设备是：键盘，输出设备是：显示器
System.in的类型是InputStream
System.out的类型是PrintStream，其是OutputStream的子类FilterOutputStream 的子类

```java
BufferedReader br = null;
InputStreamReader isr = new InputStreamReader(System.in);
br = new BufferedReader(isr);

while (true) {
    System.out.println("请输入字符串：");
    String data = br.readLine();
    if ("e".equalsIgnoreCase(data) || "exit".equalsIgnoreCase(data)) {
    		System.out.println("程序结束");
    break;
}
```

#### 字节流和字符流的区别

字节流：处理字节和字节数组或二进制对象（stream命名）； 字符流：处理字符、字符数组或字符串（Reader、Writer命名）。

- 1、字节流在操作的时候本身是不会用到缓冲区（内存）的，是与文件本身直接操作的，而字符流在操作的时候是使用到缓冲区的
- 2、字节流在操作文件时，即使不关闭资源（close方法），文件也能输出，但是如果字符流不使用close方法的话，则不会输出任何内容，说明字符流用的是缓冲区，并且可以使用flush方法强制进行刷新缓冲区，这时才能在不close的情况下输出内容
-  字节流用于操作包含ASCII字符的文件；字符流用于读取包含Unicode字符的文件；对于英文字符文件，字节流和字符流都可以用

#### 序列化

- 序列化 (Serialization)：将对象的状态信息转换为可以存储或传输的形式的过程
- 反序列化：将字节对象或XML编码格式还原成完全相等的对象

```java
public class 序列化和反序列化 {
​//注意，内部类不能进行序列化，因为它依赖于外部类 
@Test 
public void test() throws IOException { 
A a = new A(); 
a.i = 1; 
a.s = "a"; 
FileOutputStream fileOutputStream = null; 
FileInputStream fileInputStream = null; 
try { 
//将obj写入文件 
fileOutputStream = new FileOutputStream("temp"); 
ObjectOutputStream objectOutputStream = new ObjectOutputStream(fileOutputStream);
objectOutputStream.writeObject(a); 
fileOutputStream.close();
 //通过文件读取obj 
fileInputStream = new FileInputStream("temp"); 
ObjectInputStream objectInputStream = new ObjectInputStream(fileInputStream); 
A a2 = (A) objectInputStream.readObject(); 
fileInputStream.close(); 
System.out.println(a2.i); 
System.out.println(a2.s); 
//打印结果和序列化之前相同
} catch (IOException e)
{ e.printStackTrace(); 
} catch (ClassNotFoundException e) 
{ e.printStackTrace(); } } }
 
class A implements Serializable {
 
    int i;
    String s;
}
```

##### 序列化ID

反序列化的前提是序列化ID得相同，Eclipse提供两种产生序列化ID的方法，一种是：属性名+时间戳，另一种是我们一般用1L表示。

```java
// 虽然两个类的路径和功能代码完全一致，但是序列化 ID 不同，他们无法相互序列化和反序列化
public class A implements Serializable { 
    private static final long serialVersionUID = 1L; 
    private String name; 
    public String getName() 
    { 
        return name; 
    } 
    public void setName(String name) 
    { 
        this.name = name; 
    } 
} 
 
public class A implements Serializable { 
    private static final long serialVersionUID = 2L; 
    private String name; 
    public String getName() 
    { 
        return name; 
    } 
    public void setName(String name) 
    { 
        this.name = name; 
    } 
}
```

1. 序列化引擎会根据对象时候实现了`Serializable`接口类，如果没有则抛`NotSerializableException`异常（这是因为，在序列化操作过程中会对类型进行检查，要求被序列化的类必须属于Enum、Array和Serializable类型其中的任何一种。），如果实现了则会创建`ObjectOutputStream `对象，绑定到输出流上
2. 扫描对象中`static（因为其为类的状态）`、 `transient`关键字的字段，不对其进行序列化，Object对象流不写入，也不读出。
3. 如果对象有其他对象的引用则需要递归将引用对象也进行序列化
4. 如果要序列化的类有父类，要想同时将在父类中定义过的变量持久化下来，那么父类也应该集成java.io.Serializable接口，父类没实现Serializable接口那么，子类序列化时父类不会序列化。当反序列化变为对象时，因为子类对象创建会先创建父类，所以会调用父类的构造方法。子类对象的属性的值存在，父类对象的属性的值为0或者为null
5. ArrayList实现了java.io.Serializable接口,可以对它进行序列化及反序列化。但是其中数组elementData（用来保存列表中的元素的）是transient的，正常不应该被序列化保存下来。

```markdown
为什么ArrayList要通过重写writeObject 和 readObject 方法来实现序列化呢？
    为什么数组elementData是transient的
ArrayList实际上是动态数组，每次在放满以后自动增长设定的长度值，如果数组自动增长长度设为100，而实际只放了一个元素，那就会序列化99个null元素。为了保证在序列化的时候不会将这么多null同时进行序列化，ArrayList把元素数组设置为transient

    为什么要重写writeObject 和 readObject 方法
   前面说过，为了防止一个包含大量空对象的数组被序列化，为了优化存储，所以，ArrayList使用transient来声明elementData。
   但是，作为一个集合，在序列化过程中还必须保证其中的元素可以被持久化下来，所以，通过重写writeObject 和 readObject方法的方式把其中的元素保留下来。
   
        writeObject方法把elementData数组中的元素遍历的保存到输出流（ObjectOutputStream）中。
        readObject方法从输入流（ObjectInputStream）中读出对象并保存赋值到elementData数组中。
```

序列化引擎：Dubbo 框架中的 Hession、JDK 自带的 Serializable、跨语言的 Hessian、ProtoBuf、ProtoStuff
