# Agile & Waterfall

- Nhung dự án như kế toán, bán hàng không cần sáng tạo, mà phải làm đúng plan đã có sẵn thì dùng waterfall.
- Agile scrum dùng cho những dự án cần tính sáng tạo, những loại phần mềm chưa có ai viết, không có sản phẩm tương tự.

## Waterfall

Nhóm phát triển dự án **chỉ** chuyển sang giai đoạn phát triển hoặc thử nghiệm tiếp theo **nếu** bước trước đó hoàn thành thành công.

## Scrum

Scrum framework là một mô hình dựa trên tư duy agile mindset.

Một **Sprint** là 1-4 tuần. Thời gian mỗi sprint là **KHÔNG** được phép thay đổi. Không được nhanh hơn hay trễ hơn.

- Nhanh hơn thì kiếm thêm task làm
- Trễ hơn thì bỏ bớt task đi

user story

Cuộc họp khởi động dự án (KICK-OFF) 2 tiếng.

- product backlog: những yêu cầu dự án xắp xếp theo thứ tự ưu tiên
- sprint backlog: chọn một vài item từ product backlog để làm trong một spring 3 tuần.

Product Backlog Item (PBIs) = user stories, requirements.

### Daily Scrum

Daily Scrum hợp mỗi ngày 30 minutes vào buổi sáng. The development team synchronizes their work, inspects progress toward the Sprint Goal, and plans beforehand the next 24 hours.  
Trong khi họp hằng ngày thì phân công 1 bạn viết biên bản cuộc họp (Meeting minutes).

Hằng ngày, sau khi dự họp Daily Scrum xong, các thành viên trong nhóm (kể cả nhóm trưởng), phải tiến hành làm **work break down** cho các công việc sẽ tiến hành trong ngày hôm đó. Mục đích của việc này là giúp cho các bạn có kỹ năng plan trước được công việc mình phải làm trong một ngày và ước lượng được thời gian làm việc cho từng việc, để dễ quản lý công việc của bản thân.  
File này được đưa lên google drive của dự án, update hằng ngày.

Hằng ngày, nếu thành viên nào ko làm kịp task của mình thì phải báo trước cho nhóm trưởng trước ít nhất 1/4 thời lượng của task đó.  
Ví dụ: Task viết testcase màn hình A được estimate là làm 2 tiếng thì phải báo trước 30 phút trước khi task đó bị đến hạn deadline mà ko làm kịp.  
Cách thông báo: Gửi email cho nhóm trưởng (để làm bằng chứng, sau này Scrum master sẽ audit)

- Sau khi hoàn thành 1 task, các thành viên trong nhóm phải làm 3 việc:
  1. Thông báo trên group chat hoặc gửi mail cho nhóm biết là mình đã hoàn thành Task.
  2. Đưa sản phẩm của mình lên Github hoặc Google Drive
  3. Chuyển trạng thái của task (issue) trên Github thành Closed và đưa task đó sang Column 'Done' trong bảng KANBAN.

Đầu mỗi buổi sáng, mỗi thành viên sẽ copy toàn bộ các sản phẩm mình đã làm được tính đến cuối ngày hôm trước lên google drive của cá nhân, share quyền public và đưa link cho mentor. Để khi nào rảnh, mentor có thể vào review được các sản phẩm của các bạn luôn.  
Nếu ngày hôm qua không có tạo ra sản phẩm gì mới so với ngày trước đó thì hãy copy sản phẩm của ngày trước đó để lên google drive. Hãy sử dụng 1 thư mục cố định. Mỗi ngày thì tạo một thư mục nhỏ bên trong đặt tên là Day1, Day2… để khi nào rảnh, mentor có thể vào review được các sản phẩm của các bạn luôn.

status: Todo, In Progress, Done

## Roles

**Product owner** đại diện cho khách hàng, người trả tiền.

**Scrum master** thì tương đương với **product manager** trong water fall.

## Github

log task, log bug, log issue

Mỗi Task ở đây được quy định thành Issue trong GITHUB. Nhóm trưởng sẽ là người tạo ra các TASK này và giao cho các thành viên trong nhóm. Tất cả các việc từ nhỏ đến lớn trong dự án bắt buộc phải tạo thành TASK trên GITHUB. Mỗi TASK bắt buộc phải có Deadline hoàn thành y như dự án thật.

Nhóm trưởng sẽ có trách nhiệm add các task vào COLUMN (bảng) ToDo, còn các thành viên trong nhóm sẽ có trách nhiệm chuyển các Task sang bảng In Progress và Done.

Nhóm trưởng khi tạo TASK sẽ gán (giao việc) cho ai thì assign cho người đó.

### Label task

Đối với loại issue, chúng ta hãy gán label là 'Task' để phân biệt được đây là Task. Label `Task` này cần được gán ngay khi tạo mới issue task.

Đối với việc đánh giá tiến độ của Task, chúng ta chỉ cần dùng 3 label ở đây.

Sau khi 1 Task nào đó được hoàn thành, nhóm trưởng sẽ vào đánh giá là TASK đó đã thực hiện đúng hạn hay không.  
Nếu đúng hạn thì gán label cho Task đó là: `On-time`, nếu chậm tiến độ thì gán label là `Delayed`, nếu sớm hơn tiến độ thì gán label là `Over-Progress`.

Nhớ là chỉ gán label về tiến độ sau khi TASK đã hoàn thành (tức là đã được chuyển sang COLUMN DONE).

## Jira

