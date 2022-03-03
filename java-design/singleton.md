# 单例模式

> 普通懒汉式
> 
> 这种方式的单例对线具有线程问题

```java
public class Singleton{
    private Singleton(){}
    private static Singleton singleton = null;

    public static getInstance(){
        if(singleton==null){
            singleton = new Singleton();
        }
        return singleton;
    }
}
```



> 将获取方法更改成同步方法
> 
> 这种方式效率过低

```java
public class Singleton{
    private Singleton(){}
    private static Singleton singleton = null;
    public synchronized static Singleton getInstance(){
        if(singleton==null){
            singleton = new Singleton();
        }
        return singleton;
    }
}
```



> 双重锁懒汉式
> 
> 使用两个锁防止读取数据错误
> 
> 使用volatile修饰单例对象，防止JVM的指令重排序导致读取空对象
> 
> 1.创建内存
> 
> 2.将对象初始化
> 
> 3.将对象指针指向内存

```java
public class Singleton{
    private Singleton(){}
    private volatile static Singleton singleton = null;
    
    public static Singleton getInstance(){
        if(singleton==null){
            synchronized(Singleton.class){
                if(singleton==null){
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}
```



> 静态内部类懒汉式
> 
> 线程安全

```java
public class Singleton(){
    private Singleton(){}
    private class LazyLoader(){
        private static final Singleton singleton = new Singleton();
    }

    public static Singleton getInstance(){
        return LazyLoader.singleton;
    }
}
```



> 饿汉式
> 
> 在调用的时候便已经创建

```java
public class Singleton{
    private Singleton(){}
    private static final Singleton singleton = new Singleton();
    public static Singleton getInstance(){
        return singleton;
    }
}
```


