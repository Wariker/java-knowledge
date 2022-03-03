# 观察者模式

> 优点：
> 
>             降低目标与观察者的耦合度
> 
>             被观察者发送通知，观察者都会收到信息
> 
> 缺点：
> 
>             如果观察者过多，则接收通知十分耗时
> 
>             如果被观察者有循环依赖，则发送通知时会使观察者循环调用，使系统崩溃

---

> 抽象主题Subject：定义attach，detach，notify
> 
> 具体主题：实现具体功能
> 
> 抽象观察者Observer：定义update
> 
> 具体观察者：获取主题的通知

---

> JDK 的java.util包下，有两个包
> 
> Observable（被观察者包）
> 
>         addObserver(Observer o);添加观察者
> 
>         notifyObservers(Object arg);调用集合中的所有观察者的update，因为是倒                                                               序通知，所以越晚加入的观察者越早通知
> 
>         setChange();   包内部有一个change变量，只有为true的时候，才通知
> 
>         clearChange();    将change置为false
> 
> Observer（观察者包）

-----

> 抽象主题

```java
//Subject
public interface Subject{
    void attach(Observer observer);
    void detach(Observer observer);
    void notify(String message);
}
```

---

> 具体主题

```java
//BroadcastSubject
public class BroadcastSubject implements Subject{
    List<Observer> list = new ArrayList<Observer>();

    @Override
    public void attach(Observer observer){
        list.add(observer);
    }
    @Override
    public void detach(Observer observer){
        list.remove(observer);
    }
    @Override
    public void notify(String message){
        for(Observer observer: list){
            observer.update(message);
        }
    }
}
```

---

> 抽象观察者

```java
//Observer
public interface Observer{
    void update(String message);
}
```

---

> 具体观察者

```java
//User
public class User implements Observer{
    public String name;
    public User(){}
    public User(String name){
        this.name = name;
    }

    public void update(String message){
        System.out.println(name + "-" + message);
    }
}
```






