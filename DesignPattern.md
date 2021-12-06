# 设计模式

## 七大设计原则

### 开闭原则

对修改封闭，对扩展开放

- 软件测试时只需测试扩展的部分

- 可提高代码可复用性

- 可提高软件可维护性

### 里氏替换原则

子类能够代替父类，且父类出现的地方子类一定能够出现。

子类可以扩展父类的功能，但不能改变父类原有的功能。

- 子类可以实现父类的抽象方法，但不能覆盖父类的非抽象方法
- 子类中可以增加自己特有的方法
- 当子类的方法重载父类的方法时，方法的前置条件（即方法的输入参数）要比父类的方法更宽松
- 当子类的方法实现父类的方法时（重写/重载或实现抽象方法），方法的后置条件（即方法的的输出/返回值）要比父类的方法更严格或相等

### 依赖倒置原则

### 单一职责（功能）原则

一个类应该有且仅有一个引起它变化的原因（功能尽可能单一）。

### 接口隔离原则

### 迪米特法则

### 合成复用原则

主要分为三种：创建型、结构型、行为型。

创建型：单例、原型、工厂方法、抽象工厂、建造者

结构型：代理、适配器、桥接、装饰、外观、享元、组合

行为型：模板、策略、命令、职责链、状态、观察者、中介者、迭代器、备忘录、解释器

## 单例模式



<div align='center'>
    <img src='./imgs/Singleton.png' width='1000px'>
    </br></br>单例模式
</div>


```java
//饿汉式
public class Hungry {
    private Hungry(){}
    private final static Hungry HUNGRY = new Hungry();
    public static Hungry getInstance(){
        return HUNGRY;
    }
}

//静态内部类实现单例
public class Holder {
    private Holder() {}

    public static Holder getInstance(){
        return InnerClass.HOLDER;
    }

    public static class InnerClass{
        private static final Holder HOLDER = new Holder();
    }
}


//懒汉式单例
public class Lazy {
    private Lazy(){
        //System.out.println(Thread.currentThread().getName() + "start");
        synchronized (Lazy.class){
            if(lazy == null){
                throw new RuntimeException("不要试图使用反射破坏异常");
            }
        }
    }

    private volatile static Lazy lazy;

    //双重检测锁模式的懒汉式单例 DCL懒汉式
    public static Lazy getInstance() {
        if(lazy == null)
        {
            synchronized (Lazy.class){
                if(lazy == null)
                {
                    lazy = new Lazy();
                    //不是一个原子操作
                    /**
                     *1. 分配内存空间
                     *2. 执行构造方法，初始化对象
                     *3. 把这个对象指向这个空间
                     *
                     * 123 Thread A
                     * 132
                     *     Thread B //此时还没有完成构造
                     */
                }
            }
        }
        return lazy;
    }
}
```



## 简单工厂模式



<div align='center'>
    <img src='./imgs/SimpleFactory.png' width='1400px'>
    </br></br>简单工厂模式
</div>


```java
//操作类
package simpleFactory;

/**
 * @author simple.jbx
 */
abstract class Operation {
    private double numA = 0, numB = 0;

    public double getNumA() {
        return numA;
    }

    public void setNumA(double numA) {
        this.numA = numA;
    }

    public double getNumB() {
        return numB;
    }

    public void setNumB(double numB) {
        this.numB = numB;
    }

    public abstract double getResult();
}

class OperationAdd extends Operation {
    @Override
    public double getResult() {
        return getNumA() + getNumB();
    }
}

class OperationSub extends Operation {
    @Override
    public double getResult() {
        return getNumA() - getNumB();
    }
}

class OperationMul extends Operation {
    @Override
    public double getResult() {
        return getNumA() * getNumB();
    }
}

class OperationDiv extends Operation{
    @Override
    public double getResult() {
        if(0.0 == getNumB()) {
            System.out.println("Error:被除数不能为0");
            return 0.0;
        } else {
            return getNumA() / getNumB();
        }
    }
}

//运算类
public class OperationFactory {
    public static Operation createOperation(String operate) {
        Operation oper = null;
        switch (operate) {
            case "+" :
                oper = new OperationAdd();
                break; //这里不加break可以编译通过 C#不允许穿透 java c等可以
            case "-" :
                oper = new OperationSub();
                break;
            case "*" :
                oper = new OperationMul();
                break;
            case "/" :
                oper = new OperationDiv();
                break;
            default:
                break;
        }
        return oper;
    }

    public static void main(String[] args) {
        Operation oper = OperationFactory.createOperation("+");
        oper.setNumA(1.0);
        oper.setNumB(2.0);
        System.out.println(oper.getResult());
    }
}

```

## 代理模式