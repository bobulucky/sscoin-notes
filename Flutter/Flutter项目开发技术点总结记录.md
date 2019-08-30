刚刚使用Flutter开发完一个项目，对项目来说不大基本功能都会有，主流的一些插件都有使用到。现在回过来做一个项目复盘，针对项目使用的一些插件和遇到的一些问题进行分析总结以作记录。
#### 网络框架+路由导航+状态管理+json数据序列化
#### Android打包release版本之Androidx支持包兼容问题
### 项目中使用的主要功能插件
* 路由导航 fluro
* 状态管理 provider
* 网络框架 Dio
* JSON和数据序列化

### 路由导航之fluro
使用fluro对路由进行统一配置、统一管理，应用开发使用很是方便。

##### 配置路由
创建routes.dart类进行路由配置。
APP打开的第一个页面路径必须是'/',这里涉及到两个参数:
* routePath:对应页面路径
* handler：对应打开页面的handler

``` dart
class Routes {
  static String root = "/";
  static String demoSimple = "/demo";

  static void configureRoutes(Router router) {
    router.notFoundHandler = Handler(
        handlerFunc: (BuildContext context, Map<String, List<String>> params) {
      print("ROUTE WAS NOT FOUND !!!");
      return;
    });
    router.define(root, handler: rootHandler);
    router.define(demoSimple, handler: demoRouteHandler);
  }
}
```

##### 构建页面handler
创建route_handler.dart进行页面handler配置。
不需要传递参数的页面使用rootHandler的写法，如果要传递参数按demoRouteHandler的写法。
fluro有个问题不能传递中文，需要编码解决传递。

``` dart
var rootHandler = Handler(
    handlerFunc: (BuildContext context, Map<String, List<String>> params) {
  return HomePage();
});

var demoRouteHandler = Handler(
    handlerFunc: (BuildContext context, Map<String, List<String>> params) {
  String message = params["message"]?.first;
  String colorHex = params["color_hex"]?.first;
  String result = params["result"]?.first;
  Color color = Color(0xFFFFFFFF);
  if (colorHex != null && colorHex.length > 0) {
    color = Color(ColorHelpers.fromHexString(colorHex));
  }
  return DemoSimpleComponent(message: message, color: color, result: result);
});
```

##### 初始化fluro路由初始化
在main.dart进行初始化
``` dart
    final router = Router();
    Routes.configureRoutes(router);
    Application.router = router;
```

##### 执行页面跳转
``` dart
Application.router.navigateTo(context, '${Routes.demoSimple}?message=xxx&colorHex=xxx');
```
##### 解决中文不能传递问题
将中文参数在传递前进行解码转换，传递后取出参数解析
``` dart
/// fluro 参数编码解码工具类
class FluroConvertUtils {
  /// fluro 传递中文参数前，先转换，fluro 不支持中文传递
  static String fluroCnParamsEncode(String originalCn) {
    StringBuffer sb = StringBuffer();
    var encoded = Utf8Encoder().convert(originalCn);
    encoded.forEach((val) => sb.write('$val,'));
    return sb.toString().substring(0, sb.length - 1).toString();
  }

  /// fluro 传递后取出参数，解析
  static String fluroCnParamsDecode(String encodedCn) {
    var decoded = encodedCn.split('[').last.split(']').first.split(',');
    var list = <int>[];
    decoded.forEach((s) => list.add(int.parse(s.trim())));
    return Utf8Decoder().convert(list);
  }
}
```

### provider
使用provider插件包对APP状态进行管理。实现provider对状态进行管理分为三部分：
* ChangeNotifier
* ChangeNotifierProvider
* Consumer

##### ChangeNotifier
ChangeNotifier它是Flutter开发包中的一个简单类，为监听器提供更改通知。
ChangeNotifier 是封装应用状态的一种方法。对于非常简单的应用程序，可以使用一个ChangeNotifier。在复杂的模型中，你将有几个模型，因此有几个ChangeNotifiers。

创建model继承ChangeNotifier

``` dart
class CartModel extends ChangeNotifier {
  /// Internal, private state of the cart.
  final List<Item> _items = [];

  /// An unmodifiable view of the items in the cart.
  UnmodifiableListView<Item> get items => UnmodifiableListView(_items);

  /// The current total price of all items (assuming all items cost $42).
  int get totalPrice => _items.length * 42;

  /// Adds [item] to cart. This is the only way to modify the cart from outside.
  void add(Item item) {
    _items.add(item);
    // This call tells the widgets that are listening to this model to rebuild.
    notifyListeners();
  }
}
```
这里特定于ChangeNotifier代码就是notifyListeners()。只要模型更改调用此方法UI就会更改。

##### ChangeNotifierProvider
ChangeNotifierProvider是一个widget，为其后台widget提供ChangeNotiffier实例。  
ChangeNotifier至于必要的范围至上，不要污染范围。
``` dart
void main() {
  runApp(
    ChangeNotifierProvider(
      builder: (context) => CartModel(),
      child: MyApp(),
    ),
  );
}
```
如果需要提供多个类，可以使用MultiProvider:
``` dart
void main() {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(builder: (context) => CartModel()),
        Provider(builder: (context) => SomeOtherClass()),
      ],
      child: MyApp(),
    ),
  );
}
```

##### Consumer
现在CartModel通过顶部的ChangeNotifierProvider声明提供给我们应用程序中的小部件，我们可以开始使用它。
``` dart
return Consumer<CartModel>(
  builder: (context, cart, child) {
    return Text("Total price: ${cart.totalPrice}");
  },
);
```
指定想要访问的模型类型，在这里我们需要CartModel，因此写为Consumer<CartModel>。

##### Provider.of
有时，并不真正需要模型中的数据来更改UI，但仍然需要访问它。
``` dart
Provider.of<CartModel>(context, listen: false).add(item);
```
### http请求库Dio
dio是一个强大的Dart Http请求库，支持Restful API、FormData、拦截器、请求取消、Cookie管理、文件上传/下载、超时、自定义适配器等...

发起一个Get请求：
``` dart
Response response;
Dio dio = new Dio();
response = await dio.get("/test?id=12&name=wendu");
print(response.data.toString());
// 请求参数也可以通过对象传递，上面的代码等同于：
response = await dio.get("/test", queryParameters: {"id": 12, "name": "wendu"});
print(response.data.toString());
```

发起一个Post请求：
``` dart
response = await dio.post("/test", data: {"id": 12, "name": "wendu"});
```
以流的方式接收响应数据：
``` dart
Response<ResponseBody> rs = await Dio().get<ResponseBody>(url,
  options: Options(responseType: ResponseType.stream), //设置接收类型为stream
);
print(rs.data.stream); //响应流
```

以二进制数组的方式接收响应数据：
``` dart
Response<List<int>> rs = await Dio().get<List<int>>(url,
 options: Options(responseType: ResponseType.bytes), //设置接收类型为bytes
);
print(rs.data); //二进制数组
```

发送 FormData:
``` dart
FormData formData = new FormData.from({
    "name": "wendux",
    "age": 25,
  });
response = await dio.post("/info", data: formData);
```

### 使用代码生成库序列化JSON
json_serializable包是一个自动生成源代码生成器，可以生成JSON序列化样板。

##### 在项目中设置json_serializable
要在项目中包含json_serializable，需要一个常规依赖和两个dev依赖项。dev依赖项是我们项目源代码中未包含的依赖项-它们仅在开发环境中使用。
**pubspec.yaml**
``` dart
dependencies:
  # Your other regular dependencies here
  json_annotation: ^2.0.0

dev_dependencies:
  # Your other dev_dependencies here
  build_runner: ^1.0.0
  json_serializable: ^2.0.0
```
在项目根目录下运行flutter pub以在项目中使用这些新的依赖项。

##### 以json_serializable方式创建模型类
**user.dart**
``` dart
import 'package:json_annotation/json_annotation.dart';

/// This allows the `User` class to access private members in
/// the generated file. The value for this is *.g.dart, where
/// the star denotes the source file name.
part 'user.g.dart';

/// An annotation for the code generator to know that this class needs the
/// JSON serialization logic to be generated.
@JsonSerializable()

class User {
  User(this.name, this.email);

  String name;
  String email;

  /// A necessary factory constructor for creating a new User instance
  /// from a map. Pass the map to the generated `_$UserFromJson()` constructor.
  /// The constructor is named after the source class, in this case, User.
  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);

  /// `toJson` is the convention for a class to declare support for serialization
  /// to JSON. The implementation simply calls the private, generated
  /// helper method `_$UserToJson`.
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```
如果需要，还可以轻松自定义命名策略。例如，如果API返回带有snake_case的对象，并且您希望在模型中使用lowerCamelCase，则可以将@JsonKey批注与name参数一起使用：
``` dart
/// Tell json_serializable that "registration_date_millis" should be
/// mapped to this property.
@JsonKey(name: 'registration_date_millis')
final int registrationDateMillis;
```

##### 生成实用文件
在第一次创建json_serializable类时，您将得到类似于下图所示的错误。

![](https://flutter.dev/images/json/ide_warning.png)  

这些错误完全正常，仅仅是因为模型类的生成代码尚不存在。要解决此问题，请运行生成序列化样板的代码生成器

有两种运行代码生成器的方法。
###### One-time code generation
通过在项目根目录中运行flutter pub run build_runner build，可以在需要时为模型生成JSON序列化代码。这会触发一次性构建，该构建遍历源文件，选择相关文件，并为它们生成必要的序列化代码。   
虽然这很方便，但如果您不必每次在模型类中进行更改时都必须手动运行构建，那将是很好的
###### Generating code continuously
观察者使我们的源代码生成过程更加方便。它会监视项目文件中的更改，并在需要时自动构建必要的文件。通过在项目根目录中运行flutter pub run build_runner watch来启动观察程序。   
启动观察器一次并使其在后台运行是安全的。

##### 使用json_serializable模型
要以json_serializable方式解码JSON字符串，您实际上没有对我们以前的代码进行任何更改。
``` dart
Map userMap = jsonDecode(jsonString);
var user = User.fromJson(userMap);
```
编码也是如此。调用API与以前相同。
``` dart
String json = jsonEncode(user);
```
使用json_serializable，您可以忘记User类中的任何手动JSON序列化。源代码生成器创建一个名为user.g.dart的文件，该文件具有所有必需的序列化逻辑。您不再需要编写自动化测试来确保序列化工作 - 现在libarary有责任确保序列化正常工作。

##### 生成嵌套类的代码
考虑以下Address类：
``` dart
import 'package:json_annotation/json_annotation.dart';
part 'address.g.dart';

@JsonSerializable()
class Address {
  String street;
  String city;

  Address(this.street, this.city);

  factory Address.fromJson(Map<String, dynamic> json) => _$AddressFromJson(json);
  Map<String, dynamic> toJson() => _$AddressToJson(this);
}
```
Address类嵌套在User类中：
``` dart
import 'address.dart';
import 'package:json_annotation/json_annotation.dart';
part 'user.g.dart';

@JsonSerializable()
class User {
  String firstName;  
  Address address;

  User(this.firstName, this.address);

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```
在终端中运行flutter pub run build_runner build会创建* .g.dart文件，但private _ $ UserToJson（）函数类似于以下内容：
``` dart
(
Map<String, dynamic> _$UserToJson(User instance) => <String, dynamic>{      
  'firstName': instance.firstName,
  'address': instance.address,      
};
```
所有看起来都很好，但如果你对用户对象执行print（）：
``` dart
Address address = Address("My st.", "New York");
User user = User("John", address);
print(user.toJson());
```
结果却是：
``` dart
{name: John, address: Instance of 'address'}
```
想要的结果可能是这样：
``` dart
{name: John, address: {street: My st., city: New York}}
```
要使其工作，请在类声明的@JsonSerializable（）注释中传递explicitToJson：true。 User类现在看起来如下：
``` dart
import 'address.dart';
import 'package:json_annotation/json_annotation.dart';
part 'user.g.dart';

@JsonSerializable(explicitToJson: true)
class User {
  String firstName;  
  Address address;

  User(this.firstName, this.address);

  factory User.fromJson(Map<String, dynamic> json) => _$UserFromJson(json);
  Map<String, dynamic> toJson() => _$UserToJson(this);
}
```

### Flutter打包release Androidx兼容问题
项目在最后开发接近尾声时，我试着打包release包，发现了Androidx兼容的问题。问题原因就是项目中同时使用到了Androidx包和Support包，而这两种包只能存在一个，要么全部引用Androidx包，要么全部引用Support包。   
解决Androidx包冲突有两种方式：
* 全部引用Androidx包
* 全部引用Support包   

这里我使用的是第一种，建议也是使用第一种，官方建议使用Androidx,以后的第三包都会转到使用androidx的。  
转Androidx包的方法，官方提供了两种方法，一种是自动转（官方推荐），一种是手动转。

##### 使用Android Studio自动升级Androidx包
这里对Android studio版本的要求是3.2以上。
项目上右键android文件夹，Flutter -> Open Android module in Android Studio    
在新建的Android Studio窗口选择Refactor -> Migrate to AndroidX    
点击Do Refactor即可完成AndroidX的迁移工作。  

##### 手动升级Androidx
1. 打开android/gradle/wrapper/gradle-wrapper.properties，修改distributionUrl的值
```
distributionUrl=https\://services.gradle.org/distributions/gradle-4.10.2-all.zip
```
2. 打开android/build.gradle，修改com.android.tools.build:gradle 到3.3.0
```
dependencies {
    classpath 'com.android.tools.build:gradle:3.3.0'
}
```
3. 打开android/gradle.properties，添加两行
```
android.enableJetifier=true
android.useAndroidX=true
```
4. 打开android/app/build.gradle，在android{}里面，确保compileSdkVersion 和 targetSdkVersion 值为28   
替换所有过时的依赖库android.support到androidx。
