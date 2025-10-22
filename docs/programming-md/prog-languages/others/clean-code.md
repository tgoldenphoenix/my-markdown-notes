# Clean code

SQL nên viết `IN ()` không viết `NOT IN ()`

`IN (1,2,NULL)` sẽ ra `=1, =2, ==NULL` mà `==NULL` ra `unknown`; phải dùng `IS NULL`

Trong javascript, dùng `let` & `const` phải phù hợp.

Chỉ khi function re-use được thì mới tạo riêng, nếu không thì dùng `anonymous function`.

Đảo ngược điều kiện để thoát sớm khỏi function, giúp code dễ đọc và dễ hiểu hơn.  
Cái code block nào ngắn hơn thì cho vào trong `if () {}`

Mỗi một pull request/task chỉ code đúng trong phạm vi task đó. Không code thêm ngoài phạm vi.
