# 02: EAD

[link MEGA](https://mega.nz/folder/Kmo2HTDT#T46XTCNri5lRPKsS8feMuA)

[link google sheet](https://docs.google.com/spreadsheets/d/1CnFiUr6hnf77XrO0z8a6g7vTvHKYNq1sGZMIc2okduM/edit?gid=0#gid=0) phải dùng mail FPT

## Thi qua môn

bài thi chú ý tạo table, thầy sẽ cho khác

bài lab06 transaction có thể biến thể làm bài thi

## Edunext

The `<c:choose>` in `welcome.jsp` [here](https://www.tutorialspoint.com/jsp/jstl_core_choose_tag.htm)

Account, AttendanceEmployee, Employee, Role, LeaveRequest

## B1: calculator, CRUD chưa DB, search by name

table: account

Dùng Ant instead of Maven: enterprise application. Lúc tạo project có 2 modules:\
ejb chứa business logic, jakatar enterprise bean (ejb)\
web app dùng để hiển thị

Phải tải file module `.jar` về add manually vào folder library. Ejb add 1 cái ejb api thôi. Còn war phải add 2 file .jar

Lab01.1\
ejb: interface của calculator, add, subtract\
web: tạo servlet, call ejb `@ejb`, hiển thị UI của calculator

Lab02.2\
Array student (chưa dùng database)
web: servlet CRUD (chưa update), search by name

Khi build phải đứng trong cái app (icon hình tam giác), phải start payara server trước. Khi build ejb thì trong module web war/library sẽ có 1 file jar do máy tự tạo.

Phải deploy rồi mới run thì mới chạy được.

## B2: 1table-banking-LoginDepositWithdraw-decimal

table: Account
login:2024101:123 code-pin

create bank account (chưa làm)\
login bằng code & pin. After login successfully, show account balance (readonly)\
deposit, withdraw money dùng `.add()` & `.subtract()`\
Cách lấy giá trị decimal từ form\
xử lý `<input type="number">` no negative, step 0.5

jdbc:postgresql://localhost:5432/EAD

jdbc:postgresql://localhost:5432/EAD [postgres on Default schema]

`org.postgresql.Driver`

phải add thêm gói jakarta persisten (tải về thêm)

về coi lại file persistence bài stored procedure

video khóa học

Persisten 3.0; bỏ to string, hash code, equal, thêm data source; add sql driver vào library

## B3: 1Table-Old Framework JSF & managed beans (không thi)

table: Employee

Facelets in Jakarta Enterprise Beans

jsf có extenstion là `.xhtml`. Managed beand dùng JSF. Trong Managed bean định nghĩa lại model trong EJB.

vào service -> payara -> application -> un-deploy

war add 4 file .jar; trong ejb add 2 cái (ejb, persistent, ko cần driver sql thử coi)

mb = managed bean

tạo jsf page instead of jsp

buổi sau học Java Apache Strut framework (very old). still used by the japanese

## B4: 2table-Login-Employee-Department-CRUD

2 tables: tbEmployee, tbDepartment quan hệ 1 nhiều

Login: 1(id):123

Login phân quyền:\
employee chỉ nhìn thấy detail của bản thân `detail.jsp`.\
Admin sẽ được crud, reset password, update salary trong `index.jsp`\
dùng servlet JSP gọi ejb

**Homework**: update salary -> bay qua employee coi có cập nhập chưa?\
reset password (auto về 123)\
Về coi lại create query, create update. Bữa sau tự làm. phân biệt xử lý logic của deposit vs authentication trong session bean\
về làm thêm request là xong edunext nhóm mình [database design](https://stackoverflow.com/questions/3939764/what-is-a-good-database-design-schema-for-a-attendance-database)

Một admin khi vào trang index sẽ ko thấy info của các admin khác mà chỉ thấy info user => Trong session bean, câu sql thêm 1 cái WHERE clause.

Khi generate entity classes from DB: master (department table) là mờ, detail (employee) không mờ. Nhớ đổi lại tên lớp cho nó gọn, tên trong DB dài dòng (`tbEmployee` => `Employee`).

**Bugs**

Phải thêm `.jar` postgres driver vào Library của EJB folder nếu không sẽ gặp lỗi.

Dùng data type `bit` trong database sẽ gặp lỗi. Phải dùng boolean.

## B5: 2TableJPA-Form ValidationHibernate-ComplexLogin-DeleteUser-ResetPass-ChangeStatus

2 tables: tbRoles, tbUser (quan hệ 1 - n)

Admin: U101:123; User: U103:123; 102 là user nhưng access denied

check login: nếu role user (2) hiện user detail. Nếu là role admin (2) hiện list users (ko hiện những admin khác), admin có thể tạo user mới\
Khi login nếu user bị lock thì hiện error access denied\
hiện detail user, find one, update\
form validation dùng hibernate: add 2 library hiber trong ejb, 1 library jakarta validation trong war. Viết kiểu manual thì viết trong servlet, không add library.

Đi thi có thể tùy chọn: jdbc or jpa; servlet or managed bean

có 4 kiểu JDBC, mình xài kiểu 4, thông qua TCP/IP

U101:123 admin Frodo
U102 user Active Gandalf
U103 user locked Samwise

## B6: MoneyTransaction-JDBC-API

2 tables: tbAccount, tbTransaction\
có dùng decimal bignumber

Login: A101:123

Khác với lab02 which is deposit/withdraw to one account. Lab06 này là from one account -> another account

Trong BankSB:transfer() có dùng .begin() .commit() để phân định các tiểu trình

## B7: Booking-Appointment-ComboBox-selectVIEW-DateHour, combobox

Table: Doctor, Patient
Patient can login/register. After login they can book an appointment.
Doctor can view all their booking record and cancel appointment.
get appointment từ  `vwappointment` thay vì `appointment` rồi reference 2 lần\
combobox đổ lên từ DB(select option html)

Homework: thêm phần cancel (thêm status vào database)

## B8: Attendance-LeaveRequest, approve status

table: employees có 's', leaveRequests. Có boolean.\
Login phân theo role.\
Làm nút approve switch status.\
cách làm `createNativeQuery` truyền tham số theo kiểu 1, 2, 3,4 ? ? ? ?

202401 là manager
202403, 04 là staff

có thể gộp 2 tables leave request và attendance

Dòng rollback

## B9: Edunext, bài tập tự làm lấy điểm, radio button

manufacturer product\
radio button, check box

## B10: mã hóa password

Aspect-oriented programming (AOP) trong Spring.

authentication (xác thực) - who are you? Do we have your info in our Database.
authorization (phân quyền) - what is your role?

add gói `xml.bind` vào thư mục ejb để làm mã hóa password

`pstmt.setString(1, newProgram.getName());`\
set parameter trong JDBC, dùng số 1 số 2. Dùng với [prepared statement](https://en.wikipedia.org/wiki/Prepared_statement)

`.setParameter("employeeid", id)`
set parameter trong JSP. Dùng với JPQL

## Terms & References

SB: session Bean. Khi tạo thì select stateless, local interface.

EL expression: Expression Language `${value here}`

DI & IoC (Dependency Injection & Inversion of Control) model

Xử lý dị bộ class thread or interface runable

