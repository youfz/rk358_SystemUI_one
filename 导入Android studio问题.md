**一、【无标题】Android support和Androidx库不能共存问题(androidx.core:core:1.5.0) and support-compat-27.0.2-runtime (co**

非玦

已于 2022-11-16 21:29:07 修改

791
 收藏
文章标签： android
版权
因为项目里的其他库已经使用了Androidx库，但是还有应用Android support库，从而引起的冲突
不难解决 只需要在gradle.properties中添加如下

android.useAndroidX=true
android.enableJetifier=true
————————————————
版权声明：本文为CSDN博主「非玦」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_54510678/article/details/127893736



二、[Theme.MaterialComponents.DayNight not found](https://stackoverflow.com/questions/59495906/theme-materialcomponents-daynight-not-found)



三、解决Android Studio在Build时报错乱码

   ①首先我们打开**Help -> Edit Custom VM Options**；

  ②在打开文件的最后一行添加 **-Dfile.encoding=UTF-8** ；

  ③ 在**File -> Invalidate Caches** ，点击**Invalidate and Restart**清除缓存并重启即可

四、查找不到SysUiStatsLog对象，代码找不到，考虑是在架包里，使用grep -nr "SysUiStatsLog"命令查找

_Android12/out/target/common/obj/JAVA_LIBRARIES# grep -nr "SysUiStatsLog"



root@bs-Z690-UD-DDR4:/mnt/sw_test/PET_RK3568_P01_Android12/out/target/common/obj/JAVA_LIBRARIES# grep -nr "SysUiStatsLog" .
匹配到二进制文件 ./QuickstepResLib_intermediates/classes.jar
匹配到二进制文件 ./SystemUI-statsd_intermediates/classes.jar
匹配到二进制文件 ./LauncherGoResLib_intermediates/classes.jar
匹配到二进制文件 ./SystemUI-core_intermediates/classes.jar
匹配到二进制文件 ./Launcher3QuickStepLib_intermediates/classes-header.jar
匹配到二进制文件 ./Launcher3QuickStepLib_intermediates/classes.jar
匹配到二进制文件 ./SystemUI-tests_intermediates/classes-header.jar
匹配到二进制文件 ./SystemUI-tests_intermediates/classes.jar

最终发现在SystemUI-statsd架包里



五、找不到libcore.io.IoUtils    用grep命令查找

root@bs-Z690-UD-DDR4:/mnt/sw_test/PET_RK3568_P01_Android12/out/target/common/obj/JAVA_LIBRARIES/legacy.core.platform.api.stubs_intermediates# ls
classes-header.jar
root@bs-Z690-UD-DDR4:/mnt/sw_test/PET_RK3568_P01_Android12/out/target/common/obj/JAVA_LIBRARIES/legacy.core.platform.api.stubs_intermediates# sz classes-header.jar 



六、替换Android.jar后  AudioAttributes$Builder.clas  报错

​      AudioAttributes文件需要替换回系统的，不能用rk编译出来的（特列）

七、编译出来的文件，没有目标文件

​       需要检查源码是否存在该文件（例如 WifiSsidPolicy.java）





```
 Thread.setUncaughtExceptionPreHandler(uncaughtExceptionHandler);
```

八、java调用kotlin代码编译报错“找不到符号”的问题

​       ![img](https://img-blog.csdnimg.cn/7a3ab5a9d76444b08b28c664224dc0d7.png)

像`Clean Project、Rebuild Project、Invalidate Caches`这些都试过了，问题依然没解决，包括gradle版本也更换到高版本，问题依旧。

最后在检查kotlin配置的时候，发现**module下的build.gradle中没有使用kotlin插件**，就是下面这行代码：

```csharp
apply plugin: 'kotlin-android'
```



九、2 files found with path 'META-INF/androidx.lifecycle_lifecycle-view

   解决方案：在app module的build.gradle文件里面的android{}增加packagingOptions配置,exclude 排除META-INF/kotlinx_coroutines_core.version：

```puppet
    packagingOptions {
        exclude 'META-INF/kotlinx_coroutines_core.version'
    }
```

10、DaggerGlobalRootComponent.java无法生成

 代码编译好后，由dagger2生成，可以先屏蔽DaggerGlobalRootComponent.java的错误代码，先修改其它

11、javax.inject.inject 找不到

 导入dagger2架包

12、查找app依赖库关系

​       点击Terminal 输入 `gradlew -q app:dependencies` 回车即可将app中所依赖的库展示出来

13、com.android.tools.r8.errors.b: Absent Code attribute in method that is not native or abstract

​       换成combined目录的jar包，或者换成javac里面的包

1. javac： javac是Java编译器，用于将Java源代码编译成Java字节码文件（.class文件）。在Android开发中，通过javac编译Java源代码可以生成可在Android设备上运行的字节码文件。
2. combined： combined是一个用于编译Android应用的自定义构建工具。它将多个Java类文件打包成一个或多个Dalvik Executable文件（.dex文件），用于在Android系统上执行。combined会将依赖的库文件打包到.dex文件中，以便在Android设备上运行。

  

14、dagger无法自动生成代码问题。

​        1、使用新的包，不用系统自带的

​        2、接入方式  使用kapt，不用annotationProcessor

​        

```
kapt  libs.dagger_android_processor
kapt  libs.dagger_compiler
implementation libs.dagger_android

-------------------------------------

dagger_android : "com.google.dagger:dagger-android:$dagger_version",
dagger_android_processor : "com.google.dagger:dagger-android-processor:$dagger_version",
dagger_compiler : "com.google.dagger:dagger-compiler:$dagger_version",
-------------------------------------------
dagger_version = "2.40"
```
