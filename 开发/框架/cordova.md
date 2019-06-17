####插件开发
**创建插件**
```
plugman create --name nativeLocation --plugin_id com.kit.cordova.nativeLocation --plugin_version 0.0.1
cd nativeLocation\
plugman createpackagejson ./
plugman platform add --platform_name android
plugman platform add --platform_name ios
```
**安装插件**
安装插件到准备好的cordova项目中
```cordova plugin add D:\nativeLocation```
