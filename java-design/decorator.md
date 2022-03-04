# 装饰者模式

> 优点：
> 
>           装饰类和被装饰类可以独立发展，不会相互耦合，
> 
>           装饰模式是继承的一个替代模式 ，可以动态扩展一个类的功能

---

> 应用场景：
> 
>     1.当不能采用继承的方式对系统进行扩充或者采用继承不利于系统扩展和维护时
> 
>             1.1系统中存在大量独立扩展，为支持每种组合将产生大量的类
> 
>             1.2因为类定义不能继承（final类）
> 
>     2.在不影响其他对象的情况下，以动态，透明的方式给单个对象添加功能
> 
>     3.当对象的功能要求可以动态的添加或者撤销

---

> JDK中的IO流的包装类使用到了装饰者模式
> 
> BufferedInputStream,BufferedOutputStrem,BufferWriter,BufferReader

---

> 设计一个装饰者模式
> 
> 我的理解：
> 
>      假如被装饰者是游戏里的一个武器类，武器的大种类有剑和弓
> 
>     并且每把武器有名字和攻击力。

```java
public abstract Equipment{
    private String desc;
    private float damage;
    public Equipment(String desc,float damage){
        this.desc = desc;
        this.damage = damage;
    }

    //setter and getter...
    
    public abstract float totalDamage();
}
```

```java
public SwordEquipment extends Equipment{
    public SwordEquipment(String desc,float damage){
        super(desc,damage);
    }

    public float totalDamage(){
        return getDamage();
    }
}
```

```java
public BowEquipment extends Equipment{
    public BowEquipment(String desc,float damage){
        super(desc,damage);
    }

    public float totalDamage(){
        return getDamage();
    }
}
```

> 装饰者类则需要继承该武器类，并且聚合该武器

```java
public abstract Garnish extends Equipment{
    private Equipment e;
    //前缀和元素的种类及伤害从另外的随机数获得
    public Garnish(Equipment e,String desc,float damage){
        super(desc+e.getDesc(),damage+e.getDamage());
        this.e = e;
    }
}
```

> 为武器单独添加前缀，和元素

```java
public Prefix extends Garnish{
    public Prefix(Equipment e,String desc,float damage){
        super(e,desc,damage);
    }

    @Override
    public float totalDamage(){
        return getDamage();
    }
}
```

---

> 静态代理和装饰者模式的区别
> 
> - 相同点
>   
>   - 都要实现与目标类相同的业务接口
>   
>   - 在两个类中都要声明目标对象
>   
>   - 都可以在不修改目标类的前提下增强目标方法
> 
> - 不同点
>   
>   - 目的不同
>     
>     装饰者是为了增强目标对象
>     
>     静态代理是为了保护和隐藏目标对象
>   
>   - 获取目标对象构建的地方不同
>     
>     装饰者是由外界传递进来，可以通过构造方法构造
>     
>     静态代理是在代理类内部创建，以此来隐藏目标对象


