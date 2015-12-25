[TOC]
# Rxjava笔记

##学习资料
- [入门资料:](https://github.com/lzyzsd/Awesome-RxJava)https://github.com/lzyzsd/Awesome-RxJava
- [知乎RxJava和EventBus的区别？:](http://www.zhihu.com/question/32179258/answer/54989242)http://www.zhihu.com/question/32179258/answer/54989242
- [Lambda表达入门](http://blog.csdn.net/renfufei/article/details/24600507)

## 预备知识
- androdi studio 貌似还不能支持java 8的语法 [工具的配置](http://stackoverflow.com/questions/23318109/is-it-possible-to-use-java-8-for-android-development)
- [retrolambda—支持lambda的配置](https://github.com/evant/gradle-retrolambda)

#### Lambda表达 
> Lambda表达是java8新特性，取代java中匿名内部类的冗长的写法

- 基本语法`(parameters) -> expression`或`(parameters) ->{ statements; }`
- 基本语法例子：

```java
// 1. 不需要参数,返回值为 5
() -> 5

// 2. 接收一个参数(数字类型),返回其2倍的值
x -> 2 * x

// 3. 接受2个参数(数字),并返回他们的差值
(x, y) -> x – y

// 4. 接收2个int型整数,返回他们的和
(int x, int y) -> x + y

// 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)
(String s) -> System.out.print(s)
```


- 例子

```java
File dir = new File("/an/dir/");
FileFilter directoryFilter = new FileFilter() {
  public boolean accept(File file) {
     return file.isDirectory();
  }
};
```

使用Lambda的写法

```java
File dir = new File("/an/dir/");
FileFilter directoryFilter = (File f) -> f.isDirectory();
File[] dirs = dir.listFiles(directoryFilter);
//进一步简化
File dir = new File("/an/dir/");
File[] dirs = dir.listFiles((File f) -> f.isDirectory());
```
>编译器知道FileFilter只有一个方法accept()，所以accept()方法肯定对应(File f) -> f.isDirectory()
而且accept()方法只有一个File类型的参数，所以(File f) -> f.isDirectory()中的File f就是这个参数了

因为android studio不支持就不了解太多入门地址: http://blog.csdn.net/renfufei/article/details/24600507

***

## RxAndroid

### 什么是响应式编程？
>响应式编程就是与异步数据流交互的编程范式(事件流在Rx里的术语是叫”被观察者”)。点击事件本质上就是一个异步事件流(asynchronous event stream)，这样你就可以观察它的变化并使其做出一些反应(do some side effects);任何东西都可以成为一个数据流，
例如变量、用户输入、属性、缓存、数据结构等等


#### 命令式编程
>命令“机器”如何去做事情(how)，这样不管你想要的是什么(what)，它都会按照你的命令实现。

#### 声明式编程
>告诉“机器”你想要的是什么(what)，让机器想出如何去做(how)。

例子:让一个数组里的数值翻倍

1 命令式编程风格实现

```javascript
var numbers = [1,2,3,4,5]
var doubled = []
for(var i = 0; i < numbers.length; i++) {
  var newNumber = numbers[i] * 2
  doubled.push(newNumber)
 
}
console.log(doubled) 
//=> [2,4,6,8,10]
```

2 声明式编程风格

```javascript
var numbers = [1,2,3,4,5]
var doubled = numbers.map(function(n) {
  return n * 2
})
console.log(doubled) 
//=> [2,4,6,8,10]
```

- SQL是声明式编程语言



### 操作符

#### Creating Observables(Observable的创建操作符)

##### create操作符
create操作符是所有创建型操作符的“根”，也就是说其他创建型操作符最后都是通过create操作符来创建
![](http://reactivex.io/documentation/operators/images/create.png)

```java
Observable.create(new Observable.OnSubscribe<Integer>() {
    @Override
    public void call(Subscriber<? super Integer> observer) {
        try {
            if (!observer.isUnsubscribed()) {
                for (int i = 1; i < 5; i++) {
                    observer.onNext(i);
                }
                observer.onCompleted();
            }
        } catch (Exception e) {
            observer.onError(e);
        }
    }
 } ).subscribe(new Subscriber<Integer>() {
        @Override
        public void onNext(Integer item) {
            System.out.println("Next: " + item);
        }

        @Override
        public void onError(Throwable error) {
            System.err.println("Error: " + error.getMessage());
        }

        @Override
        public void onCompleted() {
            System.out.println("Sequence complete.");
        }
    });
```
>结果：
Next: 1 
Next: 2 
Next: 3 
Next: 4 
Sequence complete.

##### from操作符
from操作符是把其他类型的对象和数据类型转化成Observable，其流程图例如下： 
![](http://reactivex.io/documentation/operators/images/from.c.png)

```java
Integer[] items = { 0, 1, 2, 3, 4, 5 };
Observable myObservable = Observable.from(items);

myObservable.subscribe(
    new Action1<Integer>() {
        @Override
        public void call(Integer item) {
            System.out.println(item);
        }
    },
    new Action1<Throwable>() {
        @Override
        public void call(Throwable error) {
            System.out.println("Error encountered: " + error.getMessage());
        }
    },
    new Action0() {
        @Override
        public void call() {
            System.out.println("Sequence complete");
        }
    }
);
```
>0 
1 
2 
3 
4 
5 
Sequence complete

##### just操作符
just操作符也是把其他类型的对象和数据类型转化成Observable，它和from操作符很像，只是方法的参数有所差别，其流程图例如下： 
![](http://reactivex.io/documentation/operators/images/just.c.png)

```java
Observable.just(1, 2, 3)
          .subscribe(new Subscriber<Integer>() {
        @Override
        public void onNext(Integer item) {
            System.out.println("Next: " + item);
        }

        @Override
        public void onError(Throwable error) {
            System.err.println("Error: " + error.getMessage());
        }

        @Override
        public void onCompleted() {
            System.out.println("Sequence complete.");
        }
    });
```
>Next: 1 
Next: 2 
Next: 3 
Sequence complete.

defer操作符
defer操作符是直到有订阅者订阅时，才通过Observable的工厂方法创建Observable并执行，defer操作符能够保证Observable的状态是最新的，其流程实例如下
![](http://reactivex.io/documentation/operators/images/defer.c.png)
下面通过比较defer操作符和just操作符的运行结果作比较：

```java
        i=10;
        Observable justObservable = Observable.just(i);
        i=12;
        Observable deferObservable = Observable.defer(new Func0<Observable<Object>>() {
            @Override
            public Observable<Object> call() {
                return Observable.just(i);
            }
        });
        i=15;

        justObservable.subscribe(new Subscriber() {
            @Override
            public void onCompleted() {

            }

            @Override
            public void onError(Throwable e) {

            }

            @Override
            public void onNext(Object o) {
                System.out.println("just result:" + o.toString());
            }
        });

        deferObservable.subscribe(new Subscriber() {
            @Override
            public void onCompleted() {

            }

            @Override
            public void onError(Throwable e) {

            }

            @Override
            public void onNext(Object o) {
                System.out.println("defer result:" + o.toString());
            }
        });
   }
```
>其中i是类的成员变量，运行结果如下： 
just result:10 
defer result:15
可以看到，just操作符是在创建Observable就进行了赋值操作，而defer是在订阅者订阅时才创建Observable，此时才进行真正的赋值操作

##### timer操作符
一段时间产生一个数字，然后就结束，可以理解为延迟产生数字，其流程实例如下： 
![](http://reactivex.io/documentation/operators/images/timer.png)
timer操作符默认情况下是运行在一个新线程上的,通过observeOn指定线程

```java
 Observable.timer(200, TimeUnit.MICROSECONDS).observeOn(AndroidSchedulers.mainThread())
                .subscribe(new Subscriber<Long>() {
                    @Override
                    public void onCompleted() {
                        System.out.println("Sequence complete.");
                    }

                    @Override
                    public void onError(Throwable e) {
                        System.out.println("error:" + e.getMessage());
                    }

                    @Override
                    public void onNext(Long aLong) {
                        System.out.println("Next:" + aLong.toString());
                    }
                });
```
>结果  Next:0  Sequence complete.

##### interval操作符
interval操作符是每隔一段时间就产生一个数字，这些数字从0开始，一次递增1直至无穷大；interval操作符的实现效果跟上面的timer操作符的第二种情形一样。以下是流程实例： 
![](http://reactivex.io/documentation/operators/images/interval.png)
interval操作符默认情况下是运行在一个新线程上的，当然你可以通过传入参数来修改其运行的线程。

##### range操作符
range操作符是创建一组在从n开始，个数为m的连续数字，比如range(3,10)，就是创建3、4、5…12的一组数字，其流程实例如下： 
![](http://reactivex.io/documentation/operators/images/range.png)

```java
//产生从3开始，个数为10个的连续数字
        Observable.range(3,10).subscribe(new Subscriber<Integer>() {
            @Override
            public void onCompleted() {
                System.out.println("Sequence complete.");
            }

            @Override
            public void onError(Throwable e) {
                System.out.println("error:" + e.getMessage());
            }

            @Override
            public void onNext(Integer i) {
                System.out.println("Next:" + i.toString());
            }
        });
```
>运行结果如下： 
Next:3 
Next:4 
Next:5 
Next:6 
…. 
Next:12 
Sequence complete.

##### repeat/repeatWhen操作符
repeat操作符是对某一个Observable，重复产生多次结果，其流程实例如下： 
![](http://reactivex.io/documentation/operators/images/repeat.o.png)

```java
//连续产生两组(3,4,5)的数字
        Observable.range(3,5).repeat(2).subscribe(new Subscriber<Integer>() {
            @Override
            public void onCompleted() {
                System.out.println("Sequence complete.");
            }

            @Override
            public void onError(Throwable e) {
                System.out.println("error:" + e.getMessage());
            }

            @Override
            public void onNext(Integer i) {
                System.out.println("Next:" + i.toString());
            }
        });
```
>Next:3 
Next:4 
Next:5 
Next:3 
Next:4 
Next:5 
Sequence complete.

repeatWhen操作符是对某一个Observable，有条件地重新订阅从而产生多次结果，其流程实例如下： 
![](http://reactivex.io/documentation/operators/images/repeatWhen.f.png)
repeat和repeatWhen操作符__默认情况下是运行在一个新线程上的__，当然你可以通过传入参数来修改其运行的线程。

```java
Observable.just(1,2,3).repeatWhen(new Func1<Observable<? extends Void>, Observable<?>>() {
            @Override
            public Observable<?> call(Observable<? extends Void> observable) {
                //重复3次
                return observable.zipWith(Observable.range(1, 3), new Func2<Void, Integer, Integer>() {
                    @Override
                    public Integer call(Void aVoid, Integer integer) {
                        return integer;
                    }
                }).flatMap(new Func1<Integer, Observable<?>>() {
                    @Override
                    public Observable<?> call(Integer integer) {
                        System.out.println("delay repeat the " + integer + " count");
                        //1秒钟重复一次
                        return Observable.timer(1, TimeUnit.SECONDS);
                    }
                });
            }
        }).subscribe(new Subscriber<Integer>() {
            @Override
            public void onCompleted() {
                System.out.println("Sequence complete.");
            }

            @Override
            public void onError(Throwable e) {
                System.err.println("Error: " + e.getMessage());
            }

            @Override
            public void onNext(Integer value) {
                System.out.println("Next:" + value);
            }
        });
```
>运行结果如下： 
Next:1 
Next:2 
Next:3 
repeat the 1 count 
Next:1 
Next:2 
Next:3 
repeat the 2 count 
Next:1 
Next:2 
Next:3 
repeat the 3 count 
Next:1 
Next:2 
Next:3 
Sequence complete.


####  Transforming Observables(Observable的转换操作符)

##### buffer操作符
Buffer操作符所要做的事情就是将数据安装规定的大小做一下缓存，然后将缓存的数据作为一个集合发射出去,旦源Observable在产生结果的过程中出现异常，即使buffer已经存在收集到的结果，订阅者也会马上收到这个异常，并结束整个过程。
![](http://reactivex.io/documentation/operators/images/buffer3.png)
![](http://reactivex.io/documentation/operators/images/buffer4.png)
第一张示例图中我们指定buffer的大小为3，收集到3个数据后就发射出去，第二张图中我们加入了一个skip参数用来指定每次发射一个集合需要跳过几个数据，图中如何指定count为2，skip为3，就会每3个数据发射一个包含两个数据的集合，如果count==skip的话，我们就会发现其等效于第一种情况了。

 >buffer不仅仅可以通过数量规则来缓存，还可以通过时间等规则来缓存，如规定3秒钟缓存发射一次等，见下面代码，我们创建了两个Observable，并使用buffer对其进行转化，第一个通过数量来缓存，第二个通过时间来缓存。

```java
Observable<List<Integer>> buffer = Observable.just(1, 2, 3, 4, 5, 6, 7, 8, 9)
                .buffer(2, 3);
        //interval从0开始到无穷大
        Observable<List<Long>> bufferTimer = Observable.interval(100, TimeUnit.MICROSECONDS)
                //把上面产生的数字内容缓存到列表中，并每隔300毫秒秒通知订阅者
                .buffer(300, TimeUnit.MILLISECONDS)
                .observeOn(AndroidSchedulers.mainThread());

        buffer.subscribe(new Subscriber<List<Integer>>() {
            @Override
            public void onCompleted() {
                System.out.println("Sequence complete.");
            }

            @Override
            public void onError(Throwable e) {
                System.err.println("Error: " + e.getMessage());
            }

            @Override
            public void onNext(List<Integer> value) {
                System.out.println("Next:" + value);
            }
        });

        bufferTimer.subscribe(new Action1<List<Long>>() {
            @Override
            public void call(List<Long> longs) {
                System.out.println("bufferTimer:" + longs);
            }
        });

```
> Next:[1, 2]
 Next:[4, 5]
 Next:[7, 8]

> bufferTimer:[0, 1, 2, 3, 4, 5, 6, 7,....]
> bufferTimer:[20, 21, 22, 23, 24, 25, 26,....] ....

##### FlatMap操作符



#### map操作符,转换执行结果，将事件序列中的对象或整个序列进行加工处理，转换成不同的事件或事件序
将String装换成Bitmap 

```java
Observable.just("images/logo.png") // 输入类型 String
    .map(new Func1<String, Bitmap>() {
        @Override
        public Bitmap call(String filePath) { // 参数类型 String
            return getBitmapFromPath(filePath); // 返回类型 Bitmap
        }
    })
    .subscribe(new Action1<Bitmap>() {
        @Override
        public void call(Bitmap bitmap) { // 参数类型 Bitmap
            showBitmap(bitmap);
        }
    });
```

#### timer操作符

```java
Observable.timer(1, TimeUnit.SECONDS, AndroidSchedulers.mainThread())
                    .map(new Func1<Long, Object>() {
                        @Override
                        public Object call(Long aLong) {
                            startActivity(new Intent(mContext, LoginActivity.class));
                            finish();
                            return null;
                        }
                    })
                    .subscribe();
```

#### mergeDelayError 合并几个不同的Observable
#### sample的意思是每隔一段时间就进行采样,在时间间隔范围内获取最后一个发布的Observable
#### latMap的意思是把某一个Observable转换成另一个Observable
```java
//在欢迎页停留2秒后，要跳转到活动内容的页面(也就是一张广告的图片)上，然后在活动内容的页面停留2-3秒，再跳转到主页面；
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_welcome);

        ImageView view = (ImageView) findViewById(R.id.iv_welcome);
        view.setImageResource(R.mipmap.welcome);

        Observable.mergeDelayError(
                //在新线程中加载本地缓存图片
                loadBitmapFromLocal().subscribeOn(Schedulers.io()),
                //在新线程中加载网络图片
                loadBitmapFromNet().subscribeOn(Schedulers.newThread()),
                Observable.timer(3,TimeUnit.SECONDS).map(c->null))
                //每隔2秒获取加载数据
                .sample(2, TimeUnit.SECONDS, AndroidSchedulers.mainThread())
                .flatMap(r->{
                    if(r==null)  //如果没有获取到图片，直接跳转到主页面
                        return Observable.empty();
                    else { //如果获取到图片，则停留2秒再跳转到主页面
                        view.setImageDrawable(r);
                        return Observable.timer(2, TimeUnit.SECONDS);
                    }
                }).subscribe(
                r->{
                },
                e->{
                    startActivity(new Intent(WelcomeActivity.this, MainActivity.class));
                    finish();
                },
                ()->{
                    startActivity(new Intent(WelcomeActivity.this, MainActivity.class));
                    finish();
                }
        );
    }
```

```java
Creating Observables(Observable的创建操作符)，比如：Observable.create()、Observable.just()、Observable.from()等等；
Transforming Observables(Observable的转换操作符)，比如：observable.map()、observable.flatMap()、observable.buffer()等等；
Filtering Observables(Observable的过滤操作符)，比如：observable.filter()、observable.sample()、observable.take()等等；
Combining Observables(Observable的组合操作符)，比如：observable.join()、observable.merge()、observable.combineLatest()等等；
Error Handling Operators(Observable的错误处理操作符)，比如:observable.onErrorResumeNext()、observable.retry()等等；
Observable Utility Operators(Observable的功能性操作符)，比如：observable.subscribeOn()、observable.observeOn()、observable.delay()等等；
Conditional and Boolean Operators(Observable的条件操作符)，比如：observable.amb()、observable.contains()、observable.skipUntil()等等；
Mathematical and Aggregate Operators(Observable数学运算及聚合操作符)，比如：observable.count()、observable.reduce()、observable.concat()等等；
其他如observable.toList()、observable.connect()、observable.publish()等等；
```