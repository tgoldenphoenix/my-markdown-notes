# Clean code

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
