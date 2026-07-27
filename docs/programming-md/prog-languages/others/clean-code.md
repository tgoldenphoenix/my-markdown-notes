# Clean Code

SQL nên viết `IN ()` không viết `NOT IN ()`

`IN (1,2,NULL)` sẽ ra `=1, =2, ==NULL` mà `==NULL` ra `unknown`; phải dùng `IS NULL`

Trong javascript, dùng `let` & `const` phải phù hợp.

Chỉ khi function re-use được thì mới tạo riêng, nếu không thì dùng `anonymous function`.

Đảo ngược điều kiện để thoát sớm khỏi function, giúp code dễ đọc và dễ hiểu hơn.  
Cái code block nào ngắn hơn thì cho vào trong `if () {}`

Mỗi một pull request/task chỉ code đúng trong phạm vi task đó. Không code thêm ngoài phạm vi.

Sắp xếp lại thứ tự từ trong tên biến theo cách của tiếng Anh `listOfAttachment` => `attachmentList`

Nếu không cần dùng đến index thì hãy dùng for...of thay vì `for` loop bình thường

Các biến mang tính hằng số thì đặt theo kiểu Upper Snake Case bằng chữ in hoa `API_KEY`, `BASE_URL`

Tên biến nên thể hiện ý nghĩa của giá trị mà nó chứa, không phải quá trình mà nó được gán giá trị

```javascript
const confirmCopyAction = confirm("Are you sure you want to copy this page?\n\nThis will create a new wiki page with the same content and attachments.\nYou will be redirected to the new page after the copy is complete.\n\n***NOTE***: If the original page has many large attachments, the copy process may take a few seconds. Please be patient.");
```

- xử lý lỗi giống như đi uống bia:
  * sau khi uống hết rồi mới dọn hoặc
  * biết trước lỗi gì sẽ xảy ra và bắt luôn

## Comment

Ghi chú là những '''thất bại''' về khả năng diễn đạt: Nếu người viết có khả năng viết đoạn code đủ để diễn đạt mục đích của mình, thì không cần phải ghi chú nhiều, có thể là hoàn toàn không.

** Comment rất khó quản lý: Code liên tục thay đổi và phát triển. Nhưng những comment không thể luôn luôn kèm theo những dòng code.

Việc bảo trì comment theo thời gian mất thời gian + công sức hơn rất nhiều so với viết code tốt từ đầu.

Comment không chính xác còn NGUY HIỂM hơn là không viết comment: Hãy đảm bảo bản thân hiểu đúng nội dung mình comment, nếu không hiểu, đừng viết chúng.

Tuy nhiên thực tế thì thông thường không thể không viết, nên hãy tìm hiểu/hỏi/xác nhận toàn bộ

## Functions

Dùng ngoại lệ thay vì mã lỗi

```java
// Không tốt
int parse(String s) {
    if (s == null) return -1;
    return Integer.parseInt(s);
}

// Tốt
int parse(String s) {
    if (s == null) throw new IllegalArgumentException("Null input");
    return Integer.parseInt(s);
}
```

Hàm chỉ nên làm một việc

```java
// Không tốt
void saveUserAndLog(User user) {
    Database.save(user);
    Logger.log(user);
}

// Tốt
void saveUser(User user) {
    Database.save(user);
}

void logUser(User user) {
    Logger.log(user);
}
```

Ưu tiên trả về giá trị. Hàm nên có đầu ra rõ ràng → dễ test hơn.

```java
// Tốt
int calculateSalary(Employee e) {
    return e.base * 2;
}
```