# Everrise

## Quy trình làm việc

Commit messge: copy task id & task name

- Tạo issue/task phải:
  - chỉ rõ repo
  - ko tạo task mà chỉ có mỗi mình mình biết
  - Lấy tên issue làm tên branch & commit

- Viết report cuối ngày:
  - tiến độ hoàn thành hay chưa
  - Add issue id nếu có

## Họp kỹ thuật tuần

Họp kỹ thuật thứ 5 hằng tuần

GCP (google cloud platform)

book clean code

so sánh .equal vs ==: String vs Long

--- Share ---

1. <https://fullstackopen.com/en/>
2. <https://fastapi.tiangolo.com/tutorial/>
3. <https://github.com/donnemartin/system-design-primer>
4. <https://missing.csail.mit.edu/>

## Read if have time

- SVG in HTML:
  - <https://www.joshwcomeau.com/svg/friendly-introduction-to-svg/>
  - <https://discourse.mozilla.org/t/solved-svg-path-does-not-show/98813>

`git push origin HEAD` => nhanh hơn

nếu rebase không được thì tạo nhánh mới + cherry pick

sau khi rebase (kể cả không có conflict) cũng phải diff commit để tránh trường hợp rebase auto-merge không theo ý muốn

`useMemo()` react hook

- Lear more about rebase:
  - <https://dev.to/joemsak/git-rebase-explained-and-eventually-illustrated-5hlb>

code dùng dependency do người khác viết, trước khi rebase chạy code ok. Nhưng người kia change dependency mình dùng. Sau khi rebase (dù ko có conflict) sẽ không chạy OK nữa.

`rebase -i HEAD`

rebase khi nào có conflict?

so sánh .equal vs ==: String vs Long

Tìm sách đọc để hiểu hơn về HTTP protocol, tập dùng `fetch API`, HTTP requests in web application.

git fech origin main chỉ fetch duy nhất main thôi

Thêm demo vào return a new promise  in `.then` vs return a normal value

Cách resolve commit with ide not on web
