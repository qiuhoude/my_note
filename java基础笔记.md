# java基础笔记

### 动态代理
1. 在某些情况下，我们不希望或是不能直接访问对象 A，而是通过访问一个中介对象 B，由 B 去访问 A 达成目的，这种方式我们就称为代理。这里对象 A 所属类我们称为委托类，也称为被代理类，对象 B 所属类称为代理类。
2. 优点: 隐藏委托类的实现;解耦，不改变委托类代码情况下做一些额外处理，比如添加初始判断及其他公共操作
3. 根据程序运行前代理类是否已经存在，可以将代理分为静态代理(有点类似与装饰设计模式,不同的地方在于装饰设计模式需要继承或实现同一父类)和动态代理。
4. 步骤：1 新建委托类；2 实现`InvocationHandler`接口，这是负责连接代理类和委托类的中间类必须实现的接口;3 通过`Proxy.newProxyInstance()`类新建代理类对象。

例子:计某个类所有函数的执行时间

```java
public interface Operate {
    public void operateMethod1();

    public void operateMethod2();

    public void operateMethod3();
}

ublic class OperateImpl implements Operate {

    @Override
    public void operateMethod1() {
        System.out.println("Invoke operateMethod1");
        sleep(110);
    }

    @Override
    public void operateMethod2() {
        System.out.println("Invoke operateMethod2");
        sleep(120);
    }

    @Override
    public void operateMethod3() {
        System.out.println("Invoke operateMethod3");
        sleep(130);
    }

    private static void sleep(long millSeconds) {
        try {
            Thread.sleep(millSeconds);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

/**
 * InvocationHandler是负责连接代理类和委托类的中间类必须实现的接口
 */
public class TimingInvocationHandler implements InvocationHandler {

    //target属性表示委托类对象
    private Object target;

    public TimingInvocationHandler() {
    }

    public TimingInvocationHandler(Object target) {
        this.target = target;
    }

    /**
     * @param proxy  通过 Proxy.newProxyInstance() 生成的代理类对象。
     * @param method 表示代理对象被调用的函数。
     * @param args   表示代理对象被调用的函数的参数。
     * @return
     * @throws Throwable
     */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        long start = System.currentTimeMillis();
        Object obj = method.invoke(target, args);
        long end = System.currentTimeMillis();
        System.out.println(method.getName() + "cost time is " + (end - start));
        return obj;
    }

}

 public static void main(String[] args) {
        //create proxy instance
        //将委托类对象new OperateImpl()作为TimingInvocationHandler构造函数入参创建timingInvocationHandler对象
        TimingInvocationHandler timingInvocationHandler = new TimingInvocationHandler(new OperateImpl());
        //创建代理对象,代理类就是在这时候动态生成的,调用代理对象的方法就会调用TimingInvocationHandler的invoke方法
        Operate operate = (Operate) Proxy.newProxyInstance(OperateImpl.class.getClassLoader(),
                new Class[]{Operate.class}, timingInvocationHandler);
        //call method of proxy instance
        operate.operateMethod1();
        System.out.println();
        operate.operateMethod2();
        System.out.println();
        operate.operateMethod3();
    }
```
