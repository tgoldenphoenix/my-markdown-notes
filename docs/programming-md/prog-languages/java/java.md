# Java notes

## Basics & Terminologies

Structurally, the Java language starts with **packages**. A package is the Java language's namespace mechanism (1 folder). Within packages are classes (such as the file `main.java`), and within classes are methods, variables, constants, and more. Cấu trúc lớn hơn packages là [module](https://developer.ibm.com/tutorials/java-modularity-2/?cm_mmc=OSocial_Blog-_-Developer_IBM+Developer-_-WW_WW-_-ibmdev-OInfluencer-YouTube-KA-Java-Modularity&cm_mmca1=000037FD&cm_mmca2=10010797).

Java has a **garbage collector** and it doesn't require you to write any memory-handling code. This is known as **implicit memory management**.

When you download a JDK, you get — in addition to the compiler and other tools — a complete class library of prebuilt utilities that help you accomplish most common application-development tasks. Go to the Java API doc to learn more.  
The JDK comes packed full of useful classes like `java.lang.String`, and those in the `java.lang` package do not need to be imported (a shorthand courtesy of the Java compiler).

The Java language is a C-language derivative, so its syntax rules look much like C's.

You also may not use `true, false, and null` to name Java constructs. Technically, these are **literals**, which are the source code representations of a value, rather than keywords. [Phân biệt Reserved words vs literals](https://developer.ibm.com/learningpaths/java-get-started/java-language-basics/)

- reserved words (also called keywords): `abstract, boolean throw else super`
- literals: `true false null`

## Data type

Primitive type:

- `byte`: Stores whole numbers from -128 to 127
- `short`: Stores whole numbers from -32,768 to 32,767
- `int`: whole numbers from -2,147,483,648 to 2,147,483,647
- `long`: whole numbers from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807

- `float`: Stores fractional numbers. Sufficient for storing 6 to 7 decimal digits
- `double`: fractional numbers. Sufficient for storing 15 to 16 decimal digits. Floating-point literals (number có dùng dấu `.` like `1.0, 1.1, 1.2`) are inferred as double by **default**.

- `char`: Stores a single character/letter or ASCII values
- boolean: `true` or `false`

Literal Suffix:

- `D` (or `d`): This suffix is used to explicitly declare a floating-point literal as a double. While floating-point literals are double by default, using D can improve clarity, especially when mixing float and double values.
- `F` (or `f`): This suffix is used to explicitly declare a floating-point literal as a float. Without this suffix, a literal like 3.14 would be treated as a double and require a cast to be assigned to a float variable.

Reference type: String, Arrays, Classes

[What does the colon (:) operator do in Java?](https://stackoverflow.com/questions/2399590/what-does-the-colon-operator-do)

### Upcasting & Downcasting (ép kiểu)

- **Upcasting** can happen implicitly (trẻ nhỏ lớn lên là thuận tự nhiên). Upcasting allows us to treat an object of a subclass as if it were an object of one of its superclasses.
- **Downcasting** bắt buộc phải tường minh (explicit) (già mà thành nhỏ là phải có giấy tờ khai rõ ràng). Through downcasting, we regain access to the subclass’s methods and fields.

```java
// down casting
int myInt = 300;
byte myByte = (byte) myInt; // This will result in data loss, myByte will be 44
```

Down casting: double -> float -> long -> int -> short -> byte

- Trong 1 expression, chỉ cần có 1 real number thì Java sẽ trả về real number: `(double) 3 / 2 = 1.5`, `1.0 / 5 = 0.2`
- Còn nếu 2 số nguyên chia nhau thì chỉ lấy phần nguyên `3 / 2 = 1 not 1.5`

Có 2 loại ép kiểu: (1) ép kiểu primitive (trong procedural programming cũng có) và (2) ép kiểu kế thừa OOP.

Khi muốn biết kiểu của một object pointer, chỉ quan tâm kiểu được khai báo trước tên biến pointer. Còn bên phải dấu `=` nó up/down casting gì không quan tâm.

Trong cây quan hệ có 2 kiểu quan hệ:

1. Trực hệ (direct): cha-con, ông nội-cháu, ông cố-cháu. Chỉ được phép up/down casting khi có quan hệ trực hệ. Không được phép nhảy nhánh hay ép kiểu ngang trong cây quan hệ.
2. Chú-cháu, bác-cháu.

Khi biên dịch thì IDE chỉ quan tâm đến khai báo. Miễn là khai báo có quan hệ cha-con, cháu-ông nội thì nó coi như là cho phép biên dịch.  
Còn trong chạy thật, khi run-time. Thì nó mới xét đến bản chất thật sự của class mà biến tham chiếu đang trỏ đến và nó **chỉ cho ép downcasting thật sự khi có quan hệ** `IS-A`. Có thể kiểu khai báo là `HocVien` nhưng bản chất pointer trỏ đến một object `SinhVien`. Khai báo là một chuyện, bản chất là chuyện khác.

- `IS-A` (liên quan tới inheritance): con cháu IS-A cha, ông nội.
  - `Dog` IS-A `Animal` but `Animal` not IS-A `Dog`
  - `SinhVien` IS-A `HocVien`
- `HAS-A` (không liên quan tới inheritance): If Class A "has-a" Class B, it means Class A uses or contains an object of Class B as a part of its own structure or functionality.
  - A `Car` class "has-a" `Engine` class. This means the Car class has an Engine object as one of its fields, and it utilizes the Engine's functionalities.

```java
class Animal {}
class Dog extends Animal {}
// Upcasting
Animal animal = new Dog();
animal.eat(); // Valid because eat() is a method in Animal
// animal.bark(); // Invalid because bark() is not a method in Animal

// === Downcasting ===
Animal animal = new Dog(); // Upcasting
Dog dog = (Dog) animal; // Downcasting
dog.bark(); // Now valid because dog is a reference of type Dog

// Invalid Downcasting
Animal animal = new Animal(); // Just an Animal, not a Dog
Dog dog = (Dog) animal; // This will throw a ClassCastException at runtime
// Casting Between Siblings (Invalid)
Dog dog = new Dog();
Cat cat = (Cat) dog; // INVALID: A Dog is not a Cat

```

- Khi ép kiểu ngầm định thì Java phải đảm bảo là việc ép kiểu xảy ra thành công thì nó mới cho biên dịch cho nên khi ép kiểu ngầm định, nó chỉ ép từ class CON về class CHA cho chắc ăn
- Con khi ép kiểu tường minh thì Java nó chỉ xem thử là các class có quan hệ trực hệ hay ko, chứ nó ko chắc chắn là sẽ chạy ko lỗi. Lúc đó, nó sẽ đẩy trách nhiệm của việc kiểm soát lỗi trong quá trình chạy (lỗi run-time) cho lập trình viên tự lo, tự kiểm soát

The `instanceof` operator in Java is a binary operator used to determine if an object is an instance of a particular class or interface, or a subclass/implementation of that type.  
It returns a boolean value: `true` if the object is an instance of the specified type, and `false` otherwise. `instanceof` trả lời câu hỏi **có IS-A hay không?**

### Autoboxing and Unboxing

- **Auto-boxing** is the automatic conversion that the Java compiler makes between the primitive types and their corresponding object wrapper classes.
- Boxing là convert primitive thành Object. Unboxing là ngược lại từ wrapper class => primitive types.

Every primitive type in the Java language has a JDK counterpart class. [xem table 4](https://developer.ibm.com/learningpaths/java-get-started/java-language-basics/#listing1)

Strictly speaking, you don't need to box and unbox primitives explicitly. Instead, you can use the Java language's auto-boxing and auto-unboxing

### Null, pointer and Garbage entities

```java
// 2 dòng này is the same
SinhVien sv1;
SinhVien sv2 = null;
```

Declare without initialize thì **mặc định** java sẽ cho `= null;` tức là chưa trỏ đến đối tượng nào cả.

Biến `sv1` không phải object mà nó là reference variable.

Con trỏ trong C khi qua Java được gọi là biến tham chiếu, tiếng Anh là **reference variable**, biến này ko chứa giá trị thật như các biến kiểu nguyên thủy (int, long, double...), mà nó chứa giá trị là địa chỉ của đối tượng mà nó đang trỏ tới ví dụ `package.SinhVien@7d8a992f`.

`SinhVien sv1;` => Khai báo 1 biến tham chiếu (con trỏ) có tên là sv1. Mới declare thôi chứ chưa initialize nhưng `sv1` hứa sẽ trỏ đến 1 đối tượng của lớp SinhVien. Nhưng nó có quyền trỏ đến 1 đối tượng của lớp con, cháu của lớp SinhVien. Vd: sau khi viết dòng code này, ta viết tiếp dòng `sv1 = new SinhVienXuatSac();` sẽ không bị lỗi. Vì `SinhVienXuatSac` là con của lớp SinhVien.  
Nhưng nếu ta viết là `sv1 = new ConMeo();` thì sẽ báo lỗi. Và nếu viết là `sv1 = new Human();` cũng không được, vì class `Human` là cha của SinhVien, chứ không phải con, cháu của SinhVien.

`SinhVienGioi` IS-A `SinhVien`

`sv2 = new SinhVien(); // line 3` Tạo ra 1 đối tượng của lớp `SinhVien`. Cấp phát vùng nhớ cho đối tượng đó. Sau đó, cho `sv2` trỏ (tham chiếu) đến đối tượng đó.  
Như vậy, sau line 3 này: `sv1` đang trỏ đến null (nghĩa là chưa trỏ đến đối tượng nào cả). `sv2` đang trỏ đến 1 đối tượng của lớp SinhVien (đối tượng đó được sinh ra ở line 3). Đối tượng này sinh ra ở dòng 3.

`sv1 = sv2; // dòng 4` => Sau line 4, sv1 và sv2 cùng trỏ (tham chiếu) đến chung 1 đối tượng (đối tượng đó được sinh ra tại line 3). Nghĩa là có 1 đối tượng và 2 con trỏ (tham chiếu) đang trỏ đến nó.  
**Lưu ý**: 1 đối tượng có thể được trỏ bởi bao nhiêu con trỏ cũng được hết.

`sv2 = null; // dòng 5` => Sau line 5 thì sv2 không còn trỏ đến ai nữa. Nó quay về lại trạng thái không trỏ đến ai, tức là bằng `null`. Ta nói sv2 bị què (con trỏ là `null`).  
Như vậy, sau line 5 thì: `sv1` vẫn đang trỏ đến đối tượng được sinh ra tại line 3. Sv2 thì không còn trỏ đến đối tượng đó nữa. Đối tượng sinh ra ở line 3 chỉ còn 1 con trỏ trỏ đến nó.

`sv1 = null; // dòng 6` => Sau line 6, `sv1 và sv2` không còn trỏ đến đối tượng nào cả (2 con trỏ đều bị què, tức là bị null). Đối tượng được tạo ra ở line 3 thì bị rơi vào hoàn cảnh: Không có ai trỏ đến nó. Lúc đó, nó được gọi là **RÁC (garbage)**. Hệ điều hành phải có trách nhiệm dọn RÁC này. Khi nào hệ điều hành dọn? Lâu lâu nó sẽ đi dọn rác 1 lần, tức là dọn nhiều rác 1 lúc.

Trong C không có tự dọn rác, developer phải tự dọn.

Coi lại trong SQL cũng có `null` và rỗng khác nhau.

### NullPointerException

A **NullPointerException** (NPE) xảy ra khi cố tình truy cập tới phương thức hoặc thuộc tính của một đối tượng thông qua một biến con trỏ (reference variable) mà biến con trỏ đó hiện tại là `null`.  
NullPointerException chỉ có thể xảy ra ở những dòng có dấu `.` để gọi biến hoặc hàm.

- In Java nếu declare primitive variables mà không initialize value thì sẽ có default value chứ không phải `null` (Java không có `undefined` như Javascript).
  - Với number (int, double) default là 0
  - char: `\u0000` (the null character)
  - Boolean default value sẽ là `false`.

Còn declare object like `SinhVien sv;` mà không initialize sẽ default `sv=null`.  
Vì String không phải là một kiểu primitive trong Java nên `String a;` thì `a=null` by default.

The `java.lang.IndexOutOfBoundsException` is a runtime exception in Java that indicates an attempt to access an element at an invalid index within a data structure like an array, a list (e.g., ArrayList), or a string. This exception is thrown when the provided index is either negative or exceeds the valid range of indices for that particular structure.

`if ("Hello".equals(a)){}` nếu cần so sánh string phải làm như vầy để không có lỗi NullPointerException. Viết `a.equals("Hello")` là không đúng.

## String, StringBuilder, StringBuffer

Trong java, string không phải primitive. Nó là object được tạo từ class `String`.

In C, string handling is labor-intensive because strings are null-terminated arrays of 8-bit characters that you must manipulate.

Because Strings are first-class objects, you can use `new` to instantiate them. Setting a variable of type String to a **string literal** has the same result, because the Java language creates **a String object** to hold the literal, and then assigns that object to the instance variable.

[Why String is Immutable in Java?](https://www.scaler.com/topics/why-string-is-immutable-in-java/). But the reference to the object can be changed.

Chaining of method calls is a technique commonly used with immutable objects like String, where a modification to an immutable object always returns the modification (but doesn't change the original). You then operate on the returned, changed value.

- **String trong Java có nhiều nhược điểm** quá nên sinh ra StringBuffer:
  - Lớp String là bất biến (immutable). Lớp StringBuffer là có thể sửa đổi (mutable).
    - Any operation that appears to modify a String actually creates a new String object.
  - Khi bạn thực hiện nối nhiều chuỗi thì lớp String xử lý chậm và tốn nhiều bộ nhớ hơn, bởi vì mỗi lần nối thêm chuỗi nó tạo ra instance mới.
    - Khi bạn thực hiện nối nhiều chuỗi thì lớp `StringBuffer` xử lý nhanh và tốn ít bộ nhớ hơn. Không tạo ra đối tượng rác.
  - Lớp String ghi đề phương thức equals() của lớp Object. Vì thế bạn có thể so sánh nội dung của 2 chuỗi bằng phương thức equals().
    - Lớp StringBuffer không ghi đề phương thức equals() của lớp Object. Muốn so sánh 2 cái StringBuffer thì phải chuyển về `String`.

```java
String str = "Hello";
str = str + " World"; // This creates a new string object

StringBuilder sb = new StringBuilder("Hello");
sb.append(" World"); // Efficiently modifies the existing StringBuilder object
```

- StringBuffer vs StringBuilder:
  - `StringBuffer` là đồng bộ (synchronized) tức là luồng an toàn. Điều này có nghĩa là không thể có 2 luồng cùng truy cập phương thức của lớp StringBuffer đồng thời. Làm ứng dụng chat, messenger có xử lý multi-thread.
  - `StringBuilder`  là không đồng bộ (non-synchronized) tức là luồng không an toàn (Not thread-safe). Điều này có nghĩa là có 2 luồng cùng truy cập phương thức của lớp StringBuilder đồng thời.
  - StringBuffer không hiệu quả bằng StringBuilder. StringBuilder hiệu quả hơn StringBuffer.

## Collections

Collection là một loại đối tượng có thể chứa các địa chỉ trỏ đến các đối tượng khác. Collection không chứa the objects themselves. Mỗi phần tử trong collection là một address, not object.

Array trong Java có số phần tử cố định và phải khai rõ ràng lúc declare. Array is **NOT** collection, nó chỉ là một kiểu dữ liệu nguyên thủy trong Java.

`ArrayList` có cấu trúc giống Array (mảng) nhưng số phần tử là dynamic có thể resize được.  
Mảng thì dùng sytax `[]` để access item. Còn `ArrayList` làm cái gì cũng thông qua hàm hết như `.add(), .get(), .set()`, không làm trực tiếp như mảng.

- Interface `List`, `Map`, `Set`
- Class: `ArrayList`, `HashMap`, `HashSet`

`Map` (ánh xạ) là một loại collection dạng key-value  
Ánh xạ ví dụ `y = 2x`, với mỗi x chỉ cho ra một giá trị y.

`Set` không chứa duplicate values.

## Package

A package is a grouping of related types providing access protection and name space management. Note that types refers to classes, interfaces, enumerations, and annotation types. Two types of package:

1. Built-in Packages (packages from the Java API)
2. User-defined Packages (create your own packages)

Use keyword `package` to create your own package. Package là 1 folder có nhiều file `.java` bên trong.

**Module** là một cấp tổ chức cao hơn package. Trong module chứa nhiều related packages and other files.

Có 2 loại file trong java:

1. `.java`: file source code
2. `.class`: file byte code

Khi build code thì từ cac file `.java` trong thư mực `src` sẽ tạo ra thư mục `bin` chứa các file `.class`. Cấu trúc thư mục package giống nhau.  
Một `.java` source file can **ONLY contain one public class** whose name is same as the Java file name.

When you `import java.util.logging.Logger;`:

- module: `java.util`
- package (folder): `logging`
- Class: `Logger`

The keyword `import` khai báo chứ không cung cấp package. Vì package đã có trên máy, `import` không lên internet tải về. Tải về là nhiệm vụ của file `pom.xml` or `package.json`.

## Access Modifiers (bổ từ quy định mức độ truy cập)

We divide modifiers into two groups:

1. **Access Modifiers** (bổ từ quy định mức độ truy cập) - controls the access level of classes, interfaces and its members.
2. **Non-Access Modifiers**: do not control access level, but provides other functionality. We have `final, abstract, static`.

- There are two levels of [access control](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html):
  1. At the top level (classes, interfaces): `public`, or `package-private` (no explicit modifier, default).
  2. Class members (properties, methods): `public`, `protected`, `package-private` (default no explicit modifier), `private`.

- Có 4 mức độ truy cập:
  1. `public`: visible to all classes everywhere; cả thế giới có thể nhìn thấy tôi.
  2. `protected`: the member can only be accessed within its own package (as with package-private) and, in addition, cho phép class con khác package kế thừa copy. Class khác package & không phải class con thì không access được.
  3. `default` (no modifier or package-private): visible within its own package.
  4. `private`: can only be accessed in **its own class**.
- Có 4 mức độ nhưng chỉ có 3 bổ từ vì `default` thì không phải ghi gì cả. Classes thì chỉ có 1 bổ từ thôi.

Một class member "visible" nghĩa là ta có thể tạo object rồi access nó. Nếu nó khai `private` thì kể cả khi ta tạo được đối tượng cũng vẫn không access được.

Class A "nhìn thấy" class B nghĩa là:

- Trong code của class A có thể `extends` B.
- A có thể `new` object của B (of course constructor của Class B phải cho phép A nhìn thấy). Nhưng nếu giả sử A không nhìn thấy constructor của B (khai `private` constructor) thì A không thể `extends` B được vì A không thể gọi super constructor được. Hàm dựng phải khai `public` hoặc `default`.
- Nếu class A không thể thấy constructor của B (nó khai private constructor) thì class A cũng không thể `extends` B được.
- Nếu class B public mà khai private constructor thì không ai có thể extends & new object của B được hết.

Sau khi A nhìn thấy được class B thì ta mới xét tiếp là class A nhìn thấy được những **class member** (properties & methods) nào của B.

`protected` = default (nếu xét về mức độ nhìn thấy) + cho phép class con khác package kế thừa (copy) những properties & methods mà class cha khai là `protected`. Class con không thể nhìn thấy protected members của class cha mà chỉ được quyền copy & own bằng keyword `this & super`.

Nếu nói protected = default + class con **nhìn thấy** tt & pt của class cha => dễ gây nhầm lẫn.  
Nên nói là protected == default + class con **kế thừa** được tt và pt của class cha.

### this and super keywords

A subclass inherit from a **super-class** using keyword `extends`. A class can only extend from one parent. One class can have many subclass.

Cha em có cái nhà, em kế thừa `extends` cha em thì em sẽ lấy blue print của cha để xây cái nhà y chang. Rồi 2 cha con mỗi người ở 1 cái nhà của riêng nhưng 2 cái nhà giống y chang vì tạo ra từ 1 blue-print.

Vì class là một blueprint (bảng mô tả) nên kế thừa bản chất là thằng con **copy** (một phần hoặc tất cả) cái blueprint của thằng cha.

Ví dụ class `A entends B` & Class parent `B` có property `a` vậy thì:

- Class con A cũng có chứa thuộc tính `a`
- Class con A có khai báo thuộc tính `a` nhưng lập trình viên không cần khai báo mà Java tự động copy những dòng khai báo thuộc tính & phương thức của class cha `B` và bỏ vào trong class `A` cho mình. Tóm lại class `A` có mô tả (khai báo) thuộc tính `a`.

- `a, this.a, super.a` là 3 tên gọi chỉ **một biến duy nhất** nằm trên cơ thể class con mà nó copy được từ cha nó.
- `super.a` chỉ có giá trị khác với 2 cái kia (`this.a và a`) khi mình override biến `a` của class cha `B` trong class con `A`. Biến `super.a` lúc này sẽ refer với cái khai báo được Java auto copy từ class cha `B` qua class con `A`.
- Có thể hiểu class `A extends B` sẽ có 2 dòng khai báo biến `a`, một dòng copy từ `B` và một dòng tự class `A` khai báo override. Vì có 2 biến `a` cùng tên nên phải dùng `super.a, this.a` để phân biệt. Nhưng cả 2 biến **đều là của lớp con**, không có biến nào nằm trong class cha hết. Không có chuyện lớp con truy cập vào thuộc tính hay phương thức của lớp cha mà chỉ là truy cập chính những members là con đã copy của cha.
- `this.a` được dùng để phân biệt với `a` là parameter truyền vào trong constructors. Còn bình thường nếu chỉ dùng `a` mà không ghi gì thêm hết thì nó sẽ tự hiểu là `this.a`.
- Nếu class con `A` không override thì `this.a` và `super.a` là một.

Từ khóa `super` chỉ có thể được dùng bởi class con để chỉ tới cha nó. Class bên ngoài (như `main()`) không thể tạo đối tượng của thằng con rồi dùng `super` để chỉ tới thằng cha của nó. Vì `super` bị "che khuất" (override) nên class bên ngoài khi nhìn vô thằng con sẽ chỉ thấy `this.anCOm()` không thể thấy `super.anCom()`.

`super.a, this.a và a` nghĩa là chỉ biến `a` của lớp chính class `A` chứ không phải class B  
Ngay cả khi chúng ta sử dụng `super.a` thì ta cũng đang truy cập biến `a` của class A chứ không phải truy cập biến `a` của class B.

### Overload vs Override

Overload, OL (Nạp chồng):

- Giống: tên phương thức. Khác: danh sách tham số truyền vào(parameter types not parameter names)
- Khác: nội dung phương thức. Giống: Trong cùng 1 class.
- Ban chất là **tăng cường khả năng**: `cong(int a, int b)`, `cong(int a, int b, int c)`, `cong(int a, int b, int c, int d)`.
- Chỉ cần khác **method signature** là overload: the method's name and the parameter types (not parameter names or number of parameters). Return type cũng không tính.
- Class con có thể copy method từ class cha rồi overload nó thay vì override.

Override, OR (Ghi đè, che khuất):

- Giống: tên phương thức. Giống: danh sách tham số truyền vào
- Khác: nội dung phương thức. Khác: ở 2 class khác nhau nhưng có quan hệ cha - con, ông - cháu.
- Method override cũng giống như override biến, lúc này trên cơ thể class con sẽ có cả 2 methods cùng tên được nhưng được phân biệt bởi `this & super` keywords.
- Override biên: biến của class con che khuất biến của cha mà nó copy được. Sự che khuất diễn ra trong nội bộ class con chứ thằng con không có ảnh hưởng gì tới pt & tt của thằng cha hết.
- Xet về access modifiers có 2 trường hợp:
  1. cha con phải bằng nhau;
  2. con phải có khả năng được nhìn thấy (vivsibility) **cao hơn cha** ví dụ `con public > cha default`. Vay nếu cha là `default` thì con có thể là: `default`, `protected`, `public`.
- Nếu class cha khai là `private` thì class con sẽ không thể thấy để mà copy. Nên không kế thừa được hay override được `private` members của class cha.
- Nếu xét return type:
  1. nếu trả về primitive types thì phải giống y chang nhau mới được không casting gì hết;
  2. nếu trả về objects of classes like `Animal, Dog` thì class con có thể trả về tất cả những kiểu mà là IS-A kiểu trả về của lớp cha. Ví dụ class cha return `Animal` thì class con khi override method có thể trả về `Dog`; `Dog` IS-A `Animal`.
- Class con có copy, kế thừa nhưng không được override `static` methods của lớp cha. Nhưng có thể overload `static` methods copy được từ class cha. Class con gọi static methods copy được từ class cha không cần dùng `super` cứ gọi bình thường.

- Hiện tượng xuất hiện 2 phương thức `void a(String x) và void a(String y)` ---> Có gọi là cùng danh sách tham số truyền vào không?
- `a(int a, int b)` và `a(double d, int c)` là overload. Trong Java khai parameter `int` phải truyền đúng `int` không được truyền `double` mặc dù double double có thể chứa `int`.

### Phân biệt IS-A và HAS-A

- IS-A: là một ...; quan hệ kế thừa
- HAS-A: có một (sử dụng)...; quan hệ sở hữu
- Cả 2 đều là quan hệ giữa **Class và Class, không phải la quan hệ giữa lớp và đối tượng**.

```java
class ConNguoi extends DongVat{
  String hoTen;
  ConMat matTrai;
  ConMat matPhai;
  Chan chanTrai;
}
```

ConNguoi IS-A DongVat. Lớp con IS-A lớp cha.

- ConNguoi HAS-A `ConMat` không phải matTrai hay `matPhai` mặc dù trong class `ConNguoi` có sử dụng 1 hoặc nhiều đối tượng của `ConMat`
- ConNguoi HAS-A Chan. Không được nói `ConNguoi` HAS-A `chanTrai`.
- ConNguoi HAS-A String chứ không phải HAS-A hoTen. `hoTen` chỉ là cái tên biến do mình tự đặt ra thôi.

Implement interface **cũng** tạo ra quan hệ IS-A: `class Laptop implements Computer` thì `Laptop` IS-A `Computer`.

## Variable scope & Non-access Modifiers

[colon operator : inside java method parameter](https://stackoverflow.com/questions/15600917/java-method-with-method-with-colon-in-parameter)

- **Local variable**:
  - Các biến được khai báo trong các functions (method scope)
  - Hoặc các tham số đầu vào của các hàm
  - Vì sao gọi là biến địa phương? Vì nó chỉ tồn tại trong hàm đó thôi. Khi method hết nhiệm vụ (ra khỏi hàm) thì các biến đó bị biến mất (giải phóng)
  - Variables inside blocks `{}` is also local variables. Ra khỏi `{}` thì những biến này không còn tồn tại nữa. Ví dụ inside `if-else {}`, biến counter `i` trong `for loop`.

Thường nếu dùng `try catch {}` thì phải declare variable bên ngoài.

**Instance variable** (biến của thể hiện, biến của đối tượng, class scope). Nếu gọi là biến toàn cục (_global variable_) là cách gọi **sai**, vì biến toàn cục là cách nói trong ngôn ngữ lập trình hướng thủ tục, còn trong OOP thì gọi là biến của thể hiện.

Vì sao nó được gọi là biến của đối tượng?  
Vì khi ta tạo 1 đối tượng thì biến loại này nó cũng được tạo theo và nó trở thành một vật sở hữu của đối tượng được tạo. Khi đối tượng mất đi, biến này sẽ phải được giải phóng

In java, con trỏ = biến tham chiếu (reference variable). Ví dụ con trỏ chỉ tới một object.

**Static variable** (biến của lớp, tất cả đối tượng của lớp đó được dùng chung). Cách khai báo giống instance variable, nằm ngoài methods nhưng phải thêm `static` keyword. Nên dùng cú pháp `CLassName.static_var` để truy cập và **không cần tạo object**.  
Static variables & methods giúp ta tiết kiệm bộ nhớ ram vì **không phải tạo object** để gọi và có thể dùng chung.  
`static` Attributes and methods belongs to the class, rather than an object

- **Static context** có nghĩa là Static chỉ chơi với 2 cái:
  1. Static chỉ chơi với static
  2. Static có thể chơi với cái chắc chắn có tồn tại: nếu anh ko phải static thì tôi vẫn chơi với anh nếu như anh chắc chắn tồn tại (bằng cách tạo object rồi gọi biến & hàm).

Tức là trong static method (như `main()` chẳng hạn), muốn gọi variables & method trong 1 class thì: (1) phải là static variable/method. Còn nếu không thì phải (2) tạo object và gọi thông qua object vừa tạo.  
Lý do có static context là gì liên quan tới memory management inside RAM. Khi chạy project thì java load all static methods & varibles bỏ vào một vùng đặc biệt trên RAM. Việc này được làm trước khi chạy hàm `main()`. Static context giúp ta tránh gọi `null` dẫn tới lỗi.

**WARNING**: Tuyệt đối KHÔNG tạo static method to connect to database (variable thì được). Cause errors that cannot be fixed. Trong `main()` cứ tạo đối tượng rồi gọi kết nối DB.

A `static` method/attributes can be accessed without creating an object of the class first. There's only one copy of the static member, shared across all objects of that class. It's good for memory management. Nên dùng vừa phải, hợp lý.

The `main()` method is the starting point of your program. Vì sao hàm `main()` là hàm `static`?

- It's `static` because otherwise the compiler would need to make an object out of the class it's in first in order to call it.
- It's public because otherwise it couldn't be called from the outside.
- It's void because it can't return a value by design.

[static factory method](https://www.youtube.com/watch?v=sOpbAOX5nJs) được dùng khi constructor không dùng được.

### final & abstract

- The keyword `final` có thể đi với:
  1. variable: constant variable (không thay đổi giá trị được)
  2. method: method cannot be Overridden (cha mẹ nói sao con làm vậy, không cãi lời)
  3. class: Class đó không có class con (khong được `extends` kế thừa), class đó là class cuối cùng trong cây gia phả.

- Keyword `abstract`:
  * Không đi được với biến.
  * Abstract method: phương thức trừu tượng, chỉ có khai báo tên hàm, kiểu trả về, argument list; không có body
  * Abstract class: class có chứa 0 hoặc nhiều abstract methods

- `final` cannot go together with `abstract` vì về bản chất:
  - `final`: hứa ko thay đổi, ko có con nữa, ko ai thay thế nữa
  - `abstract`: mơ hồ, chưa rõ ràng, hứa là sẽ có con cái triển khai ý tưởng mơ hồ của mình

- Giống nhau: Đều là lời hứa.
- Khác nhau:
  - `final` thì hứa sẽ không có, không còn
  - `abstract` thì hứa sẽ có, sẽ còn
- 2 lời hứa này mâu thuẫn với nhau, nên 2 bổ từ này không được phép đi cùng nhau

## Interface & Abstract class

**Interface** is a group of related methods with empty bodies (abstract method). An interface defines the methods that a class MUST implement. A class implement from an interface using the keyword `implements`. Bắt buộc phải implement tất cả empty methods của interface nếu không  sẽ báo compilation error. Vì methods declared within an interface are implicitly `public & abstract` by default.

Abstract classes and Interfaces can NOT be instantiated directly.

If you define a reference variable (biến tham chiếu) whose type is an interface, any object you assign to it must be an instance of a class that implements the interface.

A class can `implements` more than one interface. But one class (including abstract classes) can only inherit `extend` from one super-class.  

In Java, an interface can extend **one or more** other interfaces. This mechanism is known as **interface inheritance** (khác với class inheritance).

- The child interface inherits all the abstract methods, default methods, static methods, and constant fields from its parent interfaces.
- Any class that implements the child interface must provide implementations for all the abstract methods inherited from both the child interface and all its parent interfaces.

- **Interface methods** are by default `abstract and public`. This means you do not need to explicitly add the public keyword when declaring them within an interface.
- Interface attributes (variables) are by default `public, static and final`. This means they are accessible from anywhere without needing an object of the interface, and their values cannot be changed after initialization

- **Lớp trừu tượng** là lớp không thể tạo đối tượng của lớp đó, nghĩa là không thể dùng từ khóa new để tạo đối tượng cho lớp đó. ---> vì lớp đó không có nhu cầu tạo đối tượng. Chỉ tạo ra với mục đích giải quyết những bài toán quan hệ giữa các đối tượng phức tạp.
- Cấu trúc của một abstract class: Gồm có **zero** hoặc một, hoặc nhiều, hoặc tất cả đều là phương thức trừu tượng. Có quyền không có abstract method cũng được.
- Lớp con khi kế thừa bắt buộc phải Override abstract method (nếu có).
- Bên trong lớp trừu tượng **có thể** chứa phương thức cụ thể. Nhưng Interface thì không được.
- Abstract classes có thể chứa các trường dữ liệu.

- Ý nghĩa của lớp trừu tượng:
  - Lớp trừu tượng là **lớp bất lực** và chỉ là **lớp hứa** mà thôi.
  - Hứa cái gì? ---> Hứa hành vi (phương thức)
  - Ai sẽ thực thực hiện lời hứa ---> Các lớp con sẽ thực hiện
  - Vì sao phải hứa? ---> Vì nó muốn các lớp con phải có chung hành vi nhưng mỗi lớp con được phép hiện thực hóa các hành vi theo các cách khác nhau

`HinhTamGiac` IS-A `Hinh`. Đã là hình thì bắt buộc phải có `tinhChuVi()` và `tinhDienTich()`. Mà `tinhChuVi()` và `tinhDienTich()` chỉ là những lời hứa. Vì class Hình không biết tính chu vi và diện tích cụ thể như thế nào?  
Vậy: bên trong class Hình, 2 phương thức này sẽ là 2 phương thức trừu tượng còn trong class `HinhTamGiac` 2 phương thức này sẽ là 2 phương thức cụ thể.

Phân biệt cách sử dụng Interface và abstract class. Khi gặp quan hệ IS-A thì ta xét như sau:

- Nếu là quan hệ **luôn luôn** thì chắc chắn xuất hiện Class cụ thể hoặc Abstract Class.
- Nếu là quan hệ **thỉnh thoảng** lúc có lúc không thì xuất hiện Interface.
- Khi có nhiều hơn 1 quan hệ luôn luôn thì chắc chắn có quan hệ IS-A đa cấp tức là sẽ xuất hiện quan hệ cha-con nhiều cấp
- Nếu class mà không cần phải tạo ra đối tượng thì ta cho nó là Abstract
- Tính từ thường là thể hiện của 1 interface

- khi hệ thống có một chức năng gì mà muốn thể hiện theo nhiều cách khác nhau thông qua các class khác nhau thì dùng interface. Ví dụ: 2 class là `BubbleSort` và `InsertionSort` implement interface `SortAlgorithm`; Interface `List` và các class kế thừa nó ArrayList, Vector, LinkedList.
- khi mà chương trình của chúng ta có các đối tượng các tính chất giống nhau trong cách thể hiện thì ta nghĩ đến abstract class. Ví dụ: 2 Classes `Student`, `Teacher` extends kế thừa abstract class `Person`.

Bài tập thứ nhất:

- `đàn ông` luôn luôn là `con người` rồi `con người` luôn luôn là `động vật` 24/24 => Class cụ thể hoặc abstract.
- `động vật`, `con ng`, `đàn ông` là 3 class. Có nhiều hơn 01 quan lệ **luôn luôn** nên có QH đa cấp. `động vật` > `con ng` > `đàn ông`; quan hệ ông nội - cha - con.
- `đàn ông` phải là class cụ thể vì nghiệp vụ bài toán chỉ quan tâm tới `dan ông` (ví dụ củ thể là `thanh niên nghiêm túc`). `con ng` và `động vật` trong đề bài không có nhu cầu tạo object nên cho làm abstract classes. Mục đích của abstract class là để giải quyết bài toán một cách dễ hiểu hơn thôi.
- `người chồng`, `ng bạn`, `nhân viên cty` là interfaces vì nó là quan hệ **thỉnh thoảng**. Minh không có làm chồng 24/24 mỗi ngày. Về là thì đâu còn là `nhân viên` nữa, làm bạn cũng vậy. Vậy thì class `đàn ông` **đôi khi** sẽ `implements` `người chồng`, `ng bạn` hoặc `nhân viên`.

Bài tập thứ 02:

- class: `phụ nữ`, `thiên thần`
- `độc ác`, `thánh thiện` và `nóng nảy` là interfaces nhưng mình sẽ chuyển sang danh từ để đặt tên là: `kẻ độc ác`, `người thánh thiện`, `tên nóng nảy`.

Python không có interface, một class có thể kế thừa nhiều class. Vậy interface mục đích là để làm gì?

- Định nghĩa interface: tương tự như 1 class trừu tượng nhưng **chỉ được phép chứa phương thức trừu tượng**, không được phép chứa phương thức cụ thể.
- Ý nghĩa của interface:
  1. Sinh ra chỉ để hứa (không làm được một cái gì cụ thể hết) ---> Giống như 1 bản hợp đồng
  2. Giải quyết **bài toán đa kế thừa** một CON có nhiều CHA
- Vì sao: vì trong Java chỉ cho phép mình extends 1 class duy nhất. Cho nên phải sử dụng interface đi kèm với từ khóa implement để giải quyết bài toàn nhiều CHA. Một class java có thể implements multiple interfaces.

Vd: ConNguoi IS-A cái gì? => DongVat, SinhVat, NoLe, BaoChua, ThucAnCuaLoaiKhac, ThienDichCuaLoaiKhac

- Kế thừa đa cấp: ông nội > cha > con > cháu. Chia quan hệ IS-A ra nhiều cấp => most OOP languages
- Đa kế thừa: 1 class con muốn kế thừa nhiều class cha => Java, C# không hỗ trợ nên nó mới đẻ ra khái niệm **interface**.

Vì Java, C# chỉ cho phép kế thừa 1 class và implements nhiều interface. Tức là có 1 class cha chính (QH luôn luôn), các "cha phụ" (QH thỉnh thoảng) thì chúng ta phải thiết kế thành interface. Nhưng nếu trong bài toán không có QH luôn luôn mà chỉ có duy nhất 1 QH thỉnh thoảng thì làm luôn class khỏi interface cho khỏe. Chỉ dùng công thức khi có xung đột nhiều ông cha phải chọn cha chính, cha phụ. Còn nếu chỉ có 1 QH IS-A duy nhất thì khỏi interface cứ xài class.

Một class có thể kế thừa và implements cùng một lúc như sau: `public class B extends A implements I1, I2 {}`

### Using an Interface as a Type

When you define a new interface, you are defining a new reference data type (giống object).

If you define a reference variable whose type is an interface, any object you assign to it must be an instance of a class that implements the interface.  
Reference variable can only point to object not interface.

## The four main principles of OOP

The core principle is abstraction. Without it, the others couldn't exist.

1. **Abstraction** (tính trừu tượng): Ignoring or hiding details that don’t matter, allowing us to get an overview perspective of the thing we’re implementing, instead of messing with details that don’t really matter to our implementation.
2. **Encapsulation** (tính đóng gói): Hide internal state (`private`, cannot accessible from outside the class) and requiring all interaction to be performed through an object's methods (access modifiers, `Getter and Setter`)
3. **Inheritance** means specialized classes — without additional code — can **copy** the attributes and behavior of the source classes that they specialize.
4. **Polymorphism** (tính đa hình): Subclasses of a class can define their own unique behaviors and yet share some of the same functionality of the parent class. Class con có thể override class cha.

### Polymorphism with Interface

A single interface reference variable can refer to objects of various classes that implement that interface, and calling a method on that reference will execute the specific implementation of that method provided by the actual object's class at runtime.

## Operator

There are 3 types of operators:

1. Unary: Only one operand is needed.
2. Binary: Two operands are needed.
3. Ternary:

## Flow control & Loops

At times, you need to bail out of — or terminate — a loop before the conditional expression evaluates to false:

- The `break;` statement takes you to the next executable statement outside of the loop in which it's located.
- You can also skip a single iteration of a loop but continue executing the loop. For that purpose, you need the `continue` statement

`for(dataType variable : array) {}` là **for-each loop** (or enhanced loop) in Java. They are mostly used to iterate through an array or collection of variables.

## Classes and Objects

- Objects (đối tượng) là thể hiện của 1 lớp.
- Class is a blueprint (bảng mô tả) for creating objects, mô tả các thuộc tính và hành vi của một tập hợp các đối tượng (có thuộc tính và hành vi giống nhau).

- Lớp không chứa thuộc tính và phương thức mà lớp có **mô tả** các thuộc tính và phương thức.
- Đối tượng mới thực sự có **sở hữu** các thuộc tính và phương thức.

- Khi coding thì code trong Class.
- Khi chương trình chạy thì Java cần tạo ra các đối tượng để chạy CT.

In the Java program there would be one class chứa the `main()` method. It will create objects from other class and run the program logic.

Khi chương trình chạy thì phải load chuong trình vào RAM. Khi nghe bài nhạc mp3 trên máy tính thì phải load bài nhạc đó vào RAM.  
Chương trình không chạy trên hard drive mà chạy trên RAM. RAM chứa chương trình đang chạy và những dữ liệu CT cần.

Object trong Java được chứa trong RAM.

ROM (read only memoby) nằm trên main board

- Hệ điều hành 32 bits chỉ quản lý tối đa 4GB RAM thôi. Có thêm RAM nữa cũng vô dụng.
- Máy tính trên 4GB RAM thì bắt buộc phải cài OS 64 bits

is ARM 64?

A class's methods define its behavior. Methods fall into two main categories: **constructors** and all other methods, which come in many types.  
A constructor method is used only to create an instance of a class. Other types of methods can be used for virtually any application behavior.

**Accessor methods** là getter và setter. To encapsulate a class's data from other objects, you declare its state (variables) to be `private` and then provide accessor methods.  
The naming of accessors follows a strict convention known as the **JavaBeans pattern**. In this pattern, any attribute Foo has a getter called `getFoo()` and a setter called `setFoo()`. The JavaBeans pattern is so common that support for it is built into the IDE.

Object-oriented languages follow a different programming pattern from **structured programming languages** like C and COBOL. The structured-programming paradigm is highly _data oriented_: You have data structures, and program instructions act on that data. Object-oriented languages such as the Java language combine data and program instructions (behaviours) into objects.

### Constructors

The name of the constructor must match the name of the class and constructor do **NOT** have return type. Other methods have an optional return type or must return `void`.

Types of Constructors in Java:

1. **Default Constructor**: has no parameter; automatically called when an object is created.
2. **Parameterized Constructor**.
3. Private Constructor
4. Copy Connstructor: creates a new object by copying the state of an existing object, allowing a distinct copy with the same values without referencing the original object.

**Overloading** allows us to create multiple constructors in the same class with different parameter lists.

The compiler automatically provides a no-argument, **default constructor** for any class without constructors. This default constructor will call the no-argument constructor of the superclass. In this situation, the compiler will complain if the superclass does not have a no-argument constructor so you must verify that it does. If your class has no explicit superclass, then it has an implicit superclass of `Object`, which does have a no-argument constructor.

## Exception and Error Handling

Exception khác với Errors:

- **Error**: nếu ct gặp lỗi thì dừng ct luôn; severe problems often related to the Java Virtual Machine (JVM) or system resources, that a reasonable application should not attempt to catch or recover from.
  - Typically, errors indicate issues beyond the program's control and necessitate changes to the system environment or application architecture rather than in-code handling.
  - Example: `OutOfMemoryError`, `StackOverflowError (tràn ngăn xếp)`, `VirtualMachineError`, `FileNotFoundException`

- **Exception**: cũng có dừng có khi không. Khi chúng ta có biện pháp bảo vệ (bảo hộ) thì ct ko dừng. Example: `NullPointerException`, `IOException`, `ArrayIndexOutOfBoundsException`, divide by 0, lỗi kết nối mạng.
- Có 2 loại exception:
  - **Checked Exceptions** (compile time Exceptions): bắt buộc phải `try-catch` or declared in the method signature using throws nếu không IDE sẽ báo lỗi ngay lập tức. VD: đội mũ bảo hiểm, `FileNotFoundException`
  - **Unchecked Exceptions** (Runtime Exceptions, con cháu của Class `RuntimeException`): không bắt buộc `try catch` mà IDE vẫn sẽ không báo lỗi. VD: mặc áo mưa, `NullPointerException, ArrayIndexOutOfBoundsException, NumberFormatException`

- Cơ chế bảo hộ exception chính là `try catch`
- Về mặt kỹ thuật thì Java cho phép mình dùng try-catch để bắt cả Error giống như bắt Exception, vì cả hai đều kế thừa từ lớp `Throwable`. Nhưng trong thực tế, việc catch Error thường không được khuyến khích ạ.

Nếu exception trong `if() {}` hay trong `try{}` thì những đoạn code nằm sau exception sẽ không được chạy.

Cái gì sẽ ném ra ngoại lệ? => Từ 1 hàm / constructor.

Có 2 loại Errors:

1. **compile time**: báo đỏ khi code trong IDE, cannot compile
2. **run time**: run project mới có lỗi, vẫn compile (biên dịch) được

In java's ngăn xếp gọi hàm (StackTrace) những lời gọi hàm mới nhất nằm ở trên, cũ nhất nằm bên dưới. Khi debug thì từ dòng ở cuối mò từng dòng lên trên. Mình cần tìm cái lời gọi hàm mới nhất (gần nhất latest).  
Phải dùng dòng `e.printStackTrace();` thì chương trình mới in ngăn xếp ra cho mình.

Because of **exception hierarchy**, nếu muốn catch tất cả exception thì dùng `Exception e` vì đây là parent class của tất cả exception classes.
Every exception in Java is child of `java.lang.Exception` class.

The `finally` statement always gets executed whether there are exceptions or not. **Even** if you put `return;` statement in the try-block. Therefore, you should avoid putting `return;` in the `finally{}` block since it will override `return;` in the try-block.

Thật ra cứ để code ra ngoài `try catch` thì nó cũng chạy y chang `finally`.

1. Nhưng nếu `throw` exception trong `catch {}` thì chỉ có `finally {}` thật chạy còn finally giả sẽ không chạy.
2. `return 0;` trong `catch{}` thì finally giả cũng không được chạy, chỉ chạy `finally {}` thật.

- Ứng dụng của `finally`:
  - close connection to database
  - close files đang mở

In the try-block, after an exception is thrown the rest of the code in the try block after that exception will NOT be executed.

5 tình huống sử dụng exeption từ câu chuyện giữa 2 người bạn thân học chung 1 lớp. Sáng sáng 2 thằng chở nhau đi học.

Ngày thứ 1: Đang đi trên đường, thằng ngồi trước phát hiện có cục gạch văng vô đầu. **Thằng trước lấy tay chụp cục gạch**.  
Cục gạch = Exception (xấu).

Ngày thứ 2: Đang đi trên đường, thằng ngồi trước phát hiện có cục gạch văng vô đầu hắn. Thằng ngồi trước hắn né, để thằng ngồi sau lo mà tránh. **Thằng ngồi sau thấy vậy chụp lấy cục gạch**.

Ngày thứ 3: Đang đi trên đường, thằng ngồi trước phát hiện có cục gạch văng vô đầu hắn. Thằng ngồi trước hắn né, để thằng ngồi sau lo mà tránh. **Thằng ngồi sau thấy vậy né luôn**.

Ngày thứ 4: Đang đi trên đường, thằng ngồi trước phát hiện có trái ổi văng vô đầu hắn. Thằng ngồi trước cắn 1 miếng, rồi ném phía sau. **Thằng ngồi sau thấy vậy chụp xong cắn tiếp một miếng nữa**.  
Trái ổi (thơm tho) = Exception (tốt).

Ngày thứ 5: Đang đi trên đường, thằng ngồi trước không phát hiện cái exception gì hết, mà hắn lén lấy cục gạch giấu trong người hắn giả vờ ném ra phía sau. Thằng phía sau làm gì thì đó là chuyện của thằng phía sau.  
Cục gạch (do thằng trước tự tạo ra) = Exception.

Hàm `main()` là thằng **ngồi phía sau** và nó sử dụng môt dịch vụ do **người ngồi trước** cung cấp tên là ôm eo `omEo()`.

Tình huống 1, dùng `try catch` ngay trong `omEo()` (thằng ngồi trước)

Tình huống 2, thằng ngồi trước muốn không bắt mà né vậy buộc nó phải khai báo rõ ràng cho thằng ngồi sau biết. Khi `main()` gọi `omEo()` thì **bắt buộc** phải `try catch`.  
Một hàm có quyền khai báo phóng ra nhiều exceptions khác nhau.

```java
public static void omEo() throws NullPointerException { // Khai báo rằng "tôi có kha nang se nem ra Exception nay""
  // omEo xem như là thang ngoi truoc
  String a = null;
  System.out.println(a.charAt(0));
}
```

Tình huống 03: thằng `omEo()` khai có throw exception nhưng thằng `main()` không `try catch`. Lúc chạy chương trình **chính thằng user** sẽ bị cái exception đập vào mặt red error screen and the program crash :(( => viên gạch văng vào mặt thằng chạy chương trình.

Tình huống 04: thằng `omEo()` có khai báo `throws`, có bắt `try catch` nhưng sau khi bắt & xử lý exception nó lại tiếp tục `throw` nữa cho người khác bắt. Thằng `main()` tiếp tục `try catch`. Vậy là chỉ có một exception mà bắt hai lần.  
**Rất thường gặp trong dự án** khi truy vấn SQL bị sai thì tầng xử lý DB nó sẽ bắt exception sau khi xử lý xong thì lại tiếp tục `throws` ra exeption đã bắt được để báo hiệu cho tầng phía trên biết là đã có một sự cố xuất hiện ở tầng DB.  
Mục đích là để giải quyết bài toán liên lạc giữa các tầng xử lý trong chương trình thông qua cơ chế `try catch` exception. Ở mỗi tầng sẽ luôn có những nhiệm vụ riêng khi gặp & xử lý exception. Phân tầng xử lý lỗi (Layered Exception Handling).

```java
public static void omEo() throws NullPointerException { // Khai bao tôi có kha nang se nem Exception nay
  // omEo xem như là thang ngoi truoc
  try {
    String a = null;
    System.out.println(a.charAt(0));
  } catch (NullPointerException e) {
    System.out.println("Thom qua, ngoi truoc cắn mot mien");
    throw e;
  }
}
```

Tình huống 05: thằng ngồi trước tự dựng chuyện ra rồi phóng exception cho thằng khác.

```java
public MyException extends Exception {
  // my custom exception
}

public static void omEo() throws NullPointerException { // Khai bao tôi có kha nang se nem Exception nay
  // omEo xem như là thang ngoi truoc
  System.out.println("Da den day 1!");
  throw new MyException();
}
```

### What if you don't catch it?

Each request is usually processed in its own thread (from a thread pool). The main application thread that started the server is unaffected by individual request failures.

Khi một request throw error thì other requests still continue normally because they are on different threads.

Nếu một thread bị free hoàn toàn (Example with an infinite loop `while (true) {}`). That one request thread is stuck forever. It doesn’t freeze the whole server, but it can eventually **exhaust the thread pool** if many such requests pile up.

If a runtime exception occurs **during application startup**, Spring may fail to start → the whole API won’t run. This is the closest analogy to “freezing” in JavaScript, but it happens at startup, not at runtime per request.

- JavaScript in the browser: single event loop → one uncaught error can block rendering if it hogs the thread.
- Spring (Java): multithreaded request handling → one request crashing does not block other requests or the server, unless it causes resource exhaustion (all threads stuck).

So in Spring Boot:

- Errors block only the current request thread, not the whole server.
- Unless you misuse threads (e.g. infinite loops, deadlocks), the API keeps serving other clients.

## Generic

 [This video](https://www.youtube.com/watch?v=K1iu1kXkVoA) by John explain generic good!

Generic means **generic classes**. Using Generics, it is possible to create classes that work with different data types (generic classes). An entity such as class, interface, or method that operates on a parameterized type is a **generic entity**.  
Generic còn gọi là parameterized types.  
Khi pass vô generics phải là reference type or wrapper classes like `Interger` không được pass primitive type (`int`).

`<T>` shorts for `<Type>`

Generic thường gặp trong `ArrayList<>`

Generic sẽ khiến code **type-safe** (giống như null-safe). Mình biết chắc là trong ArrayList chỉ có một loại duy nhất, không có những thứ khác có thể potentially cause error or exceptions. [This video](https://www.youtube.com/watch?v=K1iu1kXkVoA) explain it at around 8:50

You can narrow down the `<T>` that user can pass into your generics by using `<T extends ClassName>`. This is known as **bounded generic**.
NOTE: Nếu implement interface vẫn dùng `extends` chứ không dùng `implements` nha.
You can also multiple bounds.

In addition to having generic classes, you can also have have generic methods. They can take arguments of different types.

`<?>` is a generic wildcard

[Java extends vs implements](https://stackoverflow.com/questions/10839131/implements-vs-extends-when-to-use-whats-the-difference)

## Enums

You should use enum types any time you need to represent a fixed set of constants. That includes natural enum types such as the planets in our solar system and data sets where you know all possible values at compile time—for example, the choices on a menu, command line flags, and so on.

The constructor for an enum type must be package-private or private access. It automatically creates the constants that are defined at the beginning of the enum body. You cannot invoke an enum constructor yourself.

## Annonation (@)

[video](https://www.youtube.com/watch?v=DkZr7_c9ry8&t=8s) by John good!

Annotations are meta-data. Annotation is proccessed by compiler or at run-time.

`@Test` annotation dùng để khi build nó biết đây là test methods.

## Common data types

[List vs Array vs ArrayList in Java](https://www.reddit.com/r/javahelp/comments/16gjqkb/list_vs_array_vs_arraylist/)

Array have fixed size. ArrayList can grow and shrink.

List - an interface (not a class) that defines certain behavior. It cannot be instantiated on its own.

ArrayList - a concrete class that implements the List interface.

Java Collection Framework

## The build process, Maven && Gradle

[Java Build Overview: javac, Ant, Maven, Gradle, with demonstration](https://www.youtube.com/watch?v=E6dn3vRTBC8) Video ông này giải thích build process của Java rất hay. Ổng có compare file structure của 1 project build with maven và 1 project build bằng Intelliji build.

When you run Java program, a [JIT](https://www.geeksforgeeks.org/just-in-time-compiler/) (Just In Time Compiler) turn byte code into machine code and run it. The machine code is not stored on your computer, only the byte code.

Maven & Gradle are build tools that controls the development process in the tasks of compilation (using javac) and packaging to testing, deployment, and publishing. They can also manage dependencies & plugins (giống npm), versioning

Gradle is json-based, while maven is xml based. Gradle dùng cho android development.

what is the `anhao/.m2` do in maven => [stack overflow](https://stackoverflow.com/questions/63138495/what-is-m2-folder-how-can-i-configure-it-if-i-have-two-repos-with-two-differen)

### Bytecode & the JVM

When you program for the Java platform, you write source code in `.java` files and then compile them intp bytecode (`.class` files). Bytecode is run on a JVM. In adding this level of abstraction, the Java compiler differs from other language compilers, which write out assembly-language instructions suitable for the CPU chipset the program will run on.

At runtime, the JVM reads and interprets .class files and executes the program's instructions on the native hardware platform for which the JVM was written. The JVM interprets the bytecode just as a CPU would interpret assembly-language instructions. The difference is that the JVM is a piece of software written specifically for a particular platform. The JVM is the heart of the Java language's "write-once, run-anywhere" principle. Your code can run on any chipset for which a suitable JVM implementation is available. JVMs are available for major platforms like Linux and Windows, and subsets of the Java language have been implemented in JVMs for mobile phones and hobbyist chips.

## FAQs

[Cách viết javadoc cho methods](https://stackoverflow.com/questions/11763803/how-to-document-my-method-in-java-like-java-docs); [shorkey cho java doc trong netbean](https://stackoverflow.com/questions/2280261/adding-java-docs-to-a-program-in-netbeans)

[Cách dọc javadoc trong Netbean](https://stackoverflow.com/questions/6517266/how-to-see-javadoc-documentation-on-mouse-hover-in-netbeans)

NetBeans là 1 Java IDE được phát triển bởi Apache Software Foundation. Nên khi tải NetBeans sẽ phải vào trang của Apache.

`java --version`

[Guide to Java Versions and Features](https://dzone.com/articles/a-guide-to-java-versions-and-features) by my man Marco Behler

## JUnit test framework

Unit cần test trong Java là 1 class or 1 method inside class

Move cursor inside the class, hit `Cmd S t` to create unit test

[Junit test netbean video](https://www.youtube.com/watch?v=A6EDxG4lxu4) or the [blog post](https://netbeans.apache.org/tutorial/main/kb/docs/java/junit-intro/)

## Shotkeys

`^ spacebar` show code completion

`^ r` run project

IntelliJ IDEA

Place cursor on the errorneous line (yellow bulbs icons), press `Opt Return` to show quick fix menu. Máy window là `Alt Enter`

[tạo new package/file](https://www.jetbrains.com/help/idea/add-items-to-project.html): `cmd n`

[view file structure](https://www.jetbrains.com/help/idea/viewing-structure-of-a-source-file.html)

search everywhere: go to Navigate | Search Everywhere or press `⇧Shift` twice

[reddit config](https://www.reddit.com/r/vim/comments/1bbkyds/comment/kucg0nj/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)

[one more config](https://gist.github.com/AlexPl292/50a3ff4cef1badcbb23436b22cbd3cf4)

Netbean

`sout` => `System. out. println(“”)`

## Terminologies

[CI/CD](https://en.wikipedia.org/wiki/CI/CD)

JSP: Java Server Pages

Jakarta EE = Java EE (Enterprise Edition)
Jakarta Servlet = Java Servlet
[Why choose the capital city Jakarta?](https://www.quora.com/Why-did-Java-name-Jakarta-EE-even-though-Jakarta-is-the-capital-city-of-Indonesia)

JDBC: Java Database Connectivity. Cái này tự làm khá mệt, nên để spring framework làm dùm.

JCP: Java Community Process
JSRs: Java Specification Requests. Theo quy trình JCP thì có rất nhiều JSRs được đề xuất.
Umbrella JSR: a collection of other JSR and do not define any API
 Example: J2EE (Java to Enterprise Edition)

EJB: [Enterprise JavaBeans](https://en.wikipedia.org/wiki/Jakarta_Enterprise_Beans)

SFTP: [Secure File Transfer Protocol](https://www.youtube.com/watch?v=J1f5b4vcxCQ&t=137s)

DI: Dependency Injection

JRE: Java Runtime Environment. Contain JVM.
JVM: Java Virtual Machine
JDK: Java Development Kit current version 22. Contain JRE
Java Platform = Standard Edition (Java SE) version mới nhất là 8.

[Phân biệt construct vs constructor](https://stackoverflow.com/questions/23115462/what-is-the-meaning-of-construct-in-programming-languages)

OpenJDK: Open Java Development Kit

Object state là từ chỉ chung tất cả properties của object tại 1 thời điểm nhất định.

javac: the Java compiler

## References

[IBM developer Java](https://developer.ibm.com/languages/java/) has some very good tutorial.

[dev.java guides](https://dev.java/)

[doc.oracle java tutorial](https://docs.oracle.com/en/java/javase/22/)

[Marco blogs](https://www.marcobehler.com/) very good tutorials

[Jakarta EE Platform API doc](https://jakarta.ee/specifications/platform/9/apidocs/) có servlet
