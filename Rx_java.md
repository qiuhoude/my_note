
<!-- MarkdownTOC -->

- [Rxjava笔记](#rxjava笔记)
    - [习资料](#习资料)
    - [预备知识](#预备知识)
        - [Lambda表达](#lambda表达)
    - [RxAndroid](#rxandroid)
        - [什么是响应式编程？](#什么是响应式编程？)
            - [命令式编程](#命令式编程)
            - [声明式编程](#声明式编程)
        - [操作符](#操作符)
            - [Creating Observables(Observable的创建操作符)](#creating-observablesobservable的创建操作符)
                - [create操作符](#create操作符)
                - [from操作符](#from操作符)
                - [just操作符](#just操作符)
                - [timer操作符](#timer操作符)
                - [interval操作符](#interval操作符)
                - [range操作符](#range操作符)
                - [repeat/repeatWhen操作符](#repeatrepeatwhen操作符)
            - [ Transforming Observables(Observable的转换操作符)](#-transforming-observablesobservable的转换操作符)
                - [buffer操作符](#buffer操作符)
                - [FlatMap操作符](#flatmap操作符)
                - [concatMap操作符](#concatmap操作符)
                - [switchMap操作符](#switchmap操作符)
                - [groupBy操作符](#groupby操作符)
                - [groupBy操作符](#groupby操作符-1)
                - [map操作符](#map操作符)
                - [cast操作符](#cast操作符)
                - [scan操作符(和Python中的reduce很类似)](#scan操作符和python中的reduce很类似)
                - [window操作符](#window操作符)
            - [Filtering Observables(Observable的过滤操作符)](#filtering-observablesobservable的过滤操作符)
                - [ debounce操作符](#-debounce操作符)
                - [distinct操作符](#distinct操作符)
                - [elementAt操作符](#elementat操作符)
                - [filter操作符](#filter操作符)
                - [ofType操作符](#oftype操作符)
                - [first操作符](#first操作符)
                - [single操作符](#single操作符)
                - [last操作符](#last操作符)
                - [ignoreElements操作符](#ignoreelements操作符)
                - [sample操作符](#sample操作符)
                - [skip操作符](#skip操作符)
                - [skipLast操作符](#skiplast操作符)
                - [take操作符](#take操作符)
                - [takeFirst操作符](#takefirst操作符)
                - [takeLast操作符](#takelast操作符)
            - [Combining Observables(Observable的组合操作符)](#combining-observablesobservable的组合操作符)
                - [combineLatest操作符](#combinelatest操作符)
                - [join操作符](#join操作符)
                - [groupJoin操作符](#groupjoin操作符)
                - [merge操作符](#merge操作符)
                - [mergeDelayError操作符](#mergedelayerror操作符)
                - [startWith操作符](#startwith操作符)
                - [switchOnNext操作符](#switchonnext操作符)
                - [zip操作符](#zip操作符)
            - [Error Handling Operators(Observable的错误处理操作符)](#error-handling-operatorsobservable的错误处理操作符)
                - [ onErrorReturn操作符](#-onerrorreturn操作符)
                - [onErrorResumeNext操作符](#onerrorresumenext操作符)
                - [onExceptionResumeNext操作符](#onexceptionresumenext操作符)
                - [retry操作符](#retry操作符)
                - [retryWhen操作符](#retrywhen操作符)
            - [Observable Utility Operators(Observable的功能性操作符)](#observable-utility-operatorsobservable的功能性操作符)
                - [Delay操作符](#delay操作符)
                - [Do操作符](#do操作符)
                - [Meterialize操作符](#meterialize操作符)
                - [SubscribOn/ObserverOn操作符](#subscribonobserveron操作符)
                - [TimeInterval\TimeStamp操作符](#timeinterval\timestamp操作符)
                - [Timeout操作符](#timeout操作符)
                - [Using操作符](#using操作符)
            - [Conditional and Boolean Operators(Observable的条件操作符)](#conditional-and-boolean-operatorsobservable的条件操作符)
                - [All操作符](#all操作符)
                - [Contains、IsEmptyAll操作符](#contains、isemptyall操作符)
                - [DefaultIfEmpty操作符](#defaultifempty操作符)
                - [SequenceEqual操作符](#sequenceequal操作符)
                - [SkipUntil、SkipWhile操作符](#skipuntil、skipwhile操作符)
                - [TakeUntil、TakeWhile操作符](#takeuntil、takewhile操作符)
            - [Mathematical and Aggregate Operators(Observable数学运算及聚合操作符)](#mathematical-and-aggregate-operatorsobservable数学运算及聚合操作符)
                - [Concat操作符](#concat操作符)
                - [Count操作符](#count操作符)
                - [Reduce、Collect操作符](#reduce、collect操作符)
            - [map操作符,转换执行结果，将事件序列中的对象或整个序列进行加工处理，转换成不同的事件或事件序](#map操作符转换执行结果，将事件序列中的对象或整个序列进行加工处理，转换成不同的事件或事件序)

<!-- /MarkdownTOC -->

<a name="rxjava笔记"></a>
# Rxjava笔记

<a name="习资料"></a>
##学习资料
- [入门资料:](https://github.com/lzyzsd/Awesome-RxJava)https://github.com/lzyzsd/Awesome-RxJava
- [知乎RxJava和EventBus的区别？:](http://www.zhihu.com/question/32179258/answer/54989242)http://www.zhihu.com/question/32179258/answer/54989242
- [Lambda表达入门](http://blog.csdn.net/renfufei/article/details/24600507)

<a name="预备知识"></a>
## 预备知识
- androdi studio 貌似还不能支持java 8的语法 [工具的配置](http://stackoverflow.com/questions/23318109/is-it-possible-to-use-java-8-for-android-development)
- [retrolambda—支持lambda的配置](https://github.com/evant/gradle-retrolambda)

<a name="lambda表达"></a>
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

<a name="rxandroid"></a>
## RxAndroid

<a name="什么是响应式编程？"></a>
### 什么是响应式编程？
>响应式编程就是与异步数据流交互的编程范式(事件流在Rx里的术语是叫”被观察者”)。点击事件本质上就是一个异步事件流(asynchronous event stream)，这样你就可以观察它的变化并使其做出一些反应(do some side effects);任何东西都可以成为一个数据流，
例如变量、用户输入、属性、缓存、数据结构等等


<a name="命令式编程"></a>
#### 命令式编程
>命令“机器”如何去做事情(how)，这样不管你想要的是什么(what)，它都会按照你的命令实现。

<a name="声明式编程"></a>
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



<a name="操作符"></a>
### 操作符

<a name="creating-observablesobservable的创建操作符"></a>
#### Creating Observables(Observable的创建操作符)

<a name="create操作符"></a>
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

<a name="from操作符"></a>
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

<a name="just操作符"></a>
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

<a name="timer操作符"></a>
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

<a name="interval操作符"></a>
##### interval操作符
interval操作符是每隔一段时间就产生一个数字，这些数字从0开始，一次递增1直至无穷大；interval操作符的实现效果跟上面的timer操作符的第二种情形一样。以下是流程实例： 
![](http://reactivex.io/documentation/operators/images/interval.png)
interval操作符默认情况下是运行在一个新线程上的，当然你可以通过传入参数来修改其运行的线程。

<a name="range操作符"></a>
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

<a name="repeatrepeatwhen操作符"></a>
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


<a name="-transforming-observablesobservable的转换操作符"></a>
####  Transforming Observables(Observable的转换操作符)

<a name="buffer操作符"></a>
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

<a name="flatmap操作符"></a>
##### FlatMap操作符
flatMap操作符是把Observable产生的结果转换成多个Observable，然后把这多个Observable“扁平化”成一个Observable，并依次提交产生的结果给订阅者。
flatMap操作符通过传入一个函数作为参数转换源Observable，在这个函数中，你可以自定义转换规则，最后在这个函数中返回一个新的Observable，然后flatMap操作符通过合并这些Observable结果成一个Observable，并依次提交结果给订阅者。

值得注意的是，flatMap操作符在合并Observable结果时，有可能存在交叉的情况，如下流程图所示：
![](http://reactivex.io/documentation/operators/images/mergeMap.png)

```java
private Observable<File> listFiles(File f){
        if(f.isDirectory()){
            return Observable.from(f.listFiles()).flatMap(new Func1<File, Observable<File>>() {
                @Override
                public Observable<File> call(File file) {
                    return listFiles(f);
                }
            });
        } else {
            return Observable.just(f);
        }
    }


    @Override
    public void onClick(View v) {
        Observable.just(getApplicationContext().getExternalCacheDir())
                .flatMap(new Func1<File, Observable<File>>() {
                    @Override
                    public Observable<File> call(File file) {
                        //参数file是just操作符产生的结果，这里判断file是不是目录文件，如果是目录文件，则递归查找其子文件flatMap操作符神奇的地方在于，返回的结果还是一个Observable，而这个Observable其实是包含多个文件的Observable的，输出应该是ExternalCacheDir下的所有文件
                        return listFiles(file);
                    }
                })
                .subscribe(new Action1<File>() {
                    @Override
                    public void call(File file) {
                        System.out.println(file.getAbsolutePath());
                    }
                });

    }
```
map数据是一对一的转换，flatMap是数据一对多的转换，可以理解成为嵌套

<a name="concatmap操作符"></a>
##### concatMap操作符
cancatMap操作符与flatMap操作符类似，都是把Observable产生的结果转换成多个Observable，然后把这多个Observable“扁平化”成一个Observable，并依次提交产生的结果给订阅者。

与flatMap操作符不同的是，concatMap操作符在处理产生的Observable时，采用的是“连接(concat)”的方式，而不是“合并(merge)”的方式，这就能保证产生结果的顺序性，也就是说提交给订阅者的结果是按照顺序提交的，不会存在交叉的情况。
![](http://reactivex.io/documentation/operators/images/concatMap.png)
concatMap的调用例子与flatMap类似

<a name="switchmap操作符"></a>
##### switchMap操作符
switchMap操作符与flatMap操作符类似，都是把Observable产生的结果转换成多个Observable，然后把这多个Observable“扁平化”成一个Observable，并依次提交产生的结果给订阅者。

与flatMap操作符不同的是，switchMap操作符会保存最新的Observable产生的结果而舍弃旧的结果，举个例子来说，比如源Observable产生A、B、C三个结果，通过switchMap的自定义映射规则，映射后应该会产生A1、A2、B1、B2、C1、C2，但是在产生B2的同时，C1已经产生了，这样最后的结果就变成A1、A2、B1、C1、C2，B2被舍弃掉了！流程图如下： 
![](http://reactivex.io/documentation/operators/images/switchMap.png)
以下是flatMap、concatMap和switchMap的运行实例对比：

```java
//flatMap操作符的运行结果
        Observable.just(10, 20, 30).flatMap(new Func1<Integer, Observable<Integer>>() {
            @Override
            public Observable<Integer> call(Integer integer) {
                //10的延迟执行时间为200毫秒、20和30的延迟执行时间为180毫秒
                int delay = 200;
                if (integer > 10)
                    delay = 180;

                return Observable.from(new Integer[]{integer, integer / 2}).delay(delay, TimeUnit.MILLISECONDS);
            }
        }).observeOn(AndroidSchedulers.mainThread()).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                System.out.println("flatMap Next:" + integer);
            }
        });

        //concatMap操作符的运行结果
        Observable.just(10, 20, 30).concatMap(new Func1<Integer, Observable<Integer>>() {
            @Override
            public Observable<Integer> call(Integer integer) {
                //10的延迟执行时间为200毫秒、20和30的延迟执行时间为180毫秒
                int delay = 200;
                if (integer > 10)
                    delay = 180;

                return Observable.from(new Integer[]{integer, integer / 2}).delay(delay, TimeUnit.MILLISECONDS);
            }
        }).observeOn(AndroidSchedulers.mainThread()).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                System.out.println("concatMap Next:" + integer);
            }
        });

        //switchMap操作符的运行结果
        Observable.just(10, 20, 30).switchMap(new Func1<Integer, Observable<Integer>>() {
            @Override
            public Observable<Integer> call(Integer integer) {
                //10的延迟执行时间为200毫秒、20和30的延迟执行时间为180毫秒
                int delay = 200;
                if (integer > 10)
                    delay = 180;

                return Observable.from(new Integer[]{integer, integer / 2}).delay(delay, TimeUnit.MILLISECONDS);
            }
        }).observeOn(AndroidSchedulers.mainThread()).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                System.out.println("switchMap Next:" + integer);
            }
        });
```
> flatMap Next:20 
flatMap Next:10 
flatMap Next:30 
flatMap Next:15 
flatMap Next:10 
flatMap Next:5
switchMap Next:30 
switchMap Next:15
concatMap Next:10 
concatMap Next:5 
concatMap Next:20 
concatMap Next:10 
concatMap Next:30 
concatMap Next:15

<a name="groupby操作符"></a>
##### groupBy操作符

groupBy操作符是对源Observable产生的结果进行分组，形成一个类型为GroupedObservable的结果集，GroupedObservable中存在一个方法为getKey()，可以通过该方法获取结果集的Key值（类似于HashMap的key)。

值得注意的是，由于结果集中的GroupedObservable是把分组结果缓存起来，如果对每一个GroupedObservable不进行处理（既不订阅执行也不对其进行别的操作符运算），就有可能出现内存泄露。因此，如果你对某个GroupedObservable不进行处理，最好是对其使用操作符take(0)处理。
![](http://reactivex.io/documentation/operators/images/groupBy.png)

```java
Observable.interval(1, TimeUnit.SECONDS).take(10).groupBy(new Func1<Long, Long>() {
            @Override
            public Long call(Long value) {
                //按照key为0,1,2分为3组
                return value % 3;
            }
        }).subscribe(new Action1<GroupedObservable<Long, Long>>() {
            @Override
            public void call(GroupedObservable<Long, Long> result) {
                result.subscribe(new Action1<Long>() {
                    @Override
                    public void call(Long value) {
                        System.out.println("key:" + result.getKey() +", value:" + value);
                    }
                });
            }
        });
```
> 运行结果如下： 
key:0, value:0 
key:1, value:1 
key:2, value:2 
key:0, value:3 
key:1, value:4 
key:2, value:5 
key:0, value:6 
key:1, value:7 
key:2, value:8 
key:0, value:9

<a name="groupby操作符-1"></a>
##### groupBy操作符

groupBy操作符是对源Observable产生的结果进行分组，形成一个类型为GroupedObservable的结果集，GroupedObservable中存在一个方法为getKey()，可以通过该方法获取结果集的Key值（类似于HashMap的key)。

值得注意的是，由于结果集中的GroupedObservable是把分组结果缓存起来，如果对每一个GroupedObservable不进行处理（既不订阅执行也不对其进行别的操作符运算），就有可能出现内存泄露。因此，如果你对某个GroupedObservable不进行处理，最好是对其使用操作符take(0)处理。

groupBy操作符的流程图如下： 
![](http://reactivex.io/documentation/operators/images/groupBy.png)

```java
Observable.interval(1, TimeUnit.SECONDS).take(10).groupBy(new Func1<Long, Long>() {
            @Override
            public Long call(Long value) {
                //按照key为0,1,2分为3组
                return value % 3;
            }
        }).subscribe(new Action1<GroupedObservable<Long, Long>>() {
            @Override
            public void call(GroupedObservable<Long, Long> result) {
                result.subscribe(new Action1<Long>() {
                    @Override
                    public void call(Long value) {
                        System.out.println("key:" + result.getKey() +", value:" + value);
                    }
                });
            }
        });
```
> 运行结果如下： 
key:0, value:0 
key:1, value:1 
key:2, value:2 
key:0, value:3 
key:1, value:4 
key:2, value:5 
key:0, value:6 
key:1, value:7 
key:2, value:8 
key:0, value:9
    
<a name="map操作符"></a>
##### map操作符

map操作符是把源Observable产生的结果，通过映射规则转换成另一个结果集，并提交给订阅者进行处理。

map操作符的流程图如下： 
![](http://reactivex.io/documentation/operators/images/map.png)

```java
Observable.just(1,2,3,4,5,6).map(new Func1<Integer, Integer>() {
            @Override
            public Integer call(Integer integer) {
                //对源Observable产生的结果，都统一乘以3处理
                return integer*3;
            }
        }).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                System.out.println("next:" + integer);
            }
        });

```
>运行结果如下： 
next:3 
next:6 
next:9 
next:12 
next:15
next:18


<a name="cast操作符"></a>
##### cast操作符
cast操作符类似于map操作符，不同的地方在于map操作符可以通过自定义规则，把一个值A1变成另一个值A2，A1和A2的类型可以一样也可以不一样；而cast操作符主要是做类型转换的，传入参数为类型class，如果源Observable产生的结果不能转成指定的class，则会抛出ClassCastException运行时异常。

cast操作符的流程图如下：
![](http://reactivex.io/documentation/operators/images/cast.png)

```java
   Observable.just(1,2,3,4,5,6).cast(Integer.class).subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer value) {
                System.out.println("next:"+value);
            }
        });
```
> 运行结果如下： 
next:1 
next:2 
next:3 
next:4 
next:5 
next:6

<a name="scan操作符和python中的reduce很类似"></a>
##### scan操作符(和Python中的reduce很类似)
scan操作符通过遍历源Observable产生的结果，依次对每一个结果项按照指定规则进行运算，计算后的结果作为下一个迭代项参数，每一次迭代项都会把计算结果输出给订阅者。

scan操作符的流程图如下
![](http://reactivex.io/documentation/operators/images/scan.png)

```java
Observable.just(1, 2, 3, 4, 5)
    .scan(new Func2<Integer, Integer, Integer>() {
        @Override
        public Integer call(Integer sum, Integer item) {
            //参数sum就是上一次的计算结果
            return sum + item;
        }
    }).subscribe(new Subscriber<Integer>() {
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
> 运行结果如下： 
Next: 1 
Next: 3 
Next: 6 
Next: 10 
Next: 15 
Sequence complete.


<a name="window操作符"></a>
##### window操作符

window操作符非常类似于buffer操作符，区别在于buffer操作符产生的结果是一个List缓存，而window操作符产生的结果是一个Observable，订阅者可以对这个结果Observable重新进行订阅处理。

window操作符有很多个重载方法，这里只举一个简单的例子，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/window5.png)

```java
Observable.interval(1, TimeUnit.SECONDS).take(12)
                .window(3, TimeUnit.SECONDS)
                .subscribe(new Action1<Observable<Long>>() {
                    @Override
                    public void call(Observable<Long> observable) {
                        System.out.println("subdivide begin......");
                        observable.subscribe(new Action1<Long>() {
                            @Override
                            public void call(Long aLong) {
                                System.out.println("Next:" + aLong);
                            }
                        });
                    }
                });
```
> 运行结果如下： 
subdivide begin…… 
Next:0 
Next:1 
subdivide begin…… 
Next:2 
Next:3 
Next:4 
subdivide begin…… 
Next:5 
Next:6 
Next:7 
subdivide begin…… 
Next:8 
Next:9 
Next:10 
subdivide begin…… 
Next:11

<a name="filtering-observablesobservable的过滤操作符"></a>
#### Filtering Observables(Observable的过滤操作符)

<a name="-debounce操作符"></a>
#####  debounce操作符
debounce操作符对源Observable每产生一个结果后，如果在规定的间隔时间内没有别的结果产生，则把这个结果提交给订阅者处理，否则忽略该结果。

值得注意的是，如果源Observable产生的最后一个结果后在规定的时间间隔内调用了onCompleted，那么通过debounce操作符也会把这个结果提交给订阅者。

debounce操作符的流程图如下： 
![](http://reactivex.io/documentation/operators/images/debounce.png)

```java
Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                if(subscriber.isUnsubscribed()) return;
                try {
                    //产生结果的间隔时间分别为100、200、300...900毫秒
                    for (int i = 1; i < 10; i++) {
                        subscriber.onNext(i);
                        Thread.sleep(i * 100);
                    }
                    subscriber.onCompleted();
                }catch(Exception e){
                    subscriber.onError(e);
                }
            }
        }).subscribeOn(Schedulers.newThread())
          .debounce(400, TimeUnit.MILLISECONDS)  //超时时间为400毫秒
          .subscribe(
                new Action1<Integer>() {
                    @Override
                    public void call(Integer integer) {
                        System.out.println("Next:" + integer);
                    }
                }, new Action1<Throwable>() {
                    @Override
                    public void call(Throwable throwable) {
                        System.out.println("Error:" + throwable.getMessage());
                    }
                }, new Action0() {
                    @Override
                    public void call() {
                        System.out.println("completed!");
                    }
                });
```
> 运行结果如下： 
Next:4 
Next:5 
Next:6 
Next:7 
Next:8 
Next:9 
completed!


<a name="distinct操作符"></a>
##### distinct操作符
distinct操作符对源Observable产生的结果进行过滤，把重复的结果过滤掉，只输出不重复的结果给订阅者，非常类似于SQL里的distinct关键字。

![](http://reactivex.io/documentation/operators/images/distinct.png)

```java
Observable.just(1, 2, 1, 1, 2, 3)
          .distinct()
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
> 运行结果如下： 
Next: 1 
Next: 2 
Next: 3 
Sequence complete.

<a name="elementat操作符"></a>
##### elementAt操作符
elementAt操作符在源Observable产生的结果中，仅仅把指定索引的结果提交给订阅者，索引是从0开始的。其流程图如下： 
![](http://reactivex.io/documentation/operators/images/elementAt.png)

```java
Observable.just(1,2,3,4,5,6).elementAt(2)
          .subscribe(
                new Action1<Integer>() {
                    @Override
                    public void call(Integer integer) {
                        System.out.println("Next:" + integer);
                    }
                }, new Action1<Throwable>() {
                    @Override
                    public void call(Throwable throwable) {
                        System.out.println("Error:" + throwable.getMessage());
                    }
                }, new Action0() {
                    @Override
                    public void call() {
                        System.out.println("completed!");
                    }
                });
```
> 运行结果如下： 
Next:3 
completed!

<a name="filter操作符"></a>
##### filter操作符
filter操作符是对源Observable产生的结果按照指定条件进行过滤，只有满足条件的结果才会提交给订阅者，其流程图如下：
![](http://reactivex.io/documentation/operators/images/filter.png) 

```java
Observable.just(1, 2, 3, 4, 5)
          .filter(new Func1<Integer, Boolean>() {
              @Override
              public Boolean call(Integer item) {
                return( item < 4 );
              }
          }).subscribe(new Subscriber<Integer>() {
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
> 运行结果如下： 
Next: 1 
Next: 2 
Next: 3 
Sequence complete.

<a name="oftype操作符"></a>
##### ofType操作符
ofType操作符类似于filter操作符，区别在于ofType操作符是按照类型对结果进行过滤，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/ofClass.png)

```java
Observable.just(1, "hello world", true, 200L, 0.23f)
          .ofType(Float.class)
          .subscribe(new Subscriber<Object>() {
              @Override
              public void onNext(Object item) {
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
> 运行结果如下： 
Next: 0.23 
Sequence complete.


<a name="first操作符"></a>
##### first操作符
first操作符是把源Observable产生的结果的第一个提交给订阅者，first操作符可以使用elementAt(0)和take(1)替代。其流程图如下： 
![](http://reactivex.io/documentation/operators/images/first.png)

```java
Observable.just(1,2,3,4,5,6,7,8)
          .first()
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
> 运行结果如下： 
Next: 1 
Sequence complete.


<a name="single操作符"></a>
##### single操作符
single操作符是对源Observable的结果进行判断，如果产生的结果满足指定条件的数量不为1，则抛出异常，否则把满足条件的结果提交给订阅者，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/single.p.png)

```java
Observable.just(1,2,3,4,5,6,7,8)
          .single(new Func1<Integer, Boolean>() {
              @Override
              public Boolean call(Integer integer) {
                  //取大于10的第一个数字
                  return integer>10;
              }
          })
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
> 运行结果如下： 
Error: Sequence contains no elements

<a name="last操作符"></a>
##### last操作符
last操作符把源Observable产生的结果的最后一个提交给订阅者，last操作符可以使用takeLast(1)替代。其流程图如下
![](http://reactivex.io/documentation/operators/images/last.png)

```java
Observable.just(1, 2, 3)
          .last()
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
> 运行结果如下： 
Next: 3 
Sequence complete.


<a name="ignoreelements操作符"></a>
##### ignoreElements操作符
ignoreElements操作符忽略所有源Observable产生的结果，只把Observable的onCompleted和onError事件通知给订阅者。ignoreElements操作符适用于不太关心Observable产生的结果，只是在Observable结束时(onCompleted)或者出现错误时能够收到通知。
![](http://reactivex.io/documentation/operators/images/ignoreElements.png)

```java
 Observable.just(1,2,3,4,5,6,7,8).ignoreElements()
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
>运行结果如下： 
Sequence complete.

<a name="sample操作符"></a>
##### sample操作符
sample操作符定期扫描源Observable产生的结果，在指定的时间间隔范围内对源Observable产生的结果进行采样，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/sample.png)

```java
Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                if(subscriber.isUnsubscribed()) return;
                try {
                    //前8个数字产生的时间间隔为1秒，后一个间隔为3秒
                    for (int i = 1; i < 9; i++) {
                        subscriber.onNext(i);
                        Thread.sleep(1000);
                    }
                    Thread.sleep(2000);
                    subscriber.onNext(9);
                    subscriber.onCompleted();
                } catch(Exception e){
                    subscriber.onError(e);
                }
            }
        }).subscribeOn(Schedulers.newThread())
          .sample(2200, TimeUnit.MILLISECONDS)  //采样间隔时间为2200毫秒
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
>运行结果如下： 
Next: 3 
Next: 5 
Next: 7 
Next: 8 
Sequence complete.


<a name="skip操作符"></a>
##### skip操作符
skip操作符针对源Observable产生的结果，跳过前面n个不进行处理，而把后面的结果提交给订阅者处理，其流程图如下：
![](http://reactivex.io/documentation/operators/images/skip.png)

```java
Observable.just(1,2,3,4,5,6,7).skip(3)
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
> 运行结果如下： 
Next: 4 
Next: 5 
Next: 6 
Next: 7 
Sequence complete.


<a name="skiplast操作符"></a>
##### skipLast操作符
skipLast操作符针对源Observable产生的结果，忽略Observable最后产生的n个结果，而把前面产生的结果提交给订阅者处理，

值得注意的是，skipLast操作符提交满足条件的结果给订阅者是存在延迟效果的，看以下流程图即可明白： 
![](http://reactivex.io/documentation/operators/images/skipLast.png)
可以看到skipLast操作符把最后的天蓝色球、蓝色球、紫色球忽略掉了，但是前面的红色球等并不是源Observable一产生就直接提交给订阅者，这里有一个延迟的效果。

```java
Observable.just(1,2,3,4,5,6,7).skipLast(3)
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
> 运行结果如下： 
Next: 1 
Next: 2 
Next: 3 
Next: 4 
Sequence complete.


<a name="take操作符"></a>
##### take操作符
take操作符是把源Observable产生的结果，提取前面的n个提交给订阅者，而忽略后面的结果，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/take.png)

```java
bservable.just(1, 2, 3, 4, 5, 6, 7, 8)
          .take(4)
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
> 运行结果如下： 
Next: 1 
Next: 2 
Next: 3 
Next: 4 
Sequence complete.


<a name="takefirst操作符"></a>
##### takeFirst操作符

takeFirst操作符类似于take操作符，同时也类似于first操作符，都是获取源Observable产生的结果列表中符合指定条件的前一个或多个，与first操作符不同的是，first操作符如果获取不到数据，则会抛出NoSuchElementException异常，而takeFirst则会返回一个空的Observable，该Observable只有onCompleted通知而没有onNext通知。
![](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/takeFirstN.png)

```java
Observable.just(1,2,3,4,5,6,7).takeFirst(new Func1<Integer, Boolean>() {
            @Override
            public Boolean call(Integer integer) {
                //获取数值大于3的数据
                return integer>3;
            }
        })
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
> 运行结果如下： 
Next: 4 
Sequence complete


<a name="takelast操作符"></a>
##### takeLast操作符

takeLast操作符是把源Observable产生的结果的后n项提交给订阅者，提交时机是Observable发布onCompleted通知之时。其流程图如下
![](http://reactivex.io/documentation/operators/images/takeLast.n.png)

```java
Observable.just(1,2,3,4,5,6,7).takeLast(2)
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
> 运行结果如下： 
Next: 6 
Next: 7 
Sequence complete.

<a name="combining-observablesobservable的组合操作符"></a>
#### Combining Observables(Observable的组合操作符)

<a name="combinelatest操作符"></a>
##### combineLatest操作符
combineLatest操作符把两个Observable产生的结果进行合并，合并的结果组成一个新的Observable。这两个Observable中任意一个Observable产生的结果，都和另一个Observable最后产生的结果，按照一定的规则进行合并。流程图如下： 
![](http://reactivex.io/documentation/operators/images/combineLatest.png)

```java
//产生0,5,10,15,20数列
        Observable<Long> observable1 = Observable.timer(0, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 5;
                    }
                }).take(5);

        //产生0,10,20,30,40数列
        Observable<Long> observable2 = Observable.timer(500, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 10;
                    }
                }).take(5);


        Observable.combineLatest(observable1, observable2, new Func2<Long, Long, Long>() {
            @Override
            public Long call(Long aLong, Long aLong2) {
                return aLong+aLong2;
            }
        }).subscribe(new Subscriber<Long>() {
            @Override
            public void onCompleted() {
                System.out.println("Sequence complete.");
            }

            @Override
            public void onError(Throwable e) {
                System.err.println("Error: " + e.getMessage());
            }

            @Override
            public void onNext(Long aLong) {
                System.out.println("Next: " + aLong);
            }
        });

```
>运行结果如下： 
Next: 0 
Next: 5 
Next: 15 
Next: 20 
Next: 30 
Next: 35 
Next: 45 
Next: 50 
Next: 60 
Sequence complete.

<a name="join操作符"></a>
##### join操作符
join操作符把类似于combineLatest操作符，也是两个Observable产生的结果进行合并，合并的结果组成一个新的Observable，但是join操作符可以控制每个Observable产生结果的生命周期，在每个结果的生命周期内，可以与另一个Observable产生的结果按照一定的规则进行合并，流程图如下： 
![](http://reactivex.io/documentation/operators/images/join_.png)
join方法的用法如下： 
observableA.join(observableB, 
observableA产生结果生命周期控制函数， 
observableB产生结果生命周期控制函数， 
observableA产生的结果与observableB产生的结果的合并规则）

```java
//产生0,5,10,15,20数列
        Observable<Long> observable1 = Observable.timer(0, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 5;
                    }
                }).take(5);

        //产生0,10,20,30,40数列
        Observable<Long> observable2 = Observable.timer(500, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 10;
                    }
                }).take(5);

        observable1.join(observable2, new Func1<Long, Observable<Long>>() {
            @Override
            public Observable<Long> call(Long aLong) {
                //使Observable延迟600毫秒执行
                return Observable.just(aLong).delay(600, TimeUnit.MILLISECONDS);
            }
        }, new Func1<Long, Observable<Long>>() {
            @Override
            public Observable<Long> call(Long aLong) {
                //使Observable延迟600毫秒执行
                return Observable.just(aLong).delay(600, TimeUnit.MILLISECONDS);
            }
        }, new Func2<Long, Long, Long>() {
            @Override
            public Long call(Long aLong, Long aLong2) {
                return aLong + aLong2;
            }
        }).subscribe(new Subscriber<Long>() {
            @Override
            public void onCompleted() {
                System.out.println("Sequence complete.");
            }

            @Override
            public void onError(Throwable e) {
                System.err.println("Error: " + e.getMessage());
            }

            @Override
            public void onNext(Long aLong) {
                System.out.println("Next: " + aLong);
            }
        });
```
> 运行结果如下： 
Next: 0 
Next: 5 
Next: 15 
Next: 20 
Next: 30 
Next: 35 
Next: 45 
Next: 50 
Next: 60 
Sequence complete.

<a name="groupjoin操作符"></a>
##### groupJoin操作符
groupJoin操作符非常类似于join操作符，区别在于join操作符中第四个参数的传入函数不一致，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/groupJoin.png)

```java
//产生0,5,10,15,20数列
        Observable<Long> observable1 = Observable.timer(0, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 5;
                    }
                }).take(5);

        //产生0,10,20,30,40数列
        Observable<Long> observable2 = Observable.timer(500, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 10;
                    }
                }).take(5);

        observable1.groupJoin(observable2, new Func1<Long, Observable<Long>>() {
            @Override
            public Observable<Long> call(Long aLong) {
                return Observable.just(aLong).delay(1600, TimeUnit.MILLISECONDS);
            }
        }, new Func1<Long, Observable<Long>>() {
            @Override
            public Observable<Long> call(Long aLong) {
                return Observable.just(aLong).delay(600, TimeUnit.MILLISECONDS);
            }
        }, new Func2<Long, Observable<Long>, Observable<Long>>() {
            @Override
            public Observable<Long> call(Long aLong, Observable<Long> observable) {
                return observable.map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong2) {
                        return aLong + aLong2;
                    }
                });
            }
        }).subscribe(new Subscriber<Observable<Long>>() {
            @Override
            public void onCompleted() {
                System.out.println("Sequence complete.");
            }

            @Override
            public void onError(Throwable e) {
                System.err.println("Error: " + e.getMessage());
            }

            @Override
            public void onNext(Observable<Long> observable) {
                observable.subscribe(new Subscriber<Long>() {
                    @Override
                    public void onCompleted() {

                    }

                    @Override
                    public void onError(Throwable e) {

                    }

                    @Override
                    public void onNext(Long aLong) {
                        System.out.println("Next: " + aLong);
                    }
                });
            }
        });
```
> 运行结果如下： 
Next: 0 
Next: 5 
Next: 10 
Next: 15 
Next: 20 
Next: 25 
Next: 30 
Next: 35 
Next: 40 
Next: 45 
Next: 50 
Next: 60 
Next: 55 
Sequence complete.

<a name="merge操作符"></a>
##### merge操作符

merge操作符是按照两个Observable提交结果的时间顺序，对Observable进行合并，如ObservableA每隔500毫秒产生数据为0,5,10,15,20；而ObservableB每隔500毫秒产生数据0,10,20,30,40，其中第一个数据延迟500毫秒产生，最后合并结果为：0,0,5,10,10,20,15,30,20,40;其流程图如下：
![](http://reactivex.io/documentation/operators/images/merge.png)

```java
//产生0,5,10,15,20数列
        Observable<Long> observable1 = Observable.timer(0, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 5;
                    }
                }).take(5);

        //产生0,10,20,30,40数列
        Observable<Long> observable2 = Observable.timer(500, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 10;
                    }
                }).take(5);

        Observable.merge(observable1, observable2)
                .subscribe(new Subscriber<Long>() {
                    @Override
                    public void onCompleted() {
                        System.out.println("Sequence complete.");
                    }

                    @Override
                    public void onError(Throwable e) {
                        System.err.println("Error: " + e.getMessage());
                    }

                    @Override
                    public void onNext(Long aLong) {
                        System.out.println("Next:" + aLong);
                    }
                });
```
> 运行结果如下： 
Next:0 
Next:0 
Next:5 
Next:10 
Next:10 
Next:20 
Next:15 
Next:30 
Next:20 
Next:40 
Sequence complete


<a name="mergedelayerror操作符"></a>
##### mergeDelayError操作符
从merge操作符的流程图可以看出，一旦合并的某一个Observable中出现错误，就会马上停止合并，并对订阅者回调执行onError方法，而mergeDelayError操作符会把错误放到所有结果都合并完成之后才执行，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/mergeDelayError.png)

```java
//产生0,5,10数列,最后会产生一个错误
        Observable<Long> errorObservable = Observable.error(new Exception("this is end!"));
        Observable < Long > observable1 = Observable.timer(0, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 5;
                    }
                }).take(3).mergeWith(errorObservable.delay(3500, TimeUnit.MILLISECONDS));

        //产生0,10,20,30,40数列
        Observable<Long> observable2 = Observable.timer(500, 1000, TimeUnit.MILLISECONDS)
                .map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 10;
                    }
                }).take(5);

        Observable.mergeDelayError(observable1, observable2)
                .subscribe(new Subscriber<Long>() {
                    @Override
                    public void onCompleted() {
                        System.out.println("Sequence complete.");
                    }

                    @Override
                    public void onError(Throwable e) {
                        System.err.println("Error: " + e.getMessage());
                    }

                    @Override
                    public void onNext(Long aLong) {
                        System.out.println("Next:" + aLong);
                    }
                });
```
> 运行结果如下： 
Next:0 
Next:0 
Next:5 
Next:10 
Next:10 
Next:20 
Next:30 
Next:40 
Error: this is end!


<a name="startwith操作符"></a>
##### startWith操作符
startWith操作符是在源Observable提交结果之前，插入指定的某些数据，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/startWith.png)

```java
Observable.just(10,20,30).startWith(2, 3, 4).subscribe(new Subscriber<Integer>() {
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
Next:2 
Next:3 
Next:4 
Next:10 
Next:20 
Next:30 
Sequence complete.


<a name="switchonnext操作符"></a>
##### switchOnNext操作符
switchOnNext操作符是把一组Observable转换成一个Observable，转换规则为：对于这组Observable中的每一个Observable所产生的结果，如果在同一个时间内存在两个或多个Observable提交的结果，只取最后一个Observable提交的结果给订阅者，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/switchDo.png)

```java
//每隔500毫秒产生一个observable
        Observable<Observable<Long>> observable = Observable.timer(0, 500, TimeUnit.MILLISECONDS).map(new Func1<Long, Observable<Long>>() {
            @Override
            public Observable<Long> call(Long aLong) {
                //每隔200毫秒产生一组数据（0,10,20,30,40)
                return Observable.timer(0, 200, TimeUnit.MILLISECONDS).map(new Func1<Long, Long>() {
                    @Override
                    public Long call(Long aLong) {
                        return aLong * 10;
                    }
                }).take(5);
            }
        }).take(2);

        Observable.switchOnNext(observable).subscribe(new Subscriber<Long>() {
            @Override
            public void onCompleted() {
                System.out.println("Sequence complete.");
            }

            @Override
            public void onError(Throwable e) {
                System.err.println("Error: " + e.getMessage());
            }

            @Override
            public void onNext(Long aLong) {
                System.out.println("Next:" + aLong);
            }
        });

```
>运行结果如下： 
Next:0 
Next:10 
Next:20 
Next:0 
Next:10 
Next:20 
Next:30 
Next:40 
Sequence complete.


<a name="zip操作符"></a>
##### zip操作符
zip操作符是把两个observable提交的结果，严格按照顺序进行合并，其流程图如下： 
![](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/zip.o.png)

```java
Observable<Integer> observable1 = Observable.just(10,20,30);
        Observable<Integer> observable2 = Observable.just(4, 8, 12, 16);
        Observable.zip(observable1, observable2, new Func2<Integer, Integer, Integer>() {
            @Override
            public Integer call(Integer integer, Integer integer2) {
                return integer + integer2;
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
> 运行结果如下： 
Next:14 
Next:28 
Next:42 
Sequence complete.


<a name="error-handling-operatorsobservable的错误处理操作符"></a>
#### Error Handling Operators(Observable的错误处理操作符)

<a name="-onerrorreturn操作符"></a>
#####  onErrorReturn操作符
onErrorReturn操作符是在Observable发生错误或异常的时候（即将回调oError方法时），拦截错误并执行指定的逻辑，返回一个跟源Observable相同类型的结果，最后回调订阅者的onComplete方法，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/onErrorReturn.png)

```java
Observable<Integer> observable = Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                if (subscriber.isUnsubscribed()) return;
                //循环输出数字
                try {
                    for (int i = 0; i < 10; i++) {
                        if (i == 4) {
                            throw new Exception("this is number 4 error！");
                        }
                        subscriber.onNext(i);
                    }
                    subscriber.onCompleted();
                } catch (Exception e) {
                    subscriber.onError(e);
                }
            }
        });

        observable.onErrorReturn(new Func1<Throwable, Integer>() {
            @Override
            public Integer call(Throwable throwable) {
                return 1004;
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
> 运行结果如下： 
Next:0 
Next:1 
Next:2 
Next:3 
Next:1004 
Sequence complete.

<a name="onerrorresumenext操作符"></a>
##### onErrorResumeNext操作符
onErrorResumeNext操作符跟onErrorReturn类似，只不过onErrorReturn只能在错误或异常发生时只返回一个和源Observable相同类型的结果，而onErrorResumeNext操作符是在错误或异常发生时返回一个Observable，也就是说可以返回多个和源Observable相同类型的结果，其流程图如下： 
![](http://reactivex.io/documentation/operators/images/onErrorResumeNext.png)

```java
Observable<Integer> observable = Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                if (subscriber.isUnsubscribed()) return;
                //循环输出数字
                try {
                    for (int i = 0; i < 10; i++) {
                        if (i == 4) {
                            throw new Exception("this is number 4 error！");
                        }
                        subscriber.onNext(i);
                    }
                    subscriber.onCompleted();
                } catch (Exception e) {
                    subscriber.onError(e);
                }
            }
        });

        observable.onErrorResumeNext(new Func1<Throwable, Observable<? extends Integer>>() {
            @Override
            public Observable<? extends Integer> call(Throwable throwable) {
                return Observable.just(100,101, 102);
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
> 运行结果如下： 
Next:0 
Next:1 
Next:2 
Next:3 
Next:100 
Next:101 
Next:102 
Sequence complete.

<a name="onexceptionresumenext操作符"></a>
##### onExceptionResumeNext操作符

onExceptionResumeNext操作符和onErrorResumeNext操作符类似，不同的地方在于onErrorResumeNext操作符是当Observable发生错误或异常时触发，而onExceptionResumeNext是当Observable发生异常时才触发。

这里要普及一个概念就是，java的异常分为错误（error）和异常（exception）两种，它们都是继承于Throwable类。

错误（error）一般是比较严重的系统问题，比如我们经常遇到的OutOfMemoryError、StackOverflowError等都是错误。错误一般继承于Error类，而Error类又继承于Throwable类，如果需要捕获错误，需要使用try..catch(Error e)或者try..catch(Throwable e)句式。使用try..catch(Exception e)句式无法捕获错误

异常（Exception）也是继承于Throwable类，一般是根据实际处理业务抛出的异常，分为运行时异常（RuntimeException）和普通异常。普通异常直接继承于Exception类，如果方法内部没有通过try..catch句式进行处理，必须通过throws关键字把异常抛出外部进行处理（即checked异常）；而运行时异常继承于RuntimeException类，如果方法内部没有通过try..catch句式进行处理，不需要显式通过throws关键字抛出外部，如IndexOutOfBoundsException、NullPointerException、ClassCastException等都是运行时异常，当然RuntimeException也是继承于Exception类，因此是可以通过try..catch(Exception e)句式进行捕获处理的。 
onExceptionResumeNext流程图如下： 
![](http://reactivex.io/documentation/operators/images/onExceptionResumeNextViaObservable.png)

```java
Observable<Integer> observable = Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                if (subscriber.isUnsubscribed()) return;
                //循环输出数字
                try {
                    for (int i = 0; i < 10; i++) {
                        if (i == 4) {
                            throw new Exception("this is number 4 error！");
                        }
                        subscriber.onNext(i);
                    }
                    subscriber.onCompleted();
                } catch (Throwable e) {
                    subscriber.onError(e);
                }
            }
        });

        observable.onExceptionResumeNext(Observable.just(100, 101, 102)).subscribe(new Subscriber<Integer>() {
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
> 运行结果如下： 
Next:0 
Next:1 
Next:2 
Next:3 
Next:100 
Next:101 
Next:102 
Sequence complete.

<a name="retry操作符"></a>
##### retry操作符

retry操作符是当Observable发生错误或者异常时，重新尝试执行Observable的逻辑，如果经过n次重新尝试执行后仍然出现错误或者异常，则最后回调执行onError方法；当然如果源Observable没有错误或者异常出现，则按照正常流程执行。其流程图如下： 
![](http://reactivex.io/documentation/operators/images/retry.png)

```java
Observable<Integer> observable = Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                if (subscriber.isUnsubscribed()) return;
                //循环输出数字
                try {
                    for (int i = 0; i < 10; i++) {
                        if (i == 4) {
                            throw new Exception("this is number 4 error！");
                        }
                        subscriber.onNext(i);
                    }
                    subscriber.onCompleted();
                } catch (Throwable e) {
                    subscriber.onError(e);
                }
            }
        });

        observable.retry(2).subscribe(new Subscriber<Integer>() {
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
> 运行结果如下： 
Next:0 
Next:1 
Next:2 
Next:3
Next:0 
Next:1 
Next:2 
Next:3
Next:0 
Next:1 
Next:2 
Next:3 
Error: this is number 4 error！

<a name="retrywhen操作符"></a>
##### retryWhen操作符

retryWhen操作符类似于retry操作符，都是在源observable出现错误或者异常时，重新尝试执行源observable的逻辑，不同在于retryWhen操作符是在源Observable出现错误或者异常时，通过回调第二个Observable来判断是否重新尝试执行源Observable的逻辑，如果第二个Observable没有错误或者异常出现，则就会重新尝试执行源Observable的逻辑，否则就会直接回调执行订阅者的onError方法。其流程图如下： 
![](http://reactivex.io/documentation/operators/images/retryWhen.f.png)

```java
Observable<Integer> observable = Observable.create(new Observable.OnSubscribe<Integer>() {
            @Override
            public void call(Subscriber<? super Integer> subscriber) {
                System.out.println("subscribing");
                subscriber.onError(new RuntimeException("always fails"));
            }
        });

        observable.retryWhen(new Func1<Observable<? extends Throwable>, Observable<?>>() {
            @Override
            public Observable<?> call(Observable<? extends Throwable> observable) {
                return observable.zipWith(Observable.range(1, 3), new Func2<Throwable, Integer, Integer>() {
                    @Override
                    public Integer call(Throwable throwable, Integer integer) {
                        return integer;
                    }
                }).flatMap(new Func1<Integer, Observable<?>>() {
                    @Override
                    public Observable<?> call(Integer integer) {
                        System.out.println("delay retry by " + integer + " second(s)");
                        //每一秒中执行一次
                        return Observable.timer(integer, TimeUnit.SECONDS);
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
subscribing 
delay retry by 1 second(s) 
subscribing 
delay retry by 2 second(s) 
subscribing 
delay retry by 3 second(s) 
subscribing 
Sequence complete.



<a name="observable-utility-operatorsobservable的功能性操作符"></a>
#### Observable Utility Operators(Observable的功能性操作符)
[博客链接](http://blog.chinaunix.net/uid-20771867-id-5206187.html)

<a name="delay操作符"></a>
##### Delay操作符
顾名思义，Delay操作符就是让发射数据的时机延后一段时间，这样所有的数据都会依次延后一段时间发射。在Rxjava中将其实现为Delay和DelaySubscription。不同之处在于Delay是延时数据的发射，而DelaySubscription是延时注册Subscriber。


<a name="do操作符"></a>
##### Do操作符
Do操作符就是给Observable的生命周期的各个阶段加上一系列的回调监听，当Observable执行到这个阶段的时候，这些回调就会被触发。在Rxjava实现了很多的doxxx操作符。
DoOnEach可以给Observable加上这样的样一个回调：Observable每发射一个数据的时候就会触发这个回调，不仅包括onNext还包括onError和onCompleted。
DoOnNext则只有onNext的时候才会被触发。


<a name="meterialize操作符"></a>
##### Meterialize操作符
Meterialize操作符将OnNext/OnError/OnComplete都转化为一个Notification对象并按照原来的顺序发射出来，而DeMeterialize则是执行相反的过程。

<a name="subscribonobserveron操作符"></a>
##### SubscribOn/ObserverOn操作符

<a name="timeinterval\timestamp操作符"></a>
##### TimeInterval\TimeStamp操作符

<a name="timeout操作符"></a>
##### Timeout操作符

<a name="using操作符"></a>
##### Using操作符




<a name="conditional-and-boolean-operatorsobservable的条件操作符"></a>
#### Conditional and Boolean Operators(Observable的条件操作符)
[博客链接](http://blog.chinaunix.net/uid-20771867-id-5208237.html)

<a name="all操作符"></a>
##### All操作符

<a name="contains、isemptyall操作符"></a>
##### Contains、IsEmptyAll操作符

<a name="defaultifempty操作符"></a>
##### DefaultIfEmpty操作符

<a name="sequenceequal操作符"></a>
##### SequenceEqual操作符
SequenceEqual操作符用来判断两个Observable发射的数据序列是否相同（发射的数据相同，数据的序列相同，结束的状态相同），如果相同返回true，否则返回false

<a name="skipuntil、skipwhile操作符"></a>
##### SkipUntil、SkipWhile操作符

<a name="takeuntil、takewhile操作符"></a>
##### TakeUntil、TakeWhile操作符





<a name="mathematical-and-aggregate-operatorsobservable数学运算及聚合操作符"></a>
#### Mathematical and Aggregate Operators(Observable数学运算及聚合操作符)
[博客链接](http://blog.chinaunix.net/uid-20771867-id-5209862.html)

<a name="concat操作符"></a>
##### Concat操作符
 Concat操作符将多个Observable结合成一个Observable并发射数据，并且严格按照先后顺序发射数据，前一个Observable的数据没有发射完，是不能发射后面Observable的数据的。
    有两个操作符跟它类似，但是有区别，分别是
    1.startWith：仅仅是在前面插上一个数据。
    2.merge:其发射的数据是无序的。
![](http://reactivex.io/documentation/operators/images/concat.png)

<a name="count操作符"></a>
##### Count操作符
    Count操作符用来统计源Observable发射了多少个数据，最后将数目给发射出来；如果源Observable发射错误，则会将错误直接报出来；在源Observable没有终止前，count是不会发射统计数据的。

<a name="reduce、collect操作符"></a>
##### Reduce、Collect操作符
- Reduce操作符应用一个函数接收Observable发射的数据和函数的计算结果作为下次计算的参数，输出最后的结果。跟前面我们了解过的scan操作符很类似，只是scan会输出每次计算的结果，而reduce只会输出最后的结果。
-  Collect操作符类似于Reduce，但是其目的不同，collect用来将源Observable发射的数据给收集到一个数据结构里面，需要使用两个参数：
    1.一个产生收集数据结构的函数。
    2.一个接收第一个函数产生的数据结构和源Observable发射的数据作为参数的函数。

                


----

<a name="map操作符转换执行结果，将事件序列中的对象或整个序列进行加工处理，转换成不同的事件或事件序"></a>
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

 timer操作符

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

 mergeDelayError 合并几个不同的Observable
 sample的意思是每隔一段时间就进行采样,在时间间隔范围内获取最后一个发布的Observable
 latMap的意思是把某一个Observable转换成另一个Observable
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