<!-- MarkdownTOC -->

- [Android项目的目录结构](#android项目的目录结构)
    - [. Eclipse的目录结构](#-eclipse的目录结构)
    - [. Android studio的目录结构](#-android-studio的目录结构)
- [DDMS](#ddms)
- [adb常用命令](#adb常用命令)
- [pk的安装过程](#pk的安装过程)
- [android apk的打包过程](#android-apk的打包过程)
- [android 的存储](#android-的存储)
- [haredPreference](#haredpreference)
- [android 操作xml](#android-操作xml)
- [android 的测试](#android-的测试)
    - [. 测试分来](#-测试分来)
    - [. android的junit测试](#-android的junit测试)
- [SQLite数据库](#sqlite数据库)
    - [使用greendao，ORMLite等orm框架来实现数据库操作](#使用greendao，ormlite等orm框架来实现数据库操作)
- [android 网络请求](#android-网络请求)
    - [. 使用HttpURLConnection的方式](#-使用httpurlconnection的方式)
    - [. OKHttp框架的方式](#-okhttp框架的方式)
    - [. Volley框架](#-volley框架)
    - [. android-async-http框架](#-android-async-http框架)
    - [. xUtil](#-xutil)
- [android的异步](#android的异步)
    - [1. 消息队列; Handler的方式](#1-消息队列-handler的方式)
    - [2. AsyncTask的方式](#2-asynctask的方式)
    - [3. EventBus的方式](#3-eventbus的方式)
- [android 的图片处理](#android-的图片处理)
    - [1.为了避免oom大图转小图](#1为了避免oom大图转小图)
    - [2. 使用框架 Universal Image Loader](#2-使用框架-universal-image-loader)
- [Activity](#activity)
    - [Activity的跳转](#activity的跳转)
    - [Activity的生命周期](#activity的生命周期)
    - [Activity 的启动模式和标记](#activity-的启动模式和标记)
    - [activity返回值](#activity返回值)
- [播接受者\(broadcast receiver\)](#播接受者broadcast-receiver)
- [service](#service)
    - [进程优先级](#进程优先级)
    - [开启服务](#开启服务)
- [ContentProvider](#contentprovider)
- [ragment](#ragment)
- [动画](#动画)
- [.9.png\(9-Patch\)](#9png9-patch)

<!-- /MarkdownTOC -->



<a name="android项目的目录结构"></a>
# Android项目的目录结构
<a name="-eclipse的目录结构"></a>
##1. Eclipse的目录结构
* Activity：应用被打开时显示的界面
* src：项目代码
* R.java：项目中所有资源文件的资源id
* Android.jar：Android的jar包，导入此包方可使用Android的api
* libs：导入第三方jar包
* assets：存放资源文件，比方说mp3、视频文件
* bin：存放编译打包后的文件
* res：存放资源文件，存放在此文件夹下的所有资源文件都会生成资源id
* drawable：存放图片资源
* layout：存放布局文件，把布局文件通过资源id指定给activity，界面就会显示出该布局文件定义的布局
* menu：定义菜单的样式
* Strings.xml：存放字符串资源，每个资源都会有一个资源id
* project.properties 代表编译的版本
* drawable的后缀
    1. h high 高
    2. m middle 中
    3. l low 低
    4. x max 特大分辨率
    5. xx 超大

<a name="-android-studio的目录结构"></a>
##2. Android studio的目录结构
* 使用的是gradle来进行管理的
* * *

<a name="ddms"></a>
# DDMS
* Dalvik debug monitor service
* Dalvik调试监控服务
* * *

<a name="adb常用命令"></a>
# adb常用命令
- adb.exe    android debug bridge android调试桥
- adb devices 列出所有连接设备
- reset adb 重启adb的调试桥,分为两部--> adb kill-server ; adb start -server
- adb install xxx.apk 安装
- adb uninstall com.xx.xx 卸载
- adb install -r xx.apk 重装
- adb -s emulatot-5554 install xx.apk 选择不同的
- adb push 放进去
- adb pull 拿出
- adb shell 手机命令行终端
- adb shell --> monkey 5000 冒烟测试
- cmd -->netstat -anno 查看端口号


* * *

<a name="pk的安装过程"></a>
#apk的安装过程
1. 先把apk拷贝到/data/app下， 没错，就是完整的apk, 例如com.calendar.UI-2.apk
2. 解压apk，把其中的classes.dex 拷贝到/data/dalvik-cache, 其命名规则是 apk路径+classes.dex, 如： data@app@com.calendar.UI-2.apk@classes.dex, 其中@表示目录符号/
3. 在/data/data下创建对应的目录，用于存储程序的数据，例如cache, database等， 目录名称与包名相同, 如com.calendar.ui 要注意的是， 安装过程并没有把资源文件, assets目录下文件拷贝出来，他们还在apk包里面呆着，所以，当应用要访问资源的时候，其实是从apk包里读取出来的。其过程是，首先加载apk里的resources.arsc(这个文件是存储资源Id与值的映射文件)，根据资源id读取加载相应的资源。


<a name="android-apk的打包过程"></a>
# android apk的打包过程
1. 生成keystore文件 ：
    >keytool -genkeypair -alias asaiAndroid.keystore -keyalg RSA -validity 20000 -keystore asaiAndroid.keystore
    -genkeypair 指定生成密钥对
    -alias 密钥对的别名
    -keyalg 密钥对用于的算法，这里用的是RSA
    -validity 密钥对的有效期，单位为天
    -keystore 密钥对存储的文件名

2. 对未签名的apk进行签名：
    >jarsigner -verbose -certs -keystore asaiAndroid.keystore -signedjar HelloNDK_signed.apk HelloNDK.apk saiAndroid.keystore -digestalg SHA1 -sigalg MD5withRSA
    >dk 1.7需要加上-digestalg SHA1 -sigalg MD5withRSA
    -verbose 输出签名详细信息
    -keystore 指定密钥对的存储路径
    -signedjar 后面三个参数分别是 签名后的APK包 未签名的APK包 和 密钥对的别名

3. 优化apk包
    >zipalign -f -v 4 Magick.apk Magick_zip.apk
    -f 强制覆盖已有的文件
    -v 输出详细内容

4. 指定档案整理的字节数，一般为4，及32位。如果以后android的设备有64位的，可能要改成8吧.
    >Magick.apk 是未整理的apk文件名
    Magick_zip.apk 是整理后的apk文件名

5. 使用 keytool 查看签名
    >keytool -list -keystore ~/tools/app.keystore
    >然后根据提示输入keystore密钥即可

* * *

<a name="android-的存储"></a>
# android 的存储
* Ram内存：运行内存，相当于电脑的内存
* Rom内存：内部存储空间(internal storage)，相当于电脑的硬盘
    - 权限：一般情况下一个应用程序都会对应一个用户和一个进程，这样可以控制权限
    - openFileOutput的四种模式
        * MODE_PRIVATE：-rw-rw----
        * MODE_APPEND:-rw-rw----
        * MODE_WORLD_WRITEABLE:-rw-rw--w-
        * MODE_WORLD_READABLE:-rw-rw-r--
        * MODE_WORLD_READABLE | MODE_WORLD_WRITEABLE -rw-rw-rw 全局可读可写
    - 可以使用Context对象获取文件路径或输出输入流
            context.getFilesDir()获取app的internal目录 /data/data/包名/files 存放在这个路径下的文件，只要你不删，它就一直在
            context.getCacheDir()app的internal缓存目录 存放在这个路径下的文件，当内存不足时，有可能被删除
            context.openFileInput() 来获取文件输入流
            context.openFileOutput() 文件输出流
            context.deleteFile() 删除文件
* sd卡：外部存储空间(external storage)，相当于电脑的移动硬盘
    - 读SD卡上的是不要权限的，但在4.0之后就需要权限了
    - 写SD卡上的文件是需要添加权限
            Environment.getExternalStrorageDirectory();api获取SD卡的路径
            if(Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) 判断SD是否就绪
            context.getExternalCacheDir()目录 Android/包名/data/cache， 获取SD卡外部存储上的缓存
            file.getFreeSpace() or file.getTotalSpace() 来判断是否有足够的空间来保存文件

            StatFs state = new StatFs(path.getPath());
            long blockSize = state.getBlockSize();        //每个区块的大小
            long availableBlocks = state.getAvailableBlocks(); //可用区块的数量
            long totalBlock = state.getBlockCount();    //整个区块的数量
            //这个字符串就是sd卡剩余容量
            Formatter.formatFileSize(getContext(),(availableBlocks * blockSize));
            //这两个参数相乘，得到sd卡以字节为单位的剩余容量
            availableBlocks * blockSize
            Build.VERSION.SDK_INT //可以当前系统的系统号
            Build.VERSION_CODES.xx //系统的别名
      - 存储设备会被分为若干个区块，每个区块有固定的大小
      - 区块大小 * 区块数量 = 存储设备的总大小

    - 当用户卸载我们的app时，系统仅仅会删除external根目录（getExternalFilesDir()）下的相关文件
    - sd卡的路径
        >sdcard：2.3之前的sd卡路径
         mnt/sdcard：4.3之前的sd卡路径
         storage/sdcard：4.3之后的sd卡路径
    - 写sd卡需要权限
        `uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>`
    - 读sd卡，在4.0之前不需要权限，4.0之后可以设置为需要
        `<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>`

<a name="haredpreference"></a>
#SharedPreference
- 往SharedPreference里写数据
```java
    //拿到一个SharedPreference对象 config是文件名，创建在data/data/包名/shared_prefs/目录下
    SharedPreferences sp = getSharedPreferences("config", MODE_PRIVATE);
    //拿到编辑器
    Editor ed = sp.edit();
    //写数据
    ed.putString("name", name);
    ed.commit();
```
- 从SharedPreference里读数据
```java
    SharedPreferences sp = getSharedPreferences("config", MODE_PRIVATE);
    //从SharedPreference里取数据
    String name = sp.getBoolean("name", "");
```

<a name="android-操作xml"></a>
# android 操作xml
1. 序列化xml
```java
    //得到xml序列化器对象
    XmlSerializer xs = Xml.newSerializer();
    //给序列化器设置输出流
    File file = new File(Environment.getExternalStorageDirectory(), "backupsms.xml");
    FileOutputStream fos = new FileOutputStream(file);
    //给序列化器指定好输出流
    xs.setOutput(fos, "utf-8");
    //开始生成xml文件
    xs.startDocument("utf-8", true);    //设置文档的开始
    xs.startTag(null, "smss");
    ......
    xs.endTag(null, "smss");
    serializer.endDocument();    //设置文档结束
    fos.close();    //关闭流
```

2. pull解析xml
```java
    //获取src文件夹下的资源文件
    InputStream is = getClassLoader().getResourceAsStream("weather.xml");
    //拿到pull解析对象
    XmlPullParser xp = Xml.newPullParser();
    //初始化
    xp.setInput("utf-8");
    //获取当前节点的时间类型
    int type = xp.getEventType();
    //只要不是xml结束事件
    while(type != XmlPullParser.END_DOCUMENT){
        switch(type){
            case XmlPullParser.START_TAG:
            //获取当前节点的名字
                if("weather".equals(xp.getName())){
                    citys = new ArrayList<City>();
                }
                else if("city".equals(xp.getName())){
                    city = new City();
                }
                else if("name".equals(xp.getName())){
                    //获取当前节点的下一个节点的文本
                    String name = xp.nextText();
                    city.setName(name);
                }
                else if("temp".equals(xp.getName())){
                    String temp = xp.nextText();
                    city.setTemp(temp);
                }
                else if("pm".equals(xp.getName())){
                    String pm = xp.nextText();
                    city.setPm(pm);
                }
                break;
            case XmlPullParser.END_TAG:
                if("city".equals(xp.getName())){
                    citys.add(city);
                }
                  break;
        }
           //移动到下一个节点
        type = xp.next();
    }
```
   * type节点事件类型主要有五种
    * START_DOCUMENT：xml头的事件类型
    * END_DOCUMENT：xml尾的事件类型
    * START_TAG：开始节点的事件类型
    * END_TAG：结束节点的事件类型
    * TEXT：文本节点的事件类型

<a name="android-的测试"></a>
# android 的测试
<a name="-测试分来"></a>
#####1. 测试分来
* 黑盒测试
    * 测试逻辑业务
* 白盒测试
    * 测试逻辑方法

* 根据测试粒度
    * 方法测试：function test
    * 单元测试：unit test
    * 集成测试：integration test
    * 系统测试：system test

* 根据测试暴力程度
    * 冒烟测试：smoke test
    * 压力测试：pressure test

<a name="-android的junit测试"></a>
#####2. android的junit测试
1. Eclipse中的单元测试
 * 定义一个类继承AndroidTestCase，在类中定义方法，即可测试该方法
 * 在指定指令集时，targetPackage指定你要测试的应用的包名
```xml
    <instrumentation
        android:name="android.test.InstrumentationTestRunner"
        android:targetPackage="com.itheima.junit"
        ></instrumentation>
```
 * 定义使用的类库
`<uses-library android:name="android.test.runner"></uses-library>`
 * 断言的作用，检测运行结果和预期是否一致
 * 如果应用出现异常，会抛给测试框架
2. android studio中的单元测试
 >android studio基本帮你建好了目录，只用创建一个类继承 InstrumentationTestCase 就可以了


<a name="sqlite数据库"></a>
# SQLite数据库
* 创建数据库需要使用的api：SQLiteOpenHelper
    * 必须定义一个构造方法：
        ```java
        //arg1:数据库文件的名字
        //arg2:游标工厂
        //arg3:数据库版本
        public MyOpenHelper(Context context, String name, CursorFactory factory, int version){}
        ```
    * 数据库被创建时会调用：onCreate方法
    * 数据库升级时会调用：onUpgrade方法

* 创建数据库
```java
    //创建OpenHelper对象
    MyOpenHelper oh = new MyOpenHelper(getContext(), "person.db", null, 1);
    //获得数据库对象,如果数据库不存在，先创建数据库，后获得，如果存在，则直接获得
    SQLiteDatabase db = oh.getWritableDatabase();
```
 * getWritableDatabase()：打开可读写的数据库
 * getReadableDatabase()：在磁盘空间不足时打开只读数据库，否则打开可读写数据库
* 创建数据库时创建表
```java
   public void onCreate(SQLiteDatabase db) {
        db.execSQL("create table person (_id integer primary key autoincrement, name char(10), phone char(20), money integer(20))");
        }
```
* 数据库的增删改查
 - 使用sql语句进行
```java
    //插入
    db.execSQL("insert into person (name, phone, money) values (?, ?, ?);", new Object[]{"张三", 15987461, 75000});
    //查找
    Cursor cs = db.rawQuery("select _id, name, money from person where name = ?;", new String[]{"张三"});
```
 - 使用api来进行
```java
   //插入
   //以键值对的形式保存要存入数据库的数据
    ContentValues cv = new ContentValues();
    cv.put("name", "刘能");
    cv.put("phone", 1651646);
    cv.put("money", 3500);
    //返回值是改行的主键，如果出错返回-1
    long i = db.insert("person", null, cv);

    //删除
    //返回值是删除的行数
    int i = db.delete("person", "_id = ? and name = ?", new String[]{"1", "张三"});

    //修改
    ContentValues cv = new ContentValues();
    cv.put("money", 25000);
    int i = db.update("person", cv, "name = ?", new String[]{"赵四"});

    //查询
    //arg1:要查询的字段
    //arg2：查询条件
    //arg3:填充查询条件的占位符
    Cursor cs = db.query("person", new String[]{"name", "money"}, "name = ?", new String[]{"张三"}, null, null, null);
    while(cs.moveToNext()){
            //获取指定列的索引值
            String name = cs.getString(cs.getColumnIndex("name"));
            String money = cs.getString(cs.getColumnIndex("money"));
            System.out.println(name + ";" + money);
        }
```
* 事务
```java
    try {
            //开启事务
            db.beginTransaction();
            ...........
            //设置事务执行成功
            db.setTransactionSuccessful();
        } finally{
            //关闭事务
            //如果此时已经设置事务执行成功，则sql语句生效，否则不生效
            db.endTransaction();
        }
    }
```
<a name="使用greendao，ormlite等orm框架来实现数据库操作"></a>
#### 使用greendao，ORMLite等orm框架来实现数据库操作
* * *

<a name="android-网络请求"></a>
# android 网络请求
<a name="-使用httpurlconnection的方式"></a>
####1. 使用HttpURLConnection的方式
 - get方式
```java
     //如果url里面带参数可以需要编码可以使用 URLEncoder.encode("string","utf-8")
     URL url = new URL(path);
    //获取连接对象，并没有建立连接
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    //设置连接和读取超时
    conn.setReadTimeout(5000);
    conn.setConnectTimeout(5000);
    //设置请求方法，注意必须大写
    conn.setRequestMethod("GET");
    //建立连接，发送get请求
    conn.connect();
    if(conn.getResponseCode() == 200){
        InputStream = conn.getInputStream();
        ......
    }
```

 - post方式
   * post提交数据是用流写给服务器的
   * 协议头中多了两个属性
     - Content-Type: application/x-www-form-urlencoded，描述提交的数据的mimetype
     - Content-Length: 32，描述提交的数据的长度
     ```java
          //给请求头添加post多出来的两个属性
        String data = "name=" + URLEncoder.encode(name) + "&pass=" + pass;
        conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
        conn.setRequestProperty("Content-Length", data.length() + "");
        //设置允许打开post请求的流
        conn.setDoOutput(true);
        //获取连接对象的输出流，往流里写要提交给服务器的数据
        OutputStream os = conn.getOutputStream();
        os.write(data.getBytes());
     ```
 - 多线程下载文件，
  http需要设置Range头 byte=startIndex+endIndex 来进行分段下载
  1.本地创建一个大小跟服务器文件同样大小临时文件 
  2.计算分配几个线程去下载服务器上的资源，知道每个线程的下载位置
  3.开启多个线程，每个线程下载对应位置的文件
  4.如果所有线程都把自己的数据下载完毕，服务器上的资源就被下载到本地了，需要使用RandomAccessFile的rwd模式
```java
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setConnectTimeout(5000);
    conn.setRequestMethod("GET");
    conn.setRequestProperty("Range","byte=1-1024");
```

<a name="-okhttp框架的方式"></a>
####2. OKHttp框架的方式
github: https://github.com/hongyangAndroid/okhttp-utils

<a name="-volley框架"></a>
####3. Volley框架

<a name="-android-async-http框架"></a>
####4. android-async-http框架
github：https://loopj.com/android-async-http/

<a name="-xutil"></a>
####5. xUtil
github: https://github.com/wyouflf/xUtils
<a name="android的异步"></a>
# android的异步
<a name="1-消息队列-handler的方式"></a>
#### 1. 消息队列; Handler的方式
  - Handler 、 Looper 、Message三者的关系：Looper负责的就是创建一个MessageQueue，然后进入一个无限循环体不断从该MessageQueue中读取消息，而消息的创建者就是一个或多个Handler
  - Looper主要是prepare()和loop()两个方法，会在activity创建的时候自动创建
   - prepare方法，将Looper的实例放到Theadlocal中，同一个线程中只能有一个Looper实例，prepare方法同一线程中只能调用一次。
   - loop方法，
     - 与当前线程绑定，保证一个线程只会有一个Looper实例，同时一个Looper实例也只有一个MessageQueue。
     - loop()方法，不断从MessageQueue中去取消息，交给消息的target属性的dispatchMessage去处理
  - Handler 使用Handler之前，我们都是初始化一个实例，比如用于更新UI线程，我们会在声明的时候直接初始化，或者在onCreate中初始化Handler实例。
      - Handler会在构造方法中通过`mLooper = Looper.myLooper();`来获取当前线程的Looper实例和其中的MessageQueue，来建立三者的关系。
        - handler通过`sendMessage()`方法最终调用MessageQueue的`queue.enqueueMessage(msg, uptimeMillis)`将消息发送到消息队列中-->
        - loop方法如果取到消息会调用message中的`dispatchMessage(msg);`-->
        - `dispatchMessage`又会调用Handler的`handleMessage(msg)`回调方法

   - Handler常用的方法`mHandler.sendMessage(Meassage msg)`,`mHandler.sendMessageDelayed(Message msg, long delayMillis)`延迟发送，`mHandler.post(Runable r)` 其实是比没有创建线程，而是发送一条消息，执行权限要比sendMessage要高
  - message 建议不要用new来重新分配内存，而使用`Message.obtain()`，因为Message内部维护了一个Message池用于Message的复用。
  - 如果要在非ui线程中调用toast或其他ui效果可以使用
```java
    Looper.prepare();
    Toast.makeText(context, message, duration).show();
    Looper.loop();
```

<a name="2-asynctask的方式"></a>
#### 2. AsyncTask的方式
 - 主要的方法
  - onPreExecute 完成一些准备操作，比如显示进度对话框
  - doInBackground 完成耗时操作,在进行耗时操作时还能不时的通过publishProgress给onProgressUpdate中传递参数
  - onProgressUpdate 可以进行UI操作，比如更新进度条
  - onPostExecute 进行设置控件数据更新UI等操作，例如隐藏进度对话框
 - 缺陷
  - 3.0以前的系统中还是会以支持多线程并发的方式执行，支持并发数也是我们上面所计算的128，阻塞队列可以存放10个；也就是同时执行138个任务是没有问题的；而超过138会马上出现java.util.concurrent.RejectedExecutionException
  - 3.0以上包括3.0的系统无论多少任务都会单线程执行

<a name="3-eventbus的方式"></a>
#### 3. EventBus的方式
.......


* * *
<a name="android-的图片处理"></a>
# android 的图片处理
<a name="1为了避免oom大图转小图"></a>
#### 1.为了避免oom大图转小图
 - 核心代码
```java
    //设置一些附加的标记，以此来指定解码选项
    BitmapFactory.Options options = new BitmapFactory.Options();
    //设置 inJustDecodeBounds 属性为true可以在解码的时候避免内存的分配，它会返回一个null的Bitmap，但是可以获取到 outWidth, outHeight 与 outMimeType。可以允许你在构造Bitmap之前优先读图片的尺寸与类型
    options.inJustDecodeBounds = true;
    //BitmapFactory提供了一些解码（decode）的方法（decodeByteArray(), decodeFile(), decodeResource()等），用来从不同的资源中创建一个Bitmap
    BitmapFactory.decodeResource(getResources(), R.id.myimage, options);
    int imageHeight = options.outHeight;
    int imageWidth = options.outWidth;
    String imageType = options.outMimeType;
```
 - 根据需要宽高来计算SampleSize
```java
  public static int caculateInSampleSize(Options options, int reqWidth, int reqHeight)
    {
        int width = options.outWidth;
        int height = options.outHeight;

        int inSampleSize = 1;

        if (width > reqWidth || height > reqHeight)
        {
            int widthRadio = Math.round(width * 1.0f / reqWidth);
            int heightRadio = Math.round(height * 1.0f / reqHeight);

            inSampleSize = Math.max(widthRadio, heightRadio);
        }

        return inSampleSize;
    }
```
 - 大图转小图
```java
   public static Bitmap decodeSampledBitmapFromResource(Resources res, int resId,
        int reqWidth, int reqHeight) {

        // First decode with inJustDecodeBounds=true to check dimensions
        final BitmapFactory.Options options = new BitmapFactory.Options();
        options.inJustDecodeBounds = true;
        BitmapFactory.decodeResource(res, resId, options);

        // Calculate inSampleSize
        options.inSampleSize = calculateInSampleSize(options, reqWidth, reqHeight);

        // Decode bitmap with inSampleSize set
        options.inJustDecodeBounds = false;
        return BitmapFactory.decodeResource(res, resId, options);
    }
```
 - 根据ImageView获适当的压缩的宽和高
```java
   public static ImageSize getImageViewSize(ImageView imageView)
    {

        ImageSize imageSize = new ImageSize();
        DisplayMetrics displayMetrics = imageView.getContext().getResources()
                .getDisplayMetrics();

        LayoutParams lp = imageView.getLayoutParams();

        int width = imageView.getWidth();// 获取imageview的实际宽度
        if (width <= 0)
        {
            width = lp.width;// 获取imageview在layout中声明的宽度
        }
        if (width <= 0)
        {
             //width = imageView.getMaxWidth();// 检查最大值
            width = getImageViewFieldValue(imageView, "mMaxWidth");
        }
        if (width <= 0)
        {
            width = displayMetrics.widthPixels;
        }

        int height = imageView.getHeight();// 获取imageview的实际高度
        if (height <= 0)
        {
            height = lp.height;// 获取imageview在layout中声明的宽度
        }
        if (height <= 0)
        {
            height = getImageViewFieldValue(imageView, "mMaxHeight");// 检查最大值
        }
        if (height <= 0)
        {
            height = displayMetrics.heightPixels;
        }
        imageSize.width = width;
        imageSize.height = height;

        return imageSize;
    }
    //图片bean
    public static class ImageSize
    {
        int width;
        int height;
    }
    //通过反射获取imageview的某个属性值,考虑到兼容性，老版本没有getMaxWidth方法
    private static int getImageViewFieldValue(Object object, String fieldName)
    {
        int value = 0;
        try
        {
            Field field = ImageView.class.getDeclaredField(fieldName);
            field.setAccessible(true);
            int fieldValue = field.getInt(object);
            if (fieldValue > 0 && fieldValue < Integer.MAX_VALUE)
            {
                value = fieldValue;
            }
        } catch (Exception e)
        {
        }
        return value;
    }
```
<a name="2-使用框架-universal-image-loader"></a>
#### 2. 使用框架 Universal Image Loader
 github地址：https://github.com/nostra13/Android-Universal-Image-Loader

<a name="activity"></a>
# Activity
<a name="activity的跳转"></a>
#### Activity的跳转
1.显示意图
   >通过设置Activity的包名和类名实现跳转，称为显式意图,同一个应用程序里;自己激活自己的东西，推荐使用显式意图，效率高

 * 跳转至同一项目下的另一个Activity，直接指定该Activity的字节码即可
```java
 Intent intent = new Intent();
 intent.setClass(this, SecondActivity.class);
 startActivity(intent);
```

 * 跳转至其他应用中的Activity，需要指定该应用的包名和该Activity的类名
```java
 Intent intent = new Intent();
 //启动系统自带的拨号器应用
 intent.setClassName("com.android.dialer", "com.android.dialer.DialtactsActivity");
 startActivity(intent);
```

2.隐式意图
 >通过指定动作实现跳转，称为隐式意图;不同应用程序里，激活别人的应用程序，或者让自己某个页面被给别人激活推荐使用隐式意图。

 * 隐式意图跳转至指定Activity
```java
 Intent intent = new Intent();
 //启动系统自带的拨号器应用
 intent.setAction(Intent.ACTION_DIAL);
 startActivity(intent);
```
 * 要让一个Activity可以被隐式启动，需要在清单文件的activity节点中设置intent-filter子节点
```xml
 <intent-filter >
  <action android:name="com.itheima.second"/>
     <data android:scheme="asd" android:mimeType="aa/bb"/>
     <category android:name="android.intent.category.DEFAULT"/>
  </intent-filter>
```
    * action 指定动作（可以自定义，可以使用系统自带的）
    * data   指定数据（操作什么内容）
    * category 类别 （默认类别，机顶盒，车载电脑）

* 隐式意图启动Activity，需要为intent设置以上三个属性，且值必须与该Activity在清单文件中对三个属性的定义匹配
* intent-filter节点及其子节点都可以同时定义多个，隐式启动时只需与任意一个匹配即可
* 获取通过setData传递的数据
```java
 //获取启动此Activity的intent对象
 Intent intent = getIntent();
 Uri uri = intent.getData();
```
* 显式意图和隐式意图的应用场景
 - 显式意图用于启动同一应用中的Activity
 - 隐式意图用于启动不同应用中的Activity
   - 如果系统中存在多个Activity的intent-filter同时与你的intent匹配，那么系统会显示一个对话框，列出所有匹配的Activity，由用户选择启动哪一个，如果没有，程序会异常终止 activity not found exception
   - 验证是否有App去接收这个Intent
```java
   PackageManager packageManager = getPackageManager();
   List<ResolveInfo> activities = packageManager.queryIntentActivities(intent, 0);
   //如果isIntentSafe为true, 那么至少有一个app可以响应这个intent。false则说明没有app可以handle这个intent。
   boolean isIntentSafe = activities.size() > 0;
```
   - 使用`createChooser()`来创建Intent,用户可以在弹出dialog的时候选择默认启动的app
`Intent chooser = Intent.createChooser(intent, title);`

3.Activity跳转时的数据传递
* Activity通过Intent启动时，可以通过Intent对象携带数据到目标Activity
```java
 Intent intent = new Intent(this, SecondActivity.class);
 intent.putExtra("maleName", maleName);
 intent.putExtra("femaleName", femaleName);
 startActivity(intent)
```
* 在目标Activity中取出数据
```java
Intent intent = getIntent();
String maleName = intent.getStringExtra("maleName");
String femaleName = intent.getStringExtra("femaleName");
```

<a name="activity的生命周期"></a>
#### Activity的生命周期
- 完整生命周期（entire lifetime）：onCreate -> onStart ->  onResumer ->  running —>  onPause -> onStop -> onDestory
- 可视生命周期(visible lifetime)：onStart-->onResume-->onPause-->onStop
- 前台生命周期（foreground lifetime）:onResume-->onPause
- 保存状态和恢复状态，首次进来savedInstanceState是空的，解决fragment转屏重启的问题
  - onCreate -> onStart -> onRestoreInstanceState -> onResumer ->  running —> onSaveInstanceState --> onPause -> onStop -> onDestory

- onWindowFocusChanged在Activity窗口获得或失去焦点时被调用
 - onCreate -> onStart ->  onResumer -> onWindowFocusChanged -> running —>  onPause -> onWindowFocusChanged -> onStop -> onDestory

- 横竖屏切换，会重启activity，解决方案：
 1.在mainfest中activity配置`android:configChanges="orientation|keyboardHidden|screenSize"` 4.0以上需要加上screenSize(会调用activity的onConfigurationChanged方法)
 2.写死是横屏还是竖屏 `android:screenOrientation="landscape"`或`portrait`
 3.将数据存储在fragment中，需要在fragment的onCreate方法里加上`setRetainInstance(true)`; 让后提供AsyncTask对象的getDate,setDate方法。


<a name="activity-的启动模式和标记"></a>
#### Activity 的启动模式和标记
任务栈：task stack 只针对 activity而言，用来维护用户界面的体验，任务栈其实就是一个后进先出的链表，记录维护了当前开启的activity。一般情况退出一个activity，就把这个activity从任务栈顶移除，如果任务栈空了，应用程序就算关闭，但进程其实还是存在。
在activity节点的加上launchMode=singletop属性
- standard 标准启动模式

- singletop 判断栈顶activity和要开启activity是否相同，如果相同就不开启新的，即使栈中已经存在该Activity的实例，只要不在栈顶，都会创建新的实例。(会调用onNewIntent方法) 作用：避免同一个页面重复开启（浏览器添加书签运用到了）

- singleTask 开启一个activity B的时候检查任务在里面是否有存在的activity B实例存在，如果存在就清空这个任务在activity B上面所有的activity(运用场景浏览器),会调用B的onNewIntent()方法

- singleInstance启动模式非常特殊， activity会运行在自己的任务栈里面，并且这个任务栈里面只有一个实例存在，如果你要保证一个activity在整个手机操作系统里面只有一个实例存在，使用singleInstance（应用场景： 电话拨打界面）;(会有前台任务栈和后台任务栈的切换)

- - -

- Intent标记
- 添加代码
```java
Intent intent = new Intent(A.this, B.class);
intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
startActivity(intent);
```
- FLAG_ACTIVITY_CLEAR_TOP：相当于SingleTask
- FLAG_ACTIVITY_SINGLE_TOP：singleTop启动模式一样
- FLAG_ACTIVITY_REORDER_TO_FRONT：Reorder 意思是 “重新排序”;比如如果给B添加该标记，启动顺序是 ABCDEB，但栈中的顺序会是 ACDEB，把前面已经存在的B放到栈顶了。

<a name="activity返回值"></a>
#### activity返回值
点击返回键的时候会调用`onBackPressed()`方法
从A界面打开B界面， B界面关闭的时候，返回一个数据给A界面
步骤：
1. 开启activity并且获取返回值 A界面`startActivityForResult(intent, 0);`
2. 在新开启的界面里面实现设置数据的逻辑 B界面
```java
    Intent data = new Intent();
    data.putExtra("phone", phone);
    //设置一个结果数据，数据会返回给调用者
    setResult(0, data);
    finish();//关闭掉当前的activity，才会返回数据
```
3. 在开启者activity里面实现方法，A界面`onActivityResult(int requestCode, int resultCode, Intent data)`通过data获取返回的数据

<a name="播接受者broadcast-receiver"></a>
#广播接受者(broadcast receiver)
- 写一个继承BroadCastReceiver的类,重写onReceive()方法,广播接收器仅在它执行这个方法时处于活跃状态。当onReceive()返回后，它即为失活状态,注意:为了保证用户交互过程的流畅,一些费时的操作要放到线程里,如类名SMSBroadcastReceiver
- 注册方式
 1.manifest中注册，广播一旦发出，系统就会去所有清单文件中寻找，哪个广播接收者的action和广播的action是匹配的，如果找到了，就把该广播接收者的进程启动起来
```xml
 <receiver android:name="com.qiu.SMSBroadcastReceiver" >
　　<intent-filter android:priority = "1000" >
　　　　<action android:name="android.provider.Telephony.SMS_RECEIVED" />
　　</intent-filter>
</receiver >
```
可通过intentfilter中的priority属性进行设置设优先级

 2.代码注册，需要使用广播接收者时，执行注册的代码，不需要时，执行解除注册的代码
```java
IntentFilter intentFilter = new IntentFilter("android.provider.Telephony.SMS_RECEIVED");
registerReceiver(mBatteryInfoReceiver ,intentFilter);
//注销
unregisterReceiver(receiver);
```
- 特的广播接受者，必须用代码注册，manifest注册是无效的（原因广播发送太频繁）
  - 电量改变(不是低电量广播)
  - 屏幕锁屏和解锁

- 广播类型
 - 普通广播：通过Context.sendBroadcast(intent)发送广播
 - 有序广播：Context.sendOrdreBroadcast(intent,receiverPermission)发送广播,
 - 异步广播：Context.sendStickyBroadcast(intent)发送,还有sendStickyOrderedBroadcast(intent, resultReceiver, scheduler,  initialCode, initialData,initialExtras)方法进行发送，需要有android.permission.BROADCAST_STICKY权限，接收并处理完Intent后，广播依然存在，直到你调用removeStickyBroadcast(intent)主动把它去掉
 - 优先级：-1000优先级最低，0系统默认，1000优先级最高;高优先级的广播接受者可以终止掉广播事件

* abortBroadcast(),可以拦截有序广播
* 4.0以后广播接收者安装以后必须手动启动一次，否则不生效
* 4.0以后广播接收者如果被手动关闭，就不会再启动了
* **注意**：
1.生命周期只有十秒左右，如果在 onReceive() 内做超过十秒内的事情，就会报ANR(Application No Response) 程序无响应的错误信息，如果需要完成一项比较耗时的工作 , 应该通过发送 Intent 给 Service, 由Service 来完成 . 这里不能使用子线程来解决 , 因为 BroadcastReceiver 的生命周期很短 , 子线程可能还没有结束BroadcastReceiver 就先结束了 .BroadcastReceiver 一旦结束 , 此时 BroadcastReceiver 的所在进程很容易在系统需要内存时被优先杀死 , 因为它属于空进程 ( 没有任何活动组件的进程 ). 如果它的宿主进程被杀死 , 那么正在工作的子线程也会被杀死 . 所以采用子线程来解决是不可靠的
2.动态注册广播接收器还有一个特点，就是当用来注册的Activity关掉后，广播也就失效了。静态注册无需担忧广播接收器是否被关闭,只要设备是开启状态,广播接收器也是打开着的。也就是说哪怕app本身未启动,该app订阅的广播在触发时也会对它起作用

* 在activity之外的context开启activity需要在intent上加入标记`intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);`才行。

<a name="service"></a>
# service
 * service是长期在后台运行的组件
 * 服务只会被创建一次，如果服务已经创建，并没有销毁，多次调用startService的方法，只会执行onStartCommand()和onStart()的方法
 * 服务停止：1调用stopService() 2.程序管理器停止
 * 绑定服务： bindService() ,可以得到代理人对象，间接调用服务里面的方法，如果调用activity销毁，服务也会随着被销毁
 * 混合调用：1先开启服务 2绑定服务

<a name="进程优先级"></a>
#### 进程优先级
1. Foreground process（前台进程）用户可以看到进程中某个activity界面，可以操作的进程
2. Visible process（可见进程）用户仍可以看到界面，但不可操作的进程
3. Service process（服务进程）应用程序有服务，服务在后台运行的进程
4. Background process（后台进程）没有任何服务进程，打开一个activity只会，按home键最小化的进程
5. Empty process（空进程）没有任何活动组件 存在的进程
系统会当内存不足的时候会自动销毁 空进程、后台进程，服务进程也会销毁，但当内存充足又会开启刚刚关闭的服务进程

* 前台进程
	* 拥有一个正在与用户交互的activity（onResume调用）的进程
	* 拥有一个与正在和用户交互的activity绑定的服务的进程
	* 拥有一个正在“运行于前台”的服务——服务的startForeground方法调用
	* 拥有一个正在执行以下三个生命周期方法中任意一个的服务(onCreate(), onStart(), or onDestroy())
	* 拥有一个正在执行onReceive方法的广播接收者的进程
* 可见进程
	* 拥有一个不在前台，但是对用户依然可见的activity（onPause方法调用）的进程
	* 拥有一个与可见（或前台）activity绑定的服务的进程

<a name="开启服务"></a>
#### 开启服务
* startService
	* 该方法启动的服务所在的进程属于服务进程
	* Activity一旦启动服务，服务就跟Activity一毛钱关系也没有了
* bindService
	* 该方法启动的服务所在进程不属于服务进程
	* Activity与服务建立连接，Activity一旦死亡，服务也会死亡
	* 可以调用服务里面的方法，而startService办不到
	* IBinder就是中间人接口，一般继承子类Binder

```java
 //activity中的代码
 //绑定服务
 private IPlayService playService;
 Intent intent = new Intent(this, PlayService.class);
 myconn = new MyConn();
 //先启动，让服务变成一个服务进程
 startService(intent);
 //绑定服务，拿到中间人对象
 bindService(intent, myconn , BIND_AUTO_CREATE);
 private class MyConn implements ServiceConnection{

		@Override
		public void onServiceConnected(ComponentName name, IBinder service) {
			System.out.println("成功连接服务");
			playService = (IPlayService) service;
		}
		@Override
		public void onServiceDisconnected(ComponentName name) {
		}
	}
 //在activity调用service的方法
 playService.play();

 //service中的的代码
 @Override
 public IBinder onBind(Intent intent) {
	System.out.println("绑定服务");
	return new MyBinder(); //return 会返给activity中MyConn的onServiceConnected方法作为参数
 }
 //继承Binder
 private class MyBinder extends  Binder implements IPlayService{
    @Override
    public void play(String path){
     	servricePlay(path);
    }
    ......
 }
 private void servricePlay(String path){
 ....
 }

 //IPlayService接口的代码
 public interface IPlayService {
	public void play(String path);
	public void pause();
	public void replay();
	public void stop();
 }
```

 * 混合调用
 - 用服务实现音乐播放时，因为音乐播放必须运行在服务进程中，可是音乐服务中的方法，需要被前台Activity所调用，所以需要混合启动音乐服务
 - 先start，再bind，销毁时先unbind，在stop

* 服务的分类
 * 本地服务：指的是服务和启动服务的activity在同一个进程中
 * 远程服务：指的是服务和启动服务的activity不在同一个进程中
 
* IntentServices
 - IntentService是继承Service的，那么它包含了Service的全部特性，当然也包含service的生命周期，那么与service不同的是，IntentService在执行onCreate操作的时候，内部开了一个线程，去你执行你的耗时操作。
  - 多了几个方法`onStartCommand()`这个方法的具体含义是，当你的需要这个service启动的时候，或者调用这个servcie的时候，那么这个方法首先是要被回调的。
  - `onHandleIntent(Intent intent)` IntentService在执行onCreate的方法的时候，其实开了一个线程HandlerThread,并获得了当前线程队列管理的looper，并且在onStart的时候，把消息置入了消息队列
  - IntentService是通过Handler looper message的方式实现了一个多线程的操作，同时耗时操作也可以被这个线程管理和执行，同时不会产生ANR的情况。，当任务执行完后，IntentService会自动停止,可以启动IntentService多次，而每一个耗时操作会以工作队列的方式在IntentService的onHandleIntent回调方法中执行，并且，每次只会执行一个工作线程，执行完第一个再执行第二个，以此类推。onCreate方法只执行了一次，而onStartCommand和onStart方法执行了两次，开启了两个Work Thread

* aidl Android interface definition language 安卓接口定义语言
 * 作用：跨进程通信
 * 应用场景：远程服务中的中间人对象，其他应用是拿不到的，那么在通过绑定服务获取中间人对象时，就无法强制转换，使用aidl，就可以在其他应用中拿到中间人类所实现的接口

 * 步骤：
  1. 把远程服务的方法抽取成一个单独的接口java文件
  2. 把接口java文件的后缀名改成aidl
  3. 在自动生成的PublicBusiness.java文件中，有一个静态抽象类Stub，它已经继承了binder类，实现了publicBusiness接口，这个类就是新的中间人
  4. 把aidl文件复制粘贴到客户项目，粘贴的时候注意，aidl文件所在的包名必须跟服务项目中aidl所在的**包名**一致
  5. 在客户项目中d，强转中间人对象时，直接使用`iserver = Stub.asInterface（service）`

* AIDL的原理:
  1. 服务端的Binder实例会根据客户端依靠Binder驱动发来的消息，执行onTransact方法，然后由其参数决定执行服务端的代码,onTransact有四个参数
   * code 是一个整形的唯一标识，用于区分执行哪个方法，客户端会传递此参数，告诉服务端执行哪个方法
   * data客户端传递过来的参数
   * replay服务器返回回去的值
   * flags标明是否有返回值，0为有（双向），1为没有（单向）
 2. Proxy实例传入了我们的Binder驱动，并且封装了我们调用服务端的代码,客户端会通过Binder驱动的transact()方法调用服务端代码
 3. AIDL其实通过我们写的aidl文件，帮助我们生成了一个接口，一个Stub类用于服务端，一个Proxy类用于客户端调用
 4. 也可以不用adil进行进程间通许，可以在服务端实现只实现`onTransact()`方法，在客户端实现Proxy中需要调用的方法
 5. binder通信的四个角色应该是client组件，server组件，serviceManager和binder驱动。client组件和server组件进行通信，要通过ServiceManager查询server组件的引用。server向ServiceManager注册一个binders实体，client通过ServiceManager获得binder实体的一个引用，这样client和server就可以通信了。


基于Message的进程间通讯 http://blog.csdn.net/lmj623565791/article/details/47017485

* * *

<a name="contentprovider"></a>
# ContentProvider
* 内容提供者，继承ContentProvider类，重写增删改查方法，在方法中写增删改查数据库的代码
```java
 public class UserProvider extends ContentProvider{

	//定义uri的匹配器用于匹配uri 如果路径不满足条件返回-1
	private static UriMatcher matcher = new UriMatcher(UriMatcher.NO_MATCH);
	private static int INSERT = 0x001;
	private static int DELETE = 0x002;
	private static int UPDATA = 0x003;
	private static int QUERY = 0x004;
	private static int QUERY2 = 0x0042;
	private MySQLiteOpenHelper helper ;

	//content://com.qiu.sqlite/insert
	static {
		//添加一组匹配规则
		matcher.addURI("com.qiu.sqlite", "insert", INSERT);
		matcher.addURI("com.qiu.sqlite", "delete", DELETE);
		matcher.addURI("com.qiu.sqlite", "updata", UPDATA);
		matcher.addURI("com.qiu.sqlite", "query", QUERY);
        //#代表任意数字
		matcher.addURI("com.qiu.sqlite", "query/#", QUERY2);
	}

	///**在开启时调用此
	@Override
	public boolean onCreate() {
		// TODO Auto-generated method stub
		helper = new MySQLiteOpenHelper(getContext());
		return false;
	}

	@Override
	public Cursor query(Uri uri, String[] projection, String selection,
			String[] selectionArgs, String sortOrder) {
		if(matcher.match(uri)==QUERY){
			//此处不需要关闭db
			SQLiteDatabase db =  helper.getReadableDatabase();
		//	Cursor cursor = db.query("user", null, "name=?", new String[]{"qiu"}, null, null, null);
			Cursor cursor = db.query("user", projection, selection, selectionArgs, null, null, sortOrder);
			return cursor;
		}else if(matcher.match(uri)==QUERY2){
			Long id = ContentUris.parseId(uri);
			SQLiteDatabase db =  helper.getReadableDatabase();
			Cursor cursor = db.query("user", projection, "id=?", new String[]{id+""}, null, null, sortOrder);
			return cursor;
		}else{
			throw new IllegalArgumentException("访问路径不匹配，不能执行");
		}
	}

	@Override
	public String getType(Uri uri) {
		//在查询的时候判断是一组数据还是一条数据
		if(matcher.match(uri)==QUERY){
			return "vnd.android.cursor.dir/user";
		}else if(matcher.match(uri)==QUERY2){
			return "vnd.android.cursor.item/user";
		}
		return null;
	}

	@Override
	public Uri insert(Uri uri, ContentValues values) {
		if(matcher.match(uri)==INSERT){
			SQLiteDatabase db =  helper.getWritableDatabase();
			db.insert("user", null, values);
			//创建消息邮箱中的消息
			getContext().getContentResolver().notifyChange(Uri.parse("content://com.qiu"), null);
		}else{
			throw new IllegalArgumentException("访问路径不匹配，不能执行");
		}
		return null;
	}

	@Override
	public int delete(Uri uri, String selection, String[] selectionArgs) {
		int index = 0;
		if(matcher.match(uri)==DELETE){
			SQLiteDatabase db =  helper.getWritableDatabase();
			index = db.delete("user", "name=?", new String[]{"qiu"});
		}else{
			throw new IllegalArgumentException("访问路径不匹配，不能执行");
		}
		return index;
	}

	@Override
	public int update(Uri uri, ContentValues values, String selection,
			String[] selectionArgs) {
		if(matcher.match(uri)==UPDATA){
			SQLiteDatabase db =  helper.getWritableDatabase();
			db.update("user", values, "name=?", new String[]{"qiu"});
		}else{
			throw new IllegalArgumentException("访问路径不匹配，不能执行");
		}
		return 0;
	}
 }
```

* 在清单文件中定义内容提供者的标签，注意必须要有authorities属性，这是内容提供者的主机名，功能类似地址,4.0以后要加上exported属性
 ```xml
  <provider
     android:name="com.qiu.sqlite.provider.UserProvider"
     android:authorities="com.qiu.sqlite"
     android:exported="true"
    >
  </provider>
 ```
* 调用
```java
 ContentResolver resolver = getContentResolver();
 Uri uri = Uri.parse("content://com.qiu.sqlite/insert");
 ContentValues values = new ContentValues();
 values.put("name","qiu");
 resolver.insert(uri,values);

 //获取短信
 ContentResolver resolver = getContentResolver();
 // content://sms uri地址
 //content://com.android.contacts/raw_contacts 联系人
 //content://com.android.contacts/data 联系人信息
 Uri uri = Uri.parse("content://sms");
 Cursor cursor = resolver.query(uri, new String[] { "_id", "address", "date", "type", "body" }, null, null, null);
  while (cursor.moveToNext()) {
    int id = cursor.getInt(cursor.getColumnIndex("_id"));
	String address = cursor.getString(cursor.getColumnIndex("address"));
	long date = cursor.getLong(cursor.getColumnIndex("date"));
	int type = cursor.getInt(cursor.getColumnIndex("type"));
	String body = cursor.getString(cursor.getColumnIndex("body"));
 }
```
* 联系人表
    * raw\_contacts表：
        * contact_id：联系人id
    * data表：联系人的具体信息，一个信息占一行
        * data1：信息的具体内容
        * raw\_contact_id：联系人id，描述信息属于哪个联系人
        * mimetype_id：描述信息是属于什么类型
    * mimetypes表：通过mimetype_id到该表查看具体类型

* 内容观察者
```java
        // 内容观察者，消息邮箱传递机制 Linux 有消息邮箱，信号量两种进程通讯
		Uri uri = Uri.parse("content://sms");
		MyContentObserver observer = new MyContentObserver(new Handler());
		ContentResolver resolver = getContentResolver();
		// 注册一个内容观察者
		//notifyForDescendents 为true 对uri进行模糊的前缀域名进行匹配
		resolver.registerContentObserver(uri, true, observer);

        private class MyContentObserver extends ContentObserver{
            public MyContentObserver(Handler handler) {
                super(handler);

            }
            //内容观察者收到数据库发生改变的通知时，会调用此方法
            @Override
			public void onChange(boolean selfChange) {

			}
       }
```
 在内容提供者中发通知的代码
 ```java
 ContentResolver resolver = getContext().getContentResolver();
 //发出通知，所有注册在这个uri上的内容观察者都可以收到通知
 resolver.notifyChange(uri, null);
 ```

* * *

<a name="ragment"></a>
#Fragment
 - 生命周期，fragment的生命周期是依赖于activity
  onAttach --> onCreate --> onCreateView --> onActivityCreated --> onStart --> onResume --> onPause --> onStop --> onDestoryView -->onDestory --> onDetach
 - 转屏时如果加上setRetainInstance(true)会跳过onCreate 和 onDestory方法
 onPause --> onStop --> onDestoryView --> onDetach  |--> onAttach --> onCreateView -->onActivityCreated --> onStart --> onResume
 - 不加setRetainInstance(true)
 onPause --> onStop --> onDestoryView --> onDestory--> onDetach  |--> onAttach -->onCreate--> onCreateView -->onActivityCreated --> onStart --> onResume

- 介绍很详细的fragment的文章
- Fragment 真正的完全解析
  http://blog.csdn.net/lmj623565791/article/details/37970961
  http://blog.csdn.net/lmj623565791/article/details/37992017
- Fragment 你应该知道的一切 http://blog.csdn.net/lmj623565791/article/details/37992017
- Android 官方推荐 : DialogFragment创建对话框 http://blog.csdn.net/lmj623565791/article/details/37815413
- Android 屏幕旋转 处理 AsyncTask 和 ProgressDialog 的最佳方案 http://blog.csdn.net/lmj623565791/article/details/37936275
- fragment+viwepage的实现Tab http://blog.csdn.net/lmj623565791/article/details/24740977

<a name="动画"></a>
# 动画
 - 补间动画
 - 帧动画
 - 属性动画 学习地址： http://blog.csdn.net/lmj623565791/article/details/38067475

<a name="9png9-patch"></a>
# .9.png(9-Patch)
> 通过黑色边线来描述图片的拉伸情况和填充文字的方式
> 上边线表示图片水平拉伸, 左边线表示垂直拉伸
> 右边线表示垂直填充区域, 下边线表示水平填充区域


[android开发中让你详见恨晚的接口](http://blog.csdn.net/sbsujjbcy/article/details/47294997)
