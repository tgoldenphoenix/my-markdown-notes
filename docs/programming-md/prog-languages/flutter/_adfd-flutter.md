# 06: ADFD - Flutter & dart

[link MEGA](https://mega.nz/folder/rmgk3AgD#FibJqhu3yd300DwHLZ1SOA)

[link google sheet](https://docs.google.com/spreadsheets/d/1CnFiUr6hnf77XrO0z8a6g7vTvHKYNq1sGZMIc2okduM/edit?gid=0#gid=0) phải dùng mail FPT

bài 07, 08 chỉ phục vụ project, trước đó là thi qua môn xong rồi

## Thi qua môn

Dart SDK path: `/Users/anhao/Development/flutter/bin/cache/dart-sdk`

login, floating button (or toolbar dấu +), hiển thị dialog nhập login
tab-bar vs tabar view\
drawer (ngăn kéo)\
button navigation bar\
radio button, drop-down, checkbox

validation, sql lite, không call API

đi thi dùng add page dễ hơn show dialog (giống login)

thi chỉ có 1 bảng

nội dung thi
crud, Search, filter
drawer, tab bar, navigation button/bar
list view, card, list tile
add new: page or dialog

thi thực hành:\
UI: FormTextEditingControl, radio, slider, Drop, dropdown select options, alertDialog,…\
layout: drawer, buttonNavigationBar, TabView, button\
Process: CRUD, search, filter, Login, validation, sqflite

# Flutter & Dart notes

video phone auth 14:55

## Overview

Flutter dùng Dart language, có syntax giống C. One code base for multiple platform.

Flutter's most useful development features: Stateful Hot Reload. Flutter can't hot-reload web applications.

To start using Flutter, you need install:
1. Flutter SDK
2. An IDE (Visual Studio Code with the Flutter plugin)
3. The software required by your chosen development target (for example: Visual Studio to target Windows, or Xcode to target macOS)

The `pubspec.yaml` file specifies basic information about your app, such as its current version, its dependencies, and the assets with which it will ship. Nó giống `package.json` bên Node js.

The file `analysis_options.yaml` determines how strict Flutter should be when analyzing your code.

Widgets are the elements from which you build every Flutter app. As you can see, even the app itself is a widget. One explanation [here](https://stackoverflow.com/questions/63639016/how-widgets-are-different-from-ui-components). Almost everything in Flutter are widgets.

Router trong flutter gọi là **NavigationRail**

Some state is only relevant to a single widget, so it should stay with that widget. You should not store all states inside MyAppState.

The `build()` method in each widget will be automatically called every time the widget's circumstances change so that the widget is always up to date.

To upgrage a widget to become more complex, it's a good idea to extract it into a separate widget. And then use refactor menu to wrap more layout widgets.

Trong flutter mình không viết css, không dùng library class="" để styling. Mình styling 100% bằng widgets.

## Stateless vs Stateful widgets

[Flutter widgets 101](https://www.youtube.com/watch?v=CXedqMlLo7M&list=PL-B0-NEYQT8dMVGTVYKv0sv_NOg--wtx1)

[why is root element stateless in flutter?](https://stackoverflow.com/questions/59170761/should-flutter-root-widget-always-be-statelesswidget)

Variables inside Stateless widgets are immutable and not tracked. They are `final`. But you can still pass variable into stateless widgets.

Constructor inside stateless widget are called [constant constructor](https://dart.dev/language/constructors#constant-constructors)

Stateful widgets create **state objects**. State objects contain variables that you want to be updated. The state object is the one building the child widget not the stateful widget.

Class stateful widget có gọi function `createState`. Stateless không có gọi.

[Inherited Widgets](https://www.youtube.com/watch?v=Zbm3hjPjQMk)

## shared_preferences package

[this video](https://www.youtube.com/watch?v=sa_U0jffQII) cover the basic

lưu user_theme_preference (light_theme or dark_theme) để khi user restart app không bị reset to light_theme default.

## Provider

[What is provider?](https://www.youtube.com/watch?v=uQuxrZE2dqA) Trong React thì có context API & redux

[This video](https://www.youtube.com/watch?v=FUDhozpnTUw&t=3s) cover the basic very good

[Provider vs Riverpod](https://riverpod.dev/docs/from_provider/provider_vs_riverpod)

`context.watch` listen to changes, `context.read` do not

Provider save you from manually writing InheritedWidget

## TextField Widget

Text() widget thì dùng TextStyle(). Còn ElevatedButton() thì dùng ButtonStyle().

## Other widgets

[MaterialApp](https://www.geeksforgeeks.org/materialapp-class-in-flutter/) class in Flutter. Tất cả file .dart đều import `material.dart`

[Here](https://stackoverflow.com/questions/55411685/what-is-the-difference-between-scaffold-and-materialapp-in-flutter) is the difference between Scaffold and MaterialApp in Flutter:\

## CLI commands

`flutter doctor` để kiểm tra tình trạng, version, etc liên quan tới flutter.

`flutter devices`; `flutter emulators`

[Where does `pub get` download pubspec dependencies to?](https://stackoverflow.com/questions/61628388/where-does-pub-get-download-pubspec-dependencies-to)

`flutter pub add PLUGIN_NAME` [plugin names](https://firebase.google.com/docs/flutter/setup?platform=android#available-plugins)

Khi tải 1 project flutter trên internet nếu gặp lỗi dependency URI not found thì chạy lệnh `flutter pub get`

`flutter --version` & `dart --version`

## Hotkeys

`stl` create a stateless widget in VSCode.
`stfl` stateful widget

Mở command pallet VSCode `F1`. `S Cmd P` của mình không dùng được vì dính Yabai.

`F5` để chạy flutter project in VSCode.

Open the **refactor menu** in VSCode: move your cursor to the widget name you want, and press `Cmd+.`

copyWith() lets you change a lot more about the text style than just the color. To get the full list of properties you can change, put your cursor anywhere inside copyWith()'s parentheses, and hit Ctrl+Shift+Space (Win/Linux) or `Cmd+Shift+Space` (Mac).

## Flutter FAQs

[how to](https://codelabs.developers.google.com/codelabs/flutter-codelab-first#2) create a new flutter project.

[Spring boot vs Fire base for API](https://stackoverflow.com/questions/61350500/is-spring-boot-required-with-firebase-for-android#:~:text=Firebase%20is%20basically%20a%20service,storage%20and%20API%20usage%20stuff) => Spring boot is harder and require more coding of your own, firebase cost more money but less code writing.

[local state vs App-wide state](https://www.geeksforgeeks.org/flutter-local-state-vs-app-wide-state/) 
There are many way to manage app state in Flutter such as:
1. ChangeNotifier

Android Studio uses Gradle, an advanced build toolkit, to automate and manage the build process. Đó là lí do trong android/ directory có gradle files. iOS không có gradle files.

[_method that start with underscore in Dart](https://stackoverflow.com/questions/53142171/what-does-underscore-before-variable-name-mean-for-flutter)

### Installation

Android Studio SDK Folder: /Users/anhao/Library/Android/sdk

Doc cài flutter cho Android app [here](https://docs.flutter.dev/get-started/install/macos/mobile-android). Run `flutter doctor` to check.

Hướng dẫn add flutter path vào $PATH [here](https://stackoverflow.com/questions/50652071/flutter-command-not-found)

If you've installed or plan to [install the Flutter SDK](https://docs.flutter.dev/get-started/install), it includes the full Dart SDK. The Flutter SDK includes the [`dart`](https://dart.dev/tools/dart-tool) CLI tool in Flutter's `bin` folder.

## The Dart Language

`!.` [Casting away nullability](https://dart.dev/null-safety/understanding-null-safety#non-null-assertion-operator)

_Runtime failures suck._ This is especially true in a language like Dart that is designed to run on an end-user's device. If a server application fails, you can often restart it before anyone notices. But when a Flutter app crashes on a user's phone, they are not happy. When your users aren't happy, you aren't happy. This is why you need statically-typed languages and [null safety](https://dart.dev/null-safety/understanding-null-safety)

[pub](https://stackoverflow.com/questions/56788324/what-does-pub-in-flutter-stands-for) is the official package manager for Dart AND Flutter apps.

A [constant context](https://dart.dev/resources/glossary#constant-context) is a region of code in which it isn't necessary to include the const keyword because it's implied by the fact that everything in that region is required to be a constant.

### Variable

A **compile-time constant** is computed at the time the code is compiled, while a run-time constant can only be computed while the application is running. A compile-time constant will have the same value each time an application runs, while a run-time constant may change each time. Compile-time constants are required for cases such as array bounds, case expressions, or enumerator initializers.

What's the difference between `double.infinity` and `double.maxFinite` in Dart? [here](https://stackoverflow.com/questions/61706455/whats-the-difference-between-double-infinity-and-double-maxfinite-in-dart)

### Collection

List = array, ordered group of objects\
Dùng `[]`, `.add()`, `.remove()`

A `set` in Dart is an unordered collection of unique items\
dùng `{}`, `.add()`, `.remove()`

remove item trong list & set hơi khác chút.

`map` (commonly known as a dictionary or hash) stores key-value pair

 The `dart:collection` library includes Classes and utilities that supplement the collection support in `dart:core`. Trong này ngoài set, map, list thì có HashMap, HashSet

### Async in Dart

`VoidCallback` is a signature of callbacks that have no arguments and return no data.

### Classes & Object

The `new` keyword is optional when creating objects in Dart.

[class modifider](https://dart.dev/language/class-modifiers)\
`abstract` là interface

### Operators

Syntax basics>	operators

`~/` Integer division operator: Divide, returning an integer result.

`String? name` gọi là [enable nullability](https://dart.dev/language/variables#null-safety). Có ? là Nullable, không có ? thì Non-nullable type

### Mixin

In object-oriented programming languages, a [mixin](https://en.wikipedia.org/wiki/Mixin) (or mix-in) is a class that contains methods for use by other classes without having to be the parent class of those other classes. How those other classes gain access to the mixin's methods depends on the language. Mixins are sometimes described as being "included" rather than "inherited". 

### Constructors

Language>Classes& objects>constructor

Dart implements many types of constructors. Except for default constructors, these functions use the same name as their class.\
Constructor syntax cũng rất khác Java.

`Key` is a type used in Flutter to identify widgets and allows Flutter to know when a widget that's moved in the tree is the same as a widget that was previously in a different location. There's a good video about keys [here](https://www.youtube.com/watch?v=kn0EOS-ZiIc)\
The official doc for key is [here](https://docs.flutter.dev/ui#keys)

[The colon operator after Constructor in dart](https://sarunw.com/posts/dart-initialize-variables-constructor-body/) can be any of the below:\
collection literal
named parameter
[Initializer lists](https://dart.dev/language/constructors#use-an-initializer-list)
Redirecting constructors

[Dart Constructors in Inheritance](https://www.youtube.com/watch?v=5o0FDJQkhr0)

### Getter & setter

[Dart GETTER and SETTER | Private Instance variables](https://www.youtube.com/watch?v=yzQkSwXkjgg)

Doc thì vào Language>Classes& objects>[Methods](https://dart.dev/language/methods#getters-and-setters) (nó khó tìm vcl)

Trong dart nếu tạo custom getter & setter cho 1 instance variable thì có để delete luôn cái dòng khai báo biến.

You can use the arrow syntax `=> expr` inside getter. Khỏi dùng `{ return expr; }`

Calling getters & setters in other class syntax y change access property bình thường luôn. Nhìn không biết là getter & setter.

## Terminologies

AVD: Android Virtual Device

SDK: Software development kit

Business Logic [here](https://en.wikipedia.org/wiki/Business_logic)

Android .apk file extension: Android Application Package

[Static program analysis](https://en.wikipedia.org/wiki/Static_program_analysis)

[Android application id](https://developer.android.com/build/configure-app-module) tìm trong file `android/app/src/build.gradle`

## Bugs I encountered

Lỗi graphic automatic không đổi được [here](https://stackoverflow.com/questions/44328225/cant-change-emulated-performance-of-avd-in-android-studio)

Lỗi bấm icon "run" không chạy được [here](https://github.com/flutter/flutter/issues/131777). Cái nút có hình con bọ kế bên mới đúng. Nó là debug mode không phải run mode.

Lần đầu chạy bằng `flutter run` (muốn hot reload phải tự bấm nút `r`). Lần sau chạy bằng nút kia mới có hot reload.

Tải cocoapods dùng `sudo gem install cocoapods --verbose`. Nếu gặp lỗi ruby homebrew ssl [fixed](https://www.reddit.com/r/ruby/comments/187v7ku/struggling_with_rvm_and_installing_ruby_on_a_mac/)

## References

Có nhiều doc lắm:
[Dart the language doc](https://dart.dev/guides)
[Flutter SDK API doc](https://api.flutter.dev/): `material.dart`
[Flutter indo doc](https://docs.flutter.dev/get-started/install) more beginner-friendly; beginner concepts

Lên pub.dev coi những package có nhiều người dùng để biết tính năng nào hay.

[Dart language cheatseat](https://dart.dev/resources/dart-cheatsheet) very important

# 05: IDP (dart)

[MEGA](https://mega.nz/folder/G2J2hSgD#zWpYPPXXVqbUvH69qP7qcA)

phân biệt dynamic vs var

const và final giống nhau. Nhưng const cho số number, final cho string

menu + - * chia
dùng switch case

[extends-implements-mixin in Flutter’s Dart](https://medium.com/@lakshithlfvithana/understanding-the-differences-between-extends-implements-and-mixin-in-flutters-dart-f4bb152dd464)

## Thi qua môn

console app, menu, OOP, add, search (by name or id), delete, write to file, validation

## Edunext

geolocator, connectivity_plus

[video_player](https://pub.dev/packages/video_player) is the official plugin

## 01: dice-rolling-app

**Page overview**\
page1: grid view\
page2: 2 con dice (xúc sắc) dùng `Row()`\

**Functions:**\
chèn hình ảnh có onTap và không onTap (ảnh thường)\
chuyển page navigator push\
showDialog

`material` widgets is for android specific, cupertino widgetsa are for iOS (iPhone)

đôi khi copy hình vào `images` phải tắt IDE mở lại nó mới load

## 02: LoginForm_Validation_collections

**Đừng** copy bài này làm starting point, chỉ dùng để tham khảo.

**Page overview**\
Bài này có 2 file main để chạy: chạy file main trong login page, login được thì vào HomePage. Còn `main()` trong HomePage là để test trước khi login vào.\
validation: not empty/null

**Functions:**\
Form handling, form validation with regex\
Đổ data từ list ra widget `ListTile()`

## 03: 1st_DB_sqflite_ListView

**Page overview:**\

**Functions:**\
Dùng `ListView.builder` đổ data ra.\
FloatingActionButton show create form bottom right corner\
Viết DB & service 2 file riêng.\
addAlertDialog, delete row\
Mình thêm drop-down list, radio button

**TODO**: về viết validation, viết xóa hiển thị dialog, yes/no

## 04: Drawer_FilterRange

![lab041.png](./assets/lab041.png)
![lab042.png](./assets/lab042.png)

**Page overview:**\

**Functions:**\
Dùng `path_provider`, lab03 dùng `path`
Drawer trong appBar (drawer là cái hamburger button)\
Filter by RangeSlider

## 05: DI_bottomNavigationBar_TabController

NOTE: coi bài lab 102 tốt hơn

![lab05.png](./assets/lab05.png)

**Page overview:**\
HomeScreen có Navigation Bar at the bottom. Trong `Movie` có Tab Controller\
floatingActionButton & add dialog to add new movie\
có int Id, bool isFavorite

**Functions:**\
Dùng `PageController` và `PageView` để show cái page.

**Bug:**\
Hàm `loadMovies()` trong `MovieScreen` lỗi json trả về 1/0 chứ không trả về true false [cách fix](https://stackoverflow.com/questions/59343470/type-dynamic-dynamic-is-not-a-subtype-of-type-dynamic-bool-of-tes)

**TODO**:\
đổi floating button add movie lên cái app bar\
(DONE) sửa lại tính năng favorite

## 06: Login_parameterized widgets_gộpDB_Service

![lab061.png](./assets/lab061.png)
![d1768fdb566ac8286d8d714d5afeea6b.png](./assets/d1768fdb566ac8286d8d714d5afeea6b.png)

**Page overview:**\
Làm login, hiển thị user theo trạng thái isActive\
viết gộp database & service vào 1 file, không làm DI repository gì hết.\
combo box đổ gender

**Functions:**\

**TODO**: đăng nhập user nào thi user đó không hiển thị trong grid

## 07: FlutterFE_SpringRestAPI

**Page overview:**\

**Functions:**\

`networksetup -getinfo Wi-Fi`

Tải code của ông thầy về phải vào pubspec chỉnh Dart SDK version lại, chạy `flutter clean`

TODO: xóa về làm dialog

## 08: 2 tables quan hệ API

**Page overview:**\
repository query `LIKE` tìm mệnh đề tương đối

**Functions:**\

## 09: LightDarkTheme

**Page overview:**\
button navigation bar ở dưới bottom screen (chuyển page)\
tab bar (không chuyển page)\
drawer

search bar, add alert dialog

floating action button

**Functions:**\

## 102: 

**Page overview:**\

**Functions:**\

## 0?: 

**Page overview:**\

**Functions:**\

## My bugs

Android Studio load chỉ hiện file android => [stack overflow](https://stackoverflow.com/questions/71110388/android-studio-dont-show-project-folder-structure)

Tải code về phải chỉnh version Dart `^3.5.0` trong pubspec. Chạy `flutter clean`

[cách xóa emulator storage](https://stackoverflow.com/questions/54461288/installation-failed-with-message-error-android-os-parcelableexception-java-io)