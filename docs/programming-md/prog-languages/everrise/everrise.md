# Everrise

## Quy trình làm việc

Quy trình nhận và thực hiện task: <https://ever-rise.backlog.jp/alias/wiki/411677>

Commit messge: copy task id & task name

- Tạo issue/task phải:
  - chỉ rõ repo
  - ko tạo task mà chỉ có mỗi mình mình biết
  - Lấy tên issue làm tên branch & commit

- Viết report cuối ngày:
  - tiến độ hoàn thành hay chưa
  - Add issue id nếu có

thấy công việc nào cần thì tạo ticket để đó, chưa có solution cũng cứ tạo để đó cho sếp coi

Mỗi 1 task & pull request chỉ chỉnh sửa code trong phạm vi task đó; không chỉnh sửa code ngoài phạm vi của task đó.

viết code phải có tính module để có thể tái sử dụng

đặt tên task là cái mục đích goal của task, còn cái action thì để trong phần description.

Đọc mail remail cuối ngày: <https://groups.google.com/> (phải được add vào đã).

Kế hoạch training: <https://ever-rise.backlog.jp/alias/wiki/560214>

- sếp yêu cầu làm việc vận hành bằng ticket cho tốt hơn:
  - không tạo ticket chỉ có tiêu đề không. Tiêu đề không phải là sửa cái gì mà ý nghĩa của việc đó là gì
  - có bao nhiêu vấn đề cần giải quyết thì tạo ticket hết, còn việc có sửa có giải quyết hay không thì tính sao
  - Mỗi ticket cần làm rõ Goal/ mục tiêu cần đạt được là gì

- Mỗi ticket thì phải có ít nhất 3 điểm/ phần:
  1. Vấn đề gặp phải là gì?
  2. Tại sao lại làm lại chỉnh sửa như vậy
  3. Hiệu quả của việc đó là gì?

Ví dụ 1

1. Vấn đề tên biến không đồng nhất, sẽ gây khó hiểu trong lúc đọc source
2. Cần đồng nhất tên biến trong hệ thống
3. Hiệu quả : sẽ làm cho source trở nên dễ đọc dễ bảo trì hơn

commit là phải có test và phải sài được. Không commit những thứ mà mình chưa dùng đến.

## Họp kỹ thuật tuần

Họp kỹ thuật thứ 5 xen kẽ (2 tuần 1 lần) vào lúc 4h chiều.

GCP (google cloud platform)

book clean code

[x] so sánh .equal vs ==: String vs Long

- Chủ đề tuần sau
  - Java: so sánh == và equals() với kiểu dữ liệu Long (Vĩ)
  - Một số vấn đề thắc mắc về Git (các bạn mới có thể chuẩn bị câu hỏi nếu có)
Vấn đề tiếp theo của Clean Code (đến giờ vẫn chưa có người phụ trách)
Refactor code (Mr Hào chuẩn bị phần này, xoáy vào những vấn đề được sếp yêu cầu thay đổi của phần Javascript thực hiện nút Copy backlog)
....VÀ những vấn đề gặp phải trong 2 tuần (4,6,8,10 tuần) vừa qua. Nói tóm lại khi gặp phải vấn đề thì note lại để chia sẻ, nếu hôm đó chưa có thời gian nói thì tiếp tục để hôm tiếp theo (có thể đăng ký trước nội dung cho tuần sau)

### Nội dung được giao phải chuẩn bị

k

## Nghỉ phép & Lương thưởng

1 tháng 1 ngày phép có lương

tiền lương: có tiền giữ xe 200

đóng tiền quỹ 50 hàng tháng

## Read if have time

<https://opensource.com/users/seth>

<https://grahamhelton.com/>

**test rail** manage test cases

meta program?

`adrepo`

<https://codeforces.com/>

<https://cses.fi/>

`Manim` python library to draw

- Numeral Systems in Programming: <https://www.youtube.com/watch?v=e9MOCR8oX0A>
- Number system playlist: <https://www.youtube.com/watch?v=VLflTjd3lWA\&list=PL0o\_zxa4K1BXCpQbUdf0htZE8SS0PYjy->

- java 8:
  - Stream API

- SVG in HTML:
  - <https://www.joshwcomeau.com/svg/friendly-introduction-to-svg/>
  - <https://discourse.mozilla.org/t/solved-svg-path-does-not-show/98813>

nếu rebase không được thì tạo nhánh mới + cherry pick

sau khi rebase (kể cả không có conflict) cũng phải diff commit để tránh trường hợp rebase auto-merge không theo ý muốn

1. <https://fullstackopen.com/en/>
2. <https://fastapi.tiangolo.com/tutorial/>
3. <https://github.com/donnemartin/system-design-primer>
4. <https://missing.csail.mit.edu/>

- Demo 2 commands:
  - `git rebase -i origin/main`  
  - `rebase -i HEAD` to re-write commit history, squash, re-order

assert equal
assert that
Tạo mock object
temporary file/folder
test với dữ liệu từ file .csv file thay vì dùng database thực

code dùng dependency do người khác viết, trước khi rebase chạy code ok. Nhưng người kia change dependency mình dùng. Sau khi rebase (dù ko có conflict) sẽ không chạy OK nữa.

unit vs integration test in java

so sánh .equal vs ==: String vs Long  
bài học rút ra là sử dụng .equal, không dùng `==` để so sánh

`gitk` command show graph

postman vs swagger

git fech origin main chỉ fetch duy nhất main thôi

Thêm demo vào return a new promise  in `.then` vs return a normal value

Cách resolve commit with local ide code editor not on web

## Japanese

Vocab: <https://ever-rise.backlog.jp/alias/wiki/371772>

SEO: <https://ever-rise.backlog.jp/alias/wiki/372411>

<https://ever-rise.backlog.jp/alias/wiki/199077>

## File excel của anh Việt

Attendance - Equipment Management: <https://docs.google.com/spreadsheets/d/1yvuO-rSCk6Nkhr3GG8g3zJCsikVLkfhDJqV1yut34OI/edit?pli=1&gid=1753145395#gid=1753145395>

## Xin nghỉ

Trưa nay em có cuộc họp từ 12h-14h nên em xin phép dời thời gian nghỉ trưa sang 30 phút sau khi họp (14h-14h30) 🙇‍♀️

Ngày mai em nghỉ phép để đi làm giấy tờ

em xin nghỉ phép sáng T3 25/11 để đưa cha đi khám bệnh ạ.

tuần tới em xin phép nghỉ t3 t4 - 25,26/11 ạ. Em có việc về nhà ạ.

Em xin phép đổi ngày nghỉ từ 1 ngày thành nghỉ 0.5 ngày (buổi chiều) ạ.v🙇

## Task hiện tai

xếp muốn dùng firstButton, không dùng editButton
script.js trong readme sai chính tả
đổi tiêu đề h3 (đã đổi rồi!!!)
thêm code minh họa phần thêm api key
ngoặc kép tampermonkey
line 45
chọn "dòng" Create a new script
cho cái message confirm vào một biến riêng lẻ
tách riêng một function để lấy id từ url (Dũ)

```javascript
const anhao = "anhao";
const regex = new RegExp(`^${anhao} (.$`);
console.log(regex.test("anhao 9"));

// const exactPattern = /^anhao \(.\)$/;
// const anhao = "anhao";
// const regex = `/^${anhao} \(.\)$/`;

// console.log(regex.test("anhao (9)"));
```

Tên biến

```javascript
const userResponse = confirm("...");

if (userResponse) {}
```

- ``new RegExp(`^${RegExp.escape(wikiPageName)}$`)``: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/RegExp/test>
- `.search(/[^\w\s']/g)`: <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/String/search>

* The page name has invalid characters (`\(backslash), ., .., ./, ../`).
* `22/22/22/22/333` is valid
* `.....` invalid
* `/.` valid

* tạo class cho cái màn hình
* gộp 2 function copy wiki page & copy image vào button event listener luôn
* thiết kế để người dùng ko cảm nhận được copy là có 2 bước (copy & upload ảnh) => chuyển qua dùng `local storage` thay vì `session storage`.

## Miscellaneous

- <https://slackmojis.com/>
- <https://ezgif.com/resize>

[Sách networking resource](https://media.pearsoncmg.com/bc/abp/cs-resources/products/product.html#student,isbn=0136681557)

dimension là một khái niệm trong quảng cáo, không phải khái niệm trong SQL

