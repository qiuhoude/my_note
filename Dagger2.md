
# Dagger2的学习

dagger2 构造实体是依赖`javax.inject.Inject`annotation(注解)

### @Inject 
- 使用`@Inject`注解用于修饰构造器，可以创建出这个class的实体,同时会获取必要的parameters,并且会调用构造器

```java
class Thermosiphon implements Pump {
  private final Heater heater;

  @Inject
  Thermosiphon(Heater heater) {
    this.heater = heater;
  }

  ...
}
```

- 还可以直接注入字段(fields),在本例中可以获取 `Heater`和`Pump`的实体

```java
class CoffeeMaker {
  @Inject Heater heater;
  @Inject Pump pump;

  ...
}
```

- 如果class中fields有`@Inject`修饰，但是`constructor`没有,dagger会将对象注入到字段中，但是不会创建新的实体;如果需要创建新的对象,就在无参数的构造器上面加上`@Inject`

- `@Inject`不起作用的地方
	- 接口不能被构造
	- 第三方的classes没有注解
	- Configurable objects must be configured!

### @Provides
- 所有的`@Provides`都必须属于`@Module`
- 主要使用在methods上

### Building the Graph
把`@Inject`和`@Provides`的类构成对象图,连接他们的依赖。有点类似于在main方中把所需要的class进 new出来一样。  

interface中methods是不能有参数,返回是所希望的类型,通过使用`@Component`来修饰interface,和module参数类型，
dagger2会完全实现定义的协议  

实现类的前缀是Dagger,后面和接口名称相同，  
获取对象使用`builder()`会返回`builder`对象set依赖,然后调用`build()`来创建实体  

每一个module都有默认的构造器省略，但builder会自动的创建，如果所有的依赖都在构造器中创建,可以是用`create()`


### @Singleton

```java
@Provides @Singleton Heater provideHeater() {
  return new ElectricHeater();
}
```

provideHeater()只会创建一个ElectricHeater对象  

如果修饰在class上面,在内存只有一个实体，并且可以共享到多个线程中。

### Lazy<T> 
只有在第一次调用lazy.get(),才会创建T对象;  
如果T是一个单利,在ObjectGraph中lazy.get()会是相同的实体  
如果非单利,每次get都是获取自己的对象。

```java
 private final Lazy<Heater> heater; // Create a possibly costly heater only when we use it.
    private final Pump pump;

    @Inject
    CoffeeMaker(Lazy<Heater> heater, Pump pump) {
        this.heater = heater;
        this.pump = pump;
    }
```

### Provider<T>

### @Qualifier
有时，类型不足以确认依赖,就要是用`@Qualifier`进描述

```java
//跟inject一起使用
class ExpensiveCoffeeMaker {
  @Inject @Named("water") Heater waterHeater;
  @Inject @Named("hot plate") Heater hotPlateHeater;
  ...
}
//跟provider一起使用
@Provides @Named("hot plate") Heater provideHotPlateHeater() {
  return new ElectricHeater(70);
}

@Provides @Named("water") Heater provideWaterHeater() {
  return new ElectricHeater(93);
}
```

