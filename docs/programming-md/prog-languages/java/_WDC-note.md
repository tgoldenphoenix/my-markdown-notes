# 01: WDC, Java main Notes

## B2: DI, 1stJDBC

int, string

Connect to PostgreSQL bằng jdbc driver. Add từ form -> DB.

Làm DI, tạo file dao (data access object) trong folder service để CRUD vào database. Servlet (controller) gọi DAO.

Làm DI có interface. File DAO implement interface.

Có viết main() method trong ConnectDB để debug.

Trong HomeServlet viết gộp GET & POST vào trong processRequest (tên gì cũng được.)

class ProgramDAO inject ConnectDB trong util để connect vời DB.

## B3: Login DB, lombok, URL re-writing, cookie, session (edit chưa xong)

Buổi này có 2 project folders.

Project 1 demo

- login with url re-writing. Khi login thành công thì gắn username vào url (session) like `http://localhost:8080/Servlet?username=fpt`
- login with cookie
- login with session, request timeout tự động nhảy về trang login sau 1 khoảng thời gian in-active

Project 2 demo:

- Login so sánh với DB, không set cứng nữa
- Login thành công nếu role admin thì vào trang detail; role user thì vào trang index (2 trang khác nhau). User thì ẩn những account admin đi.
- CRUD account
- Dùng tag `include` trong JSP để thêm trang `header.jsp` vào trang index

Các business logic xử lý trong class DAO (phân biệt với DTO). Servlet xử lý các request, điều hướng.

[http only](https://stackoverflow.com/questions/73578301/can-you-briefly-explain-the-difference-between-httponly-cookies-and-normal-coook)

về coi lại cách viết stored procedure buổi sau học tới

tải lombok khỏi cần getter & setter thủ công, có viết sẵn.

dùng thẻ include trong file jsp để link `header.jsp` vào `index.jsp`

TODO: về làm if nếu admin thì hiện detail, delete, create

## B4: Full CRUD, search by name, stored procedure

dùng bootstrap

copy 2 file
bootstrap.min.css
bootstrap.bundle.min.js
bootstrap.min.js

Functionalities: bấm nút detail chuyển qua trang edit

Phân biệt câu truy vấn đơn vs stored procedure.

Rule of thumb: 1 entity == 1 servelt no more

stored procedure tạo lưu vào DB, bên code mình chỉ gọi thôi không có create

.executeUpdate thành công trả về 1, thất bại trả về 0

trong file jsp impression language là `${}`. phải dùng tablib core mới xài đượcs

Trong postgreSQL cú pháp JDBC để call procedure [khác](https://stackoverflow.com/questions/65116924/x-is-a-procedure-use-call-when-i-am-already-using-call)

[Procedures in PostgreSQL (Oracle, DB2) are not same like procedures in MS-SQL](https://stackoverflow.com/questions/52064701/select-usage-with-the-new-create-procedure-method)

[Calling Stored Functions and Procedures](https://jdbc.postgresql.org/documentation/callproc/)

## B5: upload image file

## B6: 1stJPA CRUD & multi-delete

Login: admin:123 (lấy từ bảng tạo trước trong pgadmin)
Muốn test code phải bật pgAdmin trước

**Tính năng chính**
Buổi 6 này dùng JPA nhưng tạo DB bằng SQL script bên pgadmin xong rồi map qua code. Phải vào window Services -> Databases tạo 1 cái connection.\
Có tính năng xóa 1 lúc nhiều rows selected.

SQL server từ version 8 trở lên phải kết nối qua SSL security socket layer, phải thêm trust certificate trong connection string.

chọn schema dbo (database owner) or public nếu dùng postgresql

Cách tạo file entity từ DB có sẵn: right-click source folder -> entity classes from DB

localhost:4848 để mở payara trên web browser

Trong class `entity manager` có: find, persist (save), merge (update) & remove

không cần làm JNDI pool vì netbean-22 có sẵn rồi.

## B7: 2 bảng có quan hệ & search min-max, filter

Thi không có cho 2 bảng.

sort khác filter?
sort 1 tăng 2 giảm

**Funtionalities**
2 tables: department, employee. 1 phòng ban có nhiều employee
Search min-max salary
Filter: chọn department, only show employee thuộc department đó

Khi create new employee, phải lấy department từ DB đổ lên form.

Phải vào netbean tab service -> connect to the DB trước rồi mới chạy code.
Nếu tạo entity class from DB mà không có hiện sẵn thì phải vào services start Payara trước. Sau đó chọn quan hệ list, sửa lại tên package, tick chọn generate named querry. Không chọn JBAX.

Có 3 loại quan hệ:

- 1 nhiều
- 1 - 1
- nhiều nhiều: chèn bảng trung gian

**parent table vs child table**
Trong bài này table department là bảng master (or parent table), employee là bảng detail (child table) sẽ hiện mờ  mờ khi add entity trong netbean. Bảng chứa foreign key reference đến bảng khác luôn là bảng detail (child).

Vô department tự chèn thêm 1 constructor 2 tham số (name description). Trong employee thêm 1  constructor 4 tham số

Trong employee, id foreign key là 1 object không phải integer.

trong file index lúc đổ ra tên department
mở `employee.java` lên coi sẽ hiểu

async là dị bộ
sync là đồng bộ

EL expressions `${}` [here](https://docs.oracle.com/javaee/6/tutorial/doc/bnaim.html) expression language.

Khai báo async ở trong class thì ko khai báo trong file web.xml

## B8: login authentication

Login này khác B3 ở chỗ có dựa vào role mà chuyển trang.

Thi có 2 dạng: basic, form-based. Mà muốn làm 2 dạng này phải lưu login info trên web-server, không lưu trên DB như buổi 3. Sau khi login vào thì làm 2 chức năng CRUD.

Trong basic ko cần tạo form, mà dùng pop-up của browser để login.

Realms là 1 database nhỏ không cần cấu hình. Bữa nay không lưu user information trong DB postgresql mà lưu trong web server (payara) which is Realm database.

admin:Admin:123
fpt:Manager:123

login basic/form-based + 2 task in CRUD either JDBC or JPA

Configurations > server config > Security > Realms > file

### Basic authentication

vào `web.xml` -> tab page -> chọn welcome file
vào tab security, cái role name phải trùng với cái bên Payara server. Group list là cái role name
constraint: cái resource là cái folder `Admin` mình tạo

Connect DB trong DAO luôn không tạo util

## B9: Hibernate validation (chưa xong)

Làm dựa theo pretest03, validation

Có 2 kiểu data types: built-in (value type) vs reference data type

nhỏ qua lớn float = int thì compiler tự làm. Còn int = float thì phải casting manually.

built-in -> ref type là boxing. Ref type -> built-in là un-boxing.

object = built-in compiler tự ép kiểu.

type casting = type coercion

Có 2 kiểu rằng buộc: rằng buộc trên DB (SQL script) & rằng buộc trên form (hibernate validation, jakarta validation API)

gói jakarta validation API có sẵn trong jakarta EE không cần khai báo trong pon

Khai rằng buộc trên entity hoặc xử lý hết bên controller

Cách tải gói jar từ trên mạng: manually install artifact

không được dùng thuộc tính HTML để validation

## Important notes

trong đề thi qua môn dùng jdbc hoặc jpa. Tránh lạc đề

Trong html form chỉ có 2 method GET, POST. Không có 4 methods

jakara ee 10 - java servlet 6.0 là bản mình dùng

[version jakata ee vs servlet](https://en.wikipedia.org/wiki/Jakarta_Servlet#History)

[javax vs jakata](https://dev.to/tbroyer/the-javax-to-jakarta-mess-its-even-worse-than-i-thought-54ag/comments)

bài 50 điểm mini asm cũng làm code query sql, nộp luôn querry

Nếu so sánh với mô hình mvc thì servlet == controller

Chỉ có thể đổ data & nhận data (từ form) from/to servlet thông qua file jsp chứ không dùng file html được.

dal = data access layer

`.sendRedirect()` khỏi cần dùng `.getRequestDispatcher()`; Nhưng `. include` với forward thì cần.

[What the hell is servlet?](https://www.reddit.com/r/programming/comments/11p5z5e/ques_what_is_a_servlet_i_am_really_confused_some/)

[JSP doc](https://docs.oracle.com/javaee/5/tutorial/doc/bnajo.html)

[Servlet doc](https://jakarta.ee/specifications/servlet/4.0/apidocs/)

thực thi prepareStatement

- select -> executeQuery()
- insert, update, delete -> executeUpdate()

`for=""` trong `<label>` match with `id=""` in `<input>`. This is for HTML purpose only.
`<input>`'s `name=""` attribute is for the servlet to get data.

createQuery and .createNamedQuery:

- đều dùng JPQL
- The `createQuery` method is used to create dynamic queries (được tạo ra ở lớp logic).
- The createNamedQuery method is used to create static queries, or queries that are defined in metadata by using the `@NamedQuery` annotation.

## Terminologies

jsp = java server page = jakarta. file `.jsp` giống như file `.html`. Dùng jsp + servlets để qua môn đầu tiên

dao: data access object
dto: data transfer object
dal: data access layer

jpa: java persistent api

JPQL: Java Persistence Query language
`@NamedQuery`

## References

[northwind: database mẫu của microsoft](https://en.wikiversity.org/wiki/Database_Examples/Northwind/SQL_Server)

[JPQL doc from Oracle](https://docs.oracle.com/javaee/6/tutorial/doc/bnbtg.html) chủ yếu giải thích mấy cái method

[JPQL syntax guide](https://thorben-janssen.com/jpql/)

