### Android打包本地AAR文件

一、在模块下的build.gradle文件中添加

```java
apply pluing 'maven'
uploadArchives{
    repositories.mavenDeployer{
        // 本地AAR文件存放目录
        repository(url:"file:/home/shanrufu/Desktop/develop/QAi/")
        // 唯一标识
        pom.groupId = "com.qdreamer"
        // 项目名称
        pom.artifactId = "qai-release"
        // 版本号
        pom.version = "0.0.1"
    }
}
```

二、在gradle命令组中upload下执行uploadArchives命令



### 引用本地AAR文件

一、将AAR包拷贝到本地磁盘或项目中

二、在项目的的build.gradle文件中添加

```java
allprojects {
    repositories {
        google()
        jcenter()
		// file:AAR包本地存放路径
        maven {url 'file:/home/shanrufu/Desktop/develop/QAi/'}
    }
}
```

三、每次更新AAR包后同步工程



### Android分包（Android5.0及以上）

一、在主模块的build.gradle文件中添加

```java
defaultConfig {
    applicationId "com.android.myapplication"
    minSdkVersion 19
    targetSdkVersion 29
    versionCode 1
    versionName "1.0"
    testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
   	//开启分包 
    multiDexEnabled true
        
   ndk {
        abiFilters "armeabi-v7a"
    }
}

//添加依赖
implementation 'com.android.support:multidex:1.0.3'
```

二、application继承MultiDexApplication()

```JAVA
public class MyAclication extends MultDexApplication{}
```
或者继承Application，重写attachBaseContext()
```Java
public class MyApplication extends Application{
	@Override
	protect voidattachBaseContext(Context base){
		super.attachBaseContext(base);
		MultiDex.install(this);
	}
}
```



### 状态栏

```xml
<style name="TranslucentTheme" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowTranslucentStatus">false</item>
    <item name="android:windowTranslucentNavigation">true</item>
    <!--Android 5.x开始需要把颜色设置透明，否则导航栏会呈现系统默认的浅灰色-->
    <item name="android:statusBarColor">@android:color/transparent</item>
</style>
```


屏幕适配
```xml
<!-- 全面屏. -->
<meta-data
	android:name="android.notch_support"
	android:value="true" />
<meta-data
	android:name="notch.config"
	android:value="portrait|landscape" />
<meta-data
	android:name="android.max_aspect"
	android:value="2.4" />
```

