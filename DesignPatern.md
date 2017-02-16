# Java 设计模式

## 一、设计模式

设计模式是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。
使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。

### 代码规范
- 首先是要注意注释文档的格式，注释文档将用来生成 HTML 格式的代码报告，
所以注释文档必须书写在类、域、构造函数、方法、定义之前。
- 注释文档由两部分组成——描述、块标记。
- 注释的种类：文件头注释、构造函数注释、域注释、方法注释和定义注释。
文件头注释需要注明该文件的创建时间、文件名、命名空间信息。构造函数注释采用描述部分注明构造
函数的作用。
方法注释采用描述部分注明方法的功能，块标记注明方法的参数、返回值、异常等信息。

### 面向对象
> 面向对象OO = 面向对象的分析OOA + 面向对象的设计OOD + 面向对象的编程OOP

### 接口
接口好比一种模板，这种模板定义了对象必须实现的方法，其目的就是让这些方法可以作为接口实例被引用。
接口不能被实例化。类可以实现多个接口并且通过这些实现的接口被索引。
接口变量只能索引实现该接口的类的实例。

### 接口和抽象的区别
1. abstract class 在Java 语音中表示的是一种继承关系，一个类只能使用一次继承关系。
但是一个类却可以实现多个interface
2. abstract class 中可以有自己的数据成员，也可以有非abstract 的成员方法，而在
interface 中，只能够有静态的不能被修改的数据成员（也就是必须是static final 的，不过在
interface 中一般不定义数据成员），所有的成员方法都是abstract
3. Abstract class 和interface 所反映出的设计理念不同。其实abstract classs 表示的是"is---a"
关系，interface 表示的是“like---a”关系。
4. 实现抽象类和接口的类必须实现其中的所有方法。抽象类中可以有非抽象方法。接口中
则不能有实现方法。
5. 接口中定义的变量默认是public static final 型，且必须给其初值，所以实现类中不能重
新定义，也不能改变其值。
6. 抽象类中的变量默认是friendly 型，其值可以再子类中重新定义，也可以重新赋值。
7. 接口中的方法默认都是public,abstract 类型的。

### 接口和委托的区别
接口可以包含属性、索引、方法以及事件。但委托不能包含事件。

### 软件设计原则：
- 开闭原则(OCP)<br>
对扩展开放，对修改关闭
- 里氏代换原则(LSP)<br>
任何类出现的地方，子类一定可以出现
- 依赖倒转原则(DIP)<br>
尽量依赖于抽象，而不要依赖于具体
- 接口隔离原则(ISP)<br>
尽量小的单独的接口，而不要提供大的接口，使通信尽可能窄
- 合成复用原则(CARP)<br>
尽量采取合成和聚合的方式达到复用，而不要使用继承达到复用
- 迪米特法则<br>
一个软件的实体尽可能少的与其他实体发生作用

## 1.1 创建型模式

- AbstractFactory (抽象工厂)<br>
- FactoryMethod (工厂方法)<br>
- Singleton (单态模式)<br>
- Builder (建造者模式)<br>
- ProtoType (原型模式)

## 1.1.1 工厂方法
定义一个用于创建对象的接口，让子类决定实例化哪一个类。FactoryMethod
使一个类的实例化延迟到其子类。

### 适用性
1. 当一个类不知道它所必须创建的对象的类的时候。
2. 当一个类希望由它的子类来指定它所创建的对象的时候。
3. 当想将创建对象的职责委托给多个帮助子类中的某一个，并且希望将哪一
个帮助子类是代理者这一信息局部化的时候。

### 参与者
1. Product<br>
定义工厂方法所创建的对象的接口。
2. ConcreteProduct<br>
实现Product 接口。
3. Creator<br>
声明工厂方法，该方法返回一个Product 类型的对象<br>
Creator 也可以定义一个工厂方法的缺省实现，它返回一个缺省的
ConcreteProduct 对象。<br>
可以调用工厂方法以创建一个Product 对象。
4. ConcreteCreator<br>
重定义工厂方法以返回一个ConcreteProduct 实例。

### 类图
![](pic1.png)

### 例子
#### product
<pre><code>
public interface Work {
    void doWork();
}
</code></pre>

#### ConcreteProduct

<pre><code>
public class StudentWork implements Work {
    public void doWork() {
        System.out.println("学生做作业!");
    }
}
public class TeacherWork implements Work {
    public void doWork() {
        System.out.println("老师审批作业!");
    }
}
</code></pre>

#### Creator
<pre><code>
public interface IWorkFactory {
    Work getWork();
}
</code></pre>

#### ConcreteCreator
<pre><code>
public class StudentWorkFactory implements IWorkFactory {
    public Work getWork() {
        return new StudentWork();
    }
}
public class TeacherWorkFactory implements IWorkFactory {
    public Work getWork() {
        return new TeacherWork();
    }
}
</code></pre>

#### Test
<pre><code>
public class Test {
    public static void main(String[] args) {
        IWorkFactory studentWorkFactory = new StudentWorkFactory();
        studentWorkFactory.getWork().doWork();
        IWorkFactory teacherWorkFactory = new TeacherWorkFactory();
        teacherWorkFactory.getWork().doWork();
    }
}
</code></pre>

#### result
> 学生做作业!<br>
> 老师审批作业!

## 1.1.2 抽象工厂

提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。

### 适用性

1. 一个系统要独立于它的产品的创建、组合和表示时。
2. 一个系统要由多个产品系列中的一个来配置时。
3. 当你要强调一系列相关的产品对象的设计以便进行联合使用时。
4. 当你提供一个产品类库，而只想显示它们的接口而不是实现时。

### 参与者
1. AbstractFactory<br>
声明一个创建抽象产品对象的操作接口。
2. ConcreteFactory<br>
实现创建具体产品对象的操作。
3. AbstractProduct<br>
为一类产品对象声明一个接口。
4. ConcreteProduct<br>
定义一个将被相应的具体工厂创建的产品对象。<br>
实现 AbstractProduct 接口。
5. Client<br>
仅使用由 AbstractFactory 和 AbstractProduct 类声明的接口

### 类图
![](pic2.png)

- 优点： 对产品的增加支持 OCP 原则
- 缺点： 对产品系列的增加不支持 OCP 原则

### 例子
#### AbstractFactory
<pre><code>
public interface IAnimalFactory {
    ICat createCat();
    IDog createDog();
}
</code></pre>

#### ConcreteFactory
<pre><code>
public class BlackAnimalFactory implements IAnimalFactory {
    public ICat createCat() {
        reture new BlackCat();
    }
    public IDog createDog() {
        return new BlackDog();
    }
}

public class WhiteAnimalFactory implements IAnimalFactory {
    public ICat createCat() {
        return new WhiteCat();
    }
    public IDog createDog() {
        return new WhiteDog();
    }
}
</code></pre>

#### AbstractProduct
<pre><code>
public interface ICat {
    void eat();
}
public interface IDog {
    void eat();
}
</code></pre>

#### Concreteproduct
<pre><code>
public class BlackCat implements ICat {
    public void eat() {
        System.out.println("The black cat is eating!");
    }
}
public class WhiteCat implements ICat {
    public void eat() {
        System.out.println("The white cat is eating!");
    }
}
public class BlackDog implements IDog {
    public void eat() {
        System.out.println("The black dog is eating");
    }
}
public class WhiteDog implements IDog {
    public void eat() {
        System.out.println("The white dog is eating!");
    }
}
</code></pre>

#### Client
<pre><code>
public static void main(String[] args) {
    IAnimalFactory blackAnimalFactory = new BlackAnimalFactory();
    ICat blackCat = blackAnimalFactory.createCat();
    blackCat.eat();
    IDog blackDog = blackAnimalFactory.createDog();
    blackDog.eat();
    IAnimalFactory whiteAnimalFactory = new WhiteAnimalFactory();
    ICat whiteCat = whiteAnimalFactory.createCat();
    whiteCat.eat();
    IDog whiteDog = whiteAnimalFactory.createDog();
    whiteDog.eat();
}
</code></pre>

#### result
> The black cat is eating!<br>
> The black dog is eating!<br>
> The white cat is eating!<br>
> The white dog is eating!<br>

## 1.1.3 创造者模式

将一个复杂对象的构成与它的表示分离，使同样的构建过程可以创建不同的
表示。
### 适用性
1. 当创建复杂对象的算法应该独立于该对象的组成部分以及它们的装配方式时。
2. 当构造过程必须允许被构造的对象有不同的表示时。

### 参与者
1. Builder<br>
为创建一个Product 对象的各个部件指定抽象接口。
2. ConcreateBuilder<br>
实现Builder 的接口以构造和装配该产品的各个部件。<br>
定义并明确它所创建的表示。<br>
提供一个检索产品的接口。
3. Director<br>
构造一个使用Builder 接口的对象。
4. Product<br>
表示被构造的复杂对象。ConcreteBuilder 创建该产品的内部表示并定义它
的装配过程。<br>
包含定义组成部件的类，包括将这些部件装配成最终产品的接口。

### 类图
![](pic3.png)
将产品的内部表象和产品的生成过程分割开来，从而使一个建造过程生成具有不同的内部表象的产品对象。
建造模式使得产品内部表象可以独立地变化，客户不必知道产品内部组成地细节。
建造模式可以强制实行一种分步进行地建造步骤。

### 例子
#### Builder
<pre><code>
public interface PersonBuilder {
    void buildHead();
    void buildBody();
    void buildFoot();
    Person buildPerson();
}
</code></pre>

#### ConcreateBuilder
<pre><code>
public class ManBuilder implements PersonBuilder {
    Person person;
    public ManBuilder() {
        person = new Man();
    }
    publoc void buildBody() {
        person.setBody("建造男人的身体");
    }
    public void buildFoot() {
        person.setFoot("建造男人的脚");
    }
    public void buildHead() {
        person.setHead("建造男人的头");
    }
    public Person buildPerson() {
        reture person;
    }
}
</code></pre>

#### Director
<pre><code>
public class PersonDirector {
    public Person constructPerson(PersonBuilder pb) {
        pb.buildHead();
        pb.buildBody();
        pb.buildFoot();
        return pb.buildPerson();
    }
}
</code></pre>

#### Product
<pre><code>
public class Person {
    private String head;
    private String body;
    private String foot;
    public String getHead() {
        return head;
    }
    public void setHead(String head) {
        this.head = head;
    }
    public String getBody() {
        return body;
    }
    public void setBody(String body) {
        this.body = body;
    }
    public String getFoot() {
        return foot;
    }
    public void setFoot(String foot) {
    this.foot = foot;
    }
}

public class Man extends Person {
}
</code></pre>

#### Test
<pre><code>
public class Test{
    public static void main(String[] args) {
        PersonDirector pd = new PersonDirector();
        Person person = pd.constructPerson(new ManBuilder());
        System.out.println(person.getBody());
        System.out.println(person.getFoot());
        System.out.println(person.getHead());
    }
}
</code></pre>

#### result
>建造男人的身体<br>
>建造男人的脚<br>
>建造男人的头<br>










