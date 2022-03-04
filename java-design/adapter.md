# 适配者模式

> 定义：
> 
> 将一个类的接口转换成客户希望的另外一个接口，使得原本
> 
> 由于接口不兼容而不能一起工作的那些类可以一起工作
> 
> 分为类适配器模式和对象适配器模式，也有接口适配器模式。4
> 
> -
> 
> 主要格式：
> 
> - 目标接口（target）：当前系统业务所期待的接口，可以是抽象类或者接口
> 
> - 被适配者类（Adaptee）：被访问和适配的现存组件库中的组件接口
> 
> - 适配器类（Adapter）：通过继承或引用被适配者的对象，实现目标接口

---

> 场景：
> 
> 一台Computer只能读取SDCard，无法直接读取TFCard
> 
> 目标接口为SDCard

```java
public class Computer {
    public Computer(){}

    public void readSD(SDCard sdCard){
        String msg = sdCard.readSD();
        System.out.println(msg);
    }
}
```

```java
public interface SDCard {
    String readSD();
    void writeSD(String msg);
}


public class SDCardImpl implements SDCard{

    @Override
    public String readSD() {
        return "read SD Card";
    }

    @Override
    public void writeSD(String msg) {

    }
}
```

```java
public interface TFCard {
    String readTF();
    void writeTF();
}
         
public class TFCardImpl implements TFCard{
    @Override
    public String readTF() {
        return "read TF Card";
    }

    @Override
    public void writeTF() {

    }
}
```

---

> 如果是类适配器
> 
> 违反了合成复用原则
> 
> 并且如果目标类没有给出接口，则只能继承其实现类，java只有单继承
> 
> 所以一般使用对象适配器

```java
public class SDAdapterTF extends TFCardImpl implements SDCard{
    public SDAdapterTF(){}

    @Override
    public String readSD() {
        return readTF();
    }

    @Override
    public void writeSD(String msg) {

    }
}
```

> 如果是对象适配器

```java
public class SDAdapterTF implements SDCard {
    private TFCard tfCard;
    public SDAdapterTF(TFCard tfCard){
        this.tfCard = tfCard;
    }

    @Override
    public String readSD() {
        return tfCard.readTF();
    }

    @Override
    public void writeSD(String msg) {

    }
}
```

---

> 适配器模式还有接口适配器
> 
> 如果一个接口定义大量方法，其实现类不想全部实现
> 
> 则可以定义一个抽象类以空方法的形式实现该接口
> 
> 子类通过继承的方式实现所需的方法
