# 代理模式



> 静态代理

```java
//售票接口
public interface SellTickets{
    void sell();
}

//火车站
public class TrainStation implements SellTickets{
    public TrainStation(){}
    public void sell(){
        System.out.println("火车站售票点")
    }
}

//代理类Proxy
public class Proxy implements SellTickets{
    private TrainStation station = new TrainStation();
    public Proxy(){}

    public void sell(){
        System.out.println("代售点收取服务费(静态)");
        station.sell();
    }
}
```

> JDK动态代理
>
> 在程序运行时，会调用Proxy.newProxyInstance方法动态创建一个$Proxy0的对象，
>
> 这个对象会构造一个InvocationHandler对象，并且将方法中传递的InvocationHandler传入动态类中
>
> 这个动态类则会调用其中的invoke方法
>
> ----------------------------------------------------
>
> 代理执行流程：
>
> 1.在测试类中通过代理对象调用sell()方法
>
> 2.根据多态的特性，执行了代理类($Proxy0)中的sell()方法
>
> 3.$Proxy0中的sell()方法又调用了InvocationHandler接口的子实现类的invoke()方法
>
> 4.invoke方法通过反射执行了真实对象火车站类中的sell()方法

```java
//售票接口
public interface SellTickets{
    void sell();
}

//火车站
public class TrainStation implements SellTickets{
    public TrainStation(){}
    public void sell(){
        System.out.println("火车站售票点")
    }
}

//代理工厂
public class ProxyFactory{
    private TrainStation station = new TrainStation();
    public ProxyFactory(){}

    public SellTickets getProxyObject(){
        //返回代理对象
        /**
         * ClassLoader loader,     :类加载器，用于加载被代理类，
         * Class<?>[] interfaces,   :被代理类实现的接口的字节码对象
         * InvocationHandler h      :被代理对象的调用处理程序
         */
        SellTickets proxyObject = (SellTickets) Proxy.newProxyInstance(
                station.getClass().getClassLoader(),
                station.getClass().getInterfaces(),
                new InvocationHandler() {
                    /**
                     * @param proxy     被代理对象，在invoke中基本不同
                     * @param method    接口实现的方法封装成Method
                     * @param args      调用方法的实际参数
                     * @return  返回值则是接口方法的返回值
                     */
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        //System.out.println("invoke方法执行了...");
                        //增强方法
                        System.out.println("代售点收取服务费(JDK动态代理)");
                        //执行被代理对象的方法
                        Object obj = method.invoke(station, args);
                        return obj;
                    }
                }
        );
        return proxyObject;
    }
}
```

> CGLib代理

```java
//没有售票接口
//火车站类
public class TrainStation{
    public TrainStation(){}
    public void sell(){
        System.out.println("火车站售票点")
    }
}
//使用到cglib包中的enhancer对象
//类似于JDK代理中的Proxy对象
public class ProxyFactory implements MethodInterceptor{
    private TrainStation station = new TrainStation();
    public ProxyFactory(){}
    
    //返回需要被代理的类
    public TrainStation getProxyObject(){
        Enhancer enhancer = new Enhancer();
        //将被代理类的字节码文件设置进enhancer
        enhancer.setSuperClass(TrainStation.class);
        //设置回调函数
        enhancer.setCallback(this);
        //创建代理对象
        //需要强制类型转换，因为返回的是Object对象
        TrainStation proxyObject = (TrainStation) enhancer.create();
    }
    
    //拦截器
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        //System.out.println("intercept方法执行了");
        //增强方法
        System.out.println("代售点收取费用(CGLib)");
        //调用目标方法
        Object obj = method.invoke(station, objects);
        return obj;
    }
}
```

---

> CGLib不能对声明为final的类或方法进行代理，因为CGLib原理是动态生成被代理类的子类
>
> 如果有接口使用JDK代理，没有接口使用CGLib代理
>
> 动态代理在接口方法比较多的时候可以灵活处理，而不是像静态代理那样为每个方法进行操作
>
> -
>
> 优点：
>
> - 代理模式在客户端与目标对象之间起到一个中介作用的保护
> - 代理对象可以扩展目标对象的功能
> - 代理模式能将客户端与目标对象分离，在一定程度 上降低了系统的耦合度
>
> 缺点：
>
> - 增加了系统的复杂度
