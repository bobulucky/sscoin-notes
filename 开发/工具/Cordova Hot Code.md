##Cordova代码热更新
基于Cordova框架将网页应用（html,js,css,图片等）打包成APP。这样的结果就是被打包的网页应用就变成了本地APP资源，如果APP需要更新则需要再次打包更新，此过程时效性差且周期变长让产品迭代变迟钝。
####插件Cordova Hot Code Push(CHCP)
插件Cordova Hot Code Push正是针对Cordova应用如何快速更新问题而提供的解决方案，可以自动更新web相关的静态文件。
####添加插件到项目中
1、下载插件
要求Cordova版本5.0+
```
cordova plugin add cordova-hot-code-push-plugin --save
```
2、下载插件的命令行供局cordova-hot-code-push-cli
```
npm install -g cordova-hot-code-push-cli
```
该行命令可帮助我们自动生成配置文件chcp.json和chcp.manifest,同时还提供了一些其他功能，详细可参考其README。
3、下载插件后，执行cordova platform add ios时可能会遇到如下报错：
```
Error: Cannot find module 'xml2js/lib/processors'
```
参考 xml2js is not installed 解决方法很简单：npm install xml2js


####热更新相关配置
Cordova Hot Code Push 下载安装到项目中后，需要对其进行相关的配置才能让其工作。
####插件配置
Cordova Hot Code Push 热更新插件需要两个配置文件：
* Application config：chcp.json 包含发布相关信息：热更新代码版本号，应用 native side 版本号等等
* Content manifest：chcp.manifest 包含项目热更新代码(静态)文件信息：文件名和文件哈希值   

这两个配置文件对于插件的运行缺一不可，前者描述了热更新代码的版本信息，后者提供了热更新代码文件的变更信息。借助 cordova-hot-code-push-cli 这个命令行工具可以辅助我们创建这两个配置文件。

#####Application config
> Application config holds information about the current release of the web project.   

chcp.json置于www目录根目录，例子如下：
```
{
  "name": "TestProject",
  "ios_identifier": "",
  "android_identifier": "",
  "update": "start",
  "content_url": "http://192.168.1.91:8080/chcp/www/",
  "release": "2019.05.15-15.19.11"
}
```
1、配置项
① name 项目名称
② content_url web 项目文件在服务器上的存储路径（即 www 目录在云存储中的目录路径），热更新插件将此 URL 作为 base url，用于下载配置文件和项目更新文件（必需的配置项）
③ release 描述 web 项目版本号，每一次发布的版本号必须唯一（默认使用时间戳，格式为：yyyy.MM.dd-HH.mm.ss），插件是将版本号进行字符串相等比较来判断是否存在新版本（必需的配置项）
④ min_native_interface

> Minimum version of the native side that is required to run this web content

* cordova 项目主要包含两部分：web content 和 native side。前者是网页内容，后者是 cordova 插件，为网页提供原生 API 支持，web content 的运行是基于 native side。  
* 该配置项指明 web content 运行时 native side 的最低版本。在 native side 代码有变更后（cordova 插件新增/删除，native side 版本号更新），为了确保 web content 能正常运行，需要更新 min_native_interface 的值    

在应用 config.xml 配置中可以定义了 native side 的版本号，例如
```
<chcp>
    <native-interface version="5" />
</chcp>
```
例如当前项目 native side 的版本号是5：
* 如果服务器上配置文件 chcp.json 中的 min_native_interface 值为 5，那么符合要求，热更新后的 web content 能够在正常运行  
* 如果服务器上配置文件 chcp.json 中的 min_native_interface 值为 10，高于 config.xml 文件中 <native-interface />，那么热更新将无法正常进行。此时，插件会提示错误 chcp_updateLoadFailed，提示应用需要更新升级  

⑤ update 何时触发进行安装（install）代码热更新
代码热更新涉及两个主要过程：fetch update 和 install update。前者获取热更新变更文件，后者将获取到的更新文件安装到 App 中生效。此字段是针对后者，何时 install update，可选值：
* start：应用启动，默认项（install update when application is launched）
* resume：应用从后台恢复（install the update when application is resumed from background state）
* now：下载更新后立即执行（install update as soon as it has been downloaded）
当然也可以禁用自动 install update，手动调用相关 API 进行 install
⑥ android_identifier / ios_identifier
* android_identifier: Package name of the Android version of the application
* ios_identifier: Identification number of the application
用于跳转到 Google Play Store 或者 App Store 该应用页面

2、如何生成该文件：
* 在 cordova 项目根目录执行 cordova-hcp init ，会通过命令行交互的方式，提示输入配置有关信息，创建该文件，会在项目根目录创建一个默认 Application config 文件 cordova-hcp.json
* 然后在每次应用打包时，再执行 cordova-hcp build 即可在 web 项目 www 根目录生成一个 chcp.json 文件。

Content manifest
> Content manifest describes the state of the files inside your web project.  

通过执行 cordova-hcp build 在 www 根目录自动生成 chcp.manifest 文件
```
[
  {
    "file": "css/index.css",
    "hash": "46df03526f60473bf81b113d600a6b00"
  },
  {
    "file": "home.html",
    "hash": "ef071787d0def9abc8f692fb683ac4dd"
  },
  {
    "file": "img/logo.png",
    "hash": "7e34c95ac701f8cd9f793586b9df2156"
  },
  {
    "file": "index.html",
    "hash": "366a88c501aec796ca86d7b83a3e2603"
  },
  {
    "file": "js/index.js",
    "hash": "b144c071205225b243caacc5b550f592"
  }
]
```
chcp.manifest 文件中包含的是 web content 静态文件信息，每一个项都包括两个字段：
* file: 相对于 www 目录的文件路径
* hash: 文件的 MD5 哈希值，用于判断文件是否发生变更

基于 chcp.manifest 文件
* 在 fetch update 阶段，从服务器上获取新增、修改文件
* 在 install update 阶段，移除被删除文件

####Cordova config.xml配置
Cordova 项目的 config.xml 文件用于设置项目配置选项，Cordova Hot Code Push 热更新插件的配置项也需要在该文件中进行相应的配置。
```
<chcp>
		<auto-download enabled="true"/>
		<auto-install enabled="true"/>
		<config-file url="http://192.168.1.91:8080/chcp/www/chcp.json"/>
		<native-interface version="1" />
</chcp>
```
* config-file：配置文件 chcp.json 从服务器上加载的路径（必须的配置项）
* auto-download：是否自动下载热更新代码，默认是 true
* auto-install：是否自动安装热更新代码，默认是 true
* native-interface：当前 native side 的版本号

可以禁用自动下载，安装热更新代码，通过手动调用执行。

##代码热更新原理
####热更新流程
------------
① 应用启动
② 热更新插件初始化，并在后台加载更新模块 (update loader)
③ 更新模块 (update loader) 从 Cordova 项目配置 config.xml 文件中获取 config-file （热更新插件配置文件 chcp.json 的加载路径），然后加载配置文件 chcp.json，获取其中的 release 版本号，对比当前的版本号，若二者不同，说明有新版本，执行下一步
④ 更新模块 (update loader) 从 chcp.json 配置文件中获取 content_url 作为 base url，然后加载 chcp.manifest 文件，或者新版本文件变更信息
⑤ 更新模块 (update loader) 根据 content_url 作为 base url，下载所有变更、新增文件
⑥ 如果一切顺利， 更新模块 (update loader) 发送通知，该更新已准备好进行安装
⑦ 安装更新，应用重定向到新版本页面

####Cordova web project 存储与更新
Cordova 项目中都包含一个 www 目录，存储网页静态文件，Cordova 打包移动应用时，会将其拷贝到各自的项目目录，同时会被打包到应用中。
* Android: platforms/android/assets/www.
* iOS: platforms/ios/www.

www 目录打包到应用中之后，我们就没办法对其进行更新了，因为只有可读权限。为了解决这一问题，在应用第一次启动的时候，从应用 bundle 中加载网页内容的同时，将 www 目录拷贝到外部目录中，在后续应用启动时，都从这个外部存储的静态文件中加载文件，而对于外部的这个存储目录，我们就有读写权限，这样就为我们动态更新网页代码提供了可能。

在 safari 调试页面执行 cordova.file.applicationStorageDirectory 可以得到应用的存储路径，点击可以打开 Finder 目录。
从 Library/Application Support 目录下就可以找到存储 web content 的外部目录

-----

-----
Cordova Hot Code Push 插件为每一个版本内容都创建了一个对应的目录，以配置文件 chcp.json 中 release 字段值为目录名，存放不同版本 www 目录中的静态文件，这种处理方式的好处是：
* 避免了文件缓存问题。例如 iOS UIWebView 缓存 css 文件，即使刷新页面，也不会清除缓存，除非重启应用才能强制清除缓存。不同版本置于不同的目录，由于加载路径不同，这样就可以解决文件的缓存问题
* 避免在更新代码文件时，和当前已有文件出现冲突
* 方便回滚到前一个版本

下面了解一下，获取更新内容和安装更新内容时都发生了什么？
1、**获取更新内容**
* 根据 release 版本号，创建一个新的目录
* 在新目录中，创建 update 目录，根据 chcp.manifest 文件，将所有变更、新增文件下载到该目录中
* 新版本对应的 chcp.json 和 chcp.manifest 文件也会置于 update 目录中

2、**安装更新内容**
* 将当前版本对应目录下的 www 目录拷贝到新版本对应的目录下
* 在新版本对应目录下，将 update 目录中变更、新增文件拷贝到 www 目录中，同时根据 chcp.manifest 移除被删除文件
* 移除 update 目录
* 应用重定向到新版本目录下加载网页

####插件 JS 接口
默认情况下，Cordova Hot Code Push (CHCP) 插件不需要额外的代码，就可以自动执行 checking->downloading->installation 这个更新循环。当然也可以通过其提供的接口来控制这更新流程，这时，我们需要在项目 config.xml 文件中配置 auto-download 和 auto-install 为 false
```
<chcp>
  <auto-download enabled="false"/>
  <auto-install enabled="false"/>
  <config-file url="http://192.168.1.91:8080/chcp/www/chcp.json"/>
  <native-interface version="1" />
</chcp>
```
####事件监听
Cordova Hot Code Push 插件提供了一系列事件监听，方便我们对不同情况进行不同的处理。例如：chcp_updateInstalled 事件，当更新安装完成时会发出这个通知；chcp_updateInstallFailed 事件，当更新安装失败时发出这个通知，等等。

值得注意的是，需要在 deviceready 事件回调后，才进行 CHCP 插件的事件监听注册
```
var app = {
  // Application Constructor
  initialize: function() {
    this.bindEvents();
  },
  // Bind any events that are required.
  // Usually you should subscribe on 'deviceready' event to know, when you can start calling cordova modules
  bindEvents: function() {
    document.addEventListener('deviceready', this.onDeviceReady, false);
    document.addEventListener('chcp_updateIsReadyToInstall', this.onUpdateReady, false);
  },
  // deviceready Event Handler
  onDeviceReady: function() {
    console.log('Device is ready for work');
  },
  // chcp_updateIsReadyToInstall Event Handler
  onUpdateReady: function() {
    console.log('Update is ready for installation');
  }
};
app.initialize();
```
详细事件监听列表参考文档：[Listen for update events](https://github.com/nordnet/cordova-hot-code-push/wiki/Listen-for-update-events)

####获取/安装更新
① fetch update chcp.fetchUpdate
调用 API 从服务器中获取更新
```
function fetchUpdate(cb) {
    var options = {
        'config-file': 'https://kss.ksyun.com/******/chcp.json'
    };
    chcp.fetchUpdate(updateCallback, options);
    function updateCallback(error, data) {
        if (error) {
            console.log('--fetchUpdate error--', error.code, error.description);
        }
        console.log('--fetchUpdate--', data, data.config);
        cb && cb(error, data);
    }
}
```
② install update chcp.installUpdate
调用 API 安装更新
```
function installUpdate(cb) {
    chcp.installUpdate(installationCallback);
    function installationCallback(error) {
        if (error) {
            console.log('Failed to install the update with error code: ' + error.code);
            console.log(error.description);
        } else {
            console.log('Update installed!');
        }
        cb && cb(error);
    }
}
```
在安装更新之前，还需要检测是否有更新可用于安装
chcp.isUpdateAvailableForInstallation
```
function checkIsUpdateAvailableForInstallation(cb) {
    chcp.isUpdateAvailableForInstallation(callbackMethod);
    function callbackMethod(error, data) {
        if (error) {
            console.log('No update was loaded => nothing to install');
        } else {
            console.log('Current content version: ' + data.currentVersion);
            console.log('Ready to be installed:' + data.readyToInstallVersion);
        }
        cb && cb(error, data);
    }
}
```
####获取版本信息
```
function getVersionInfo(cb) {
    chcp.getVersionInfo((err, data) => {
        console.log('Current web version: ' + data.currentWebVersion);
        console.log('Previous web version: ' + data.previousWebVersion);
        console.log('Loaded and ready for installation web version: ' + data.readyToInstallWebVersion);
        console.log('Application version name: ' + data.appVersion);
        console.log('Application build version: ' + data.buildVersion);
        cb && cb(err, data);
    });
}
```
####错误代码
在下载，安装更新过程中都有可能出现错误，详细的错误代码参考：[Error codes](https://github.com/nordnet/cordova-hot-code-push/wiki/Error-codes)
####请求到应用商店进行 APP 升级
插件配置文件 chcp.json 中 min_native_interface 选项是网页内容执行时要求 native side 最低版本号。每一次热更新过程中，都会去检查这个逻辑，判断当前 native side 的版本是否符合要求。如果当前 APP 中的 native side 版本号低于 chcp.json 中 min_native_interface 的选项值，那么执行热更新就会提示错误：chcp.error.APPLICATION_BUILD_VERSION_TOO_LOW，这个时候，我们应当提示用户前往应用商店对 APP 进行升级。

恰当的处理方式是，在出现 chcp.error.APPLICATION_BUILD_VERSION_TOO_LOW 错误时，弹框提示用户前往应用商店进行升级，弹框有两个按键：一个点击后跳转到应用商店该 APP 对应下载页面；另一个点击后关闭弹框。插件也提供了 API 处理这个过程，我们只需：
* 在 chcp.json 配置文件中设置 android_identifier 和 ios_identifier
* 调用 chcp.requestApplicationUpdate 方法

监听 chcp_updateLoadFailed 事件，判断错误代码为 chcp.error.APPLICATION_BUILD_VERSION_TOO_LOW 时，调用 chcp.requestApplicationUpdate 方法。

```
var app = {
  // Application Constructor
  initialize: function() {
    this.bindEvents();
  },
  // Bind any events that are required.
  // Usually you should subscribe on 'deviceready' event to know, when you can start calling cordova modules
  bindEvents: function() {
    document.addEventListener('deviceready', this.onDeviceReady, false);
    document.addEventListener('chcp_updateLoadFailed', this.onUpdateLoadError, false);
  },
  // deviceready Event Handler
  onDeviceReady: function() {
  },
  onUpdateLoadError: function(eventData) {
    var error = eventData.detail.error;
    if (error && error.code == chcp.error.APPLICATION_BUILD_VERSION_TOO_LOW) {
        console.log('Native side update required');
        var dialogMessage = 'New version of the application is available on the store. Please, update.';
        chcp.requestApplicationUpdate(dialogMessage, this.userWentToStoreCallback, this.userDeclinedRedirectCallback);
    }
  },
  userWentToStoreCallback: function() {
    // user went to the store from the dialog
  },
  userDeclinedRedirectCallback: function() {
    // User didn't want to leave the app.
    // Maybe he will update later.
  }
};
app.initialize();
```
####[Usage Limitations](https://github.com/nordnet/cordova-hot-code-push/wiki/Usage-Limitations)

1、Don’t rename/delete/move your index page
Cordova 项目 config.xml 文件中都会定义一个入口页面 index.html：
```
<content src="index.html" />
```
应用启动的时候，就会加载 index.html 页面作为入口，在代码热更新过程中，这是唯一不能删除，移动和重命名的文件，否则，代码热更新后，应用就无法正常加载到 index.html 入口页面，所以会出错。

诚然，如果你需要重命名，或者修改其存储路径，那么需要在 config.xml 文件中修改 content 配置。

2、Do not clean plugin’s inner preferences with cordova-plugin-nativestorage
cordova-plugin-nativestorage 插件提供了读写本地存储数据的能力，例如在 iOS 中对应的本地存储是 NSUserDefault，CHCP 热更新插件在其中存储了一些属性。

调用 cordova-plugin-nativestorage 插件中的 NativeStorage.clear() 方法会清除本地存储数据，这就会影响到 CHCP 插件的正常运行，导致下一次应用启动时加载的是应用 bundle 中 www 目录中的网页内容，而非外部目录存储的当前版本网页内容。

####将 www 目录打包上传到服务器或者云存储目录
新版本发布时，都需要执行如下处理：
* 对 www 目录下的静态文件进行打包，包括代码压缩，合并等等
* 执行 cordova-hcp build 生成 chcp.json 和 chcp.manifest 文件
* 将 www 目录下的静态文件上传至服务器或者云存储目录

------------------------

> [https://objcer.com/2017/06/18/cordova-hot-code-push/](https://objcer.com/2017/06/18/cordova-hot-code-push/)
