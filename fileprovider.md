FileProvider重复
这个也出现在模块话开发中，或是引用的三方库中也定义了FileProvider，就会报FileProvider重复的错误。

`Attribute provider#android.support.v4.content.FileProvider@authorities value=(***.fileProvider) from AndroidManifest.xml:352:13-62 is also present at ...
`

解决方法也很简单，就是定义一个我们自己的FileProvider：

```java
public class MyFileProvider extends FileProvider {
}
```

是的，其他什么也不用干，直接继承FileProvider创建一个自己的FileProvider就好

然后，AndroidManifest文件中定义的FileProvider的name属性改成上面的MyFileProvider的路径就好

```xml
<provider
    android:name=".xtreme.provider.MyFileProvider" //自定义的FileProvider的路径
    android:authorities="${applicationId}.myfileprovider"
    android:grantUriPermissions="true"
    android:exported="false">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_paths" />
</provider>
```