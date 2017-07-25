## Builder模式介绍
Builder模式是一步一步创建一个复杂对象的创建型模式，它允许用户在不知道内部构建细节的情况下，可以更精细地控制对象的构造流程。该模式是为了将构建复杂对象的过程和它的部件解耦，使得构建过程和部件的表示隔离开来。
## Builder模式的定义
将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。
## Builder模式的使用场景
* 相同的方法，不同的执行顺序，产生不同的事件结果时。
* 多个部件或零件，都可以装配到一个对象中，但是产生的运行结果又不相同时
* 产品类非常复杂，或者产品类中的调用顺序不同产生了不同的作用，这个时候使用建造者模式非常合适。
* 当初始化一个对象特别复杂，如参数多，且很多参数都具有默认值时。
## UML类图
![](http://i2.kiimg.com/599943/cb890fdc533a0de2.jpg)
* Product产品类--产品的抽象类；
* Builder--抽象Builder类，规范产品的组建，一般是由子类实现具体的组建过程；
* ConcreteBuilder--具体的Builder类；
* Director--统一组装过程;
## 示例代码
这里以组装计算机为例，把组装计算机的过程简化为构建主机、设置操作系统、设置显示器3个部分，然后通过Director和具体的Builder来构建计算机对象。
* 计算机抽象类，即Product角色
```java
public abstract class Computer {
    protected String mBoard;
    protected String mDisplay;
    protected String mOs;
    protected Computer() {

    }
    //设置CPU核心数
    public void setBoard(String board) {
        mBoard = board;
    }
    //设置内存
    public void setDisplay(String display) {
        mDisplay = display;
    }
    //设置操作系统
    public abstract void setOs();
    @Override
    public String toString() {
        return "Computer{" +
                "mBoard='" + mBoard + '\'' +
                ", mDisplay='" + mDisplay + '\'' +
                ", mOs='" + mOs + '\'' +
                '}';
    }
}
```
* 具体的Computer类，MacBook
```java
public class MacBook extends Computer {
    protected MacBook() {
    }
    @Override
    public void setOs() {
        mOs="Mac os10.0";
    }
}
```
* 抽象Builder类
```java
public abstract class Builder {
    //设置主机
    public abstract void buildBoard(String board);
    //设置显示器
    public abstract void buildDisplay(String display);
    //设置操作系统
    public abstract void buildOs();
    public abstract Computer create();
}
```
* 具体的Builder类，MacBookBuilder
```java
public class MacBookBuilder extends Builder {
    private Computer mComputer=new MacBook();
    @Override
    public void buildBoard(String board) {
        mComputer.setBoard(board);
    }

    @Override
    public void buildDisplay(String display) {
        mComputer.setDisplay(display);
    }

    @Override
    public void buildOs() {
        mComputer.setOs();
    }

    @Override
    public Computer create() {
        return mComputer;
    }
}
```
* Director类，负责构造Computer
```java
public class Director {
    Builder mBuilder = null;

    public Director(Builder builder) {
        mBuilder = builder;
    }

    public void construct(String board, String display) {
        mBuilder.buildBoard(board);
        mBuilder.buildDisplay(display);
        mBuilder.buildOs();
    }
}
```
* 运行类Test
```java
public class Test {
    public static void main(String[] args) {
        Builder builder = new MacBookBuilder();
        Director pcDirector = new Director(builder);
        pcDirector.construct("英特尔主板", "Retina显示器");
        System.out.println("计算机信息" + builder.create().toString());
    }
}
```
通过具体的MacBookBuilder来构建MacBook对象，而Director封装了构建复杂产品对象的过程，对外隐藏构建细节。Builder与Director一起将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的对象。然而在现实开发中，Director角色经常会被省略，直接使用一个Builder来进行对象的组装，这个Builder通常为链式调用，它的关键点是每个setter方法都返回自身，这样就使得setter方法可以链式调用。这种形式不仅去除了Director角色，整个结构也更加简单，也能对Produce对象的组装过程有更精细地控制。
## Android中Builder模式的实现
* 最常用Builder模式的就是AlertDialog.Builder模式。使用该Builder模式来构建复杂的AlertDialog。
## 优点
* 良好的封装性，使用建造者模式可以使客户端不必知道产品内部组成的细节。
* 建造者独立，容易扩展。
## 缺点
* 会产生多余的Builder对象以及Director对象，消耗内存。
