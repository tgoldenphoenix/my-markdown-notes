# Adrepo Notes

## Current Notes

`co.everrise.batch.tiktok.BatchGetMasterTiktok`

`co.everrise.batch.catalog.REPORT_TYPE` enum

## Questions

Module `co.everrise.ag` chứa gì?

dao vs dto

Tại sao: do thay đổi của tiktok gần đây, nhận thấy rằng data của AdEbis là chưa đủ

Mỗi tài khoản parent account chỉ được phép một platform hay nhiều? Có bao nhiêu token?

## Basics

Có các actors: end user, adrepo, platform, etl, web api của everrise

- End user
  - chạy quảng cáo
  - Vào front end của ad repo đăng ký để được nhận report quảng cáo
  - adrepo re-direct tới web api của everrise

End user có tài khoảng quảng cáo trên các platform là các api của twitter, facebook

Everries làm việc với adrepo, báo cáo cho ads report cho phía adrepo, không làm trược tiếp với khách hàng end user.  
Adrepo là khách hàng của everrise.

web api (elt harbest) nhận re-direct từ adrepo, cho advertiser authenticate trên platform. Sau đó api lấy token của khách hàng lưu vào database. Phía adrepo không lưu token ủy quyền của khách hàng.

We only lấy thành tích quảng cáo (the only thing we care about). Everries không chạy quảng cáo.

Repository fujiyama lấy report quảng cáo (batch)

Phía everrise không làm front end, đã bán front-end cho phía ad repo

Everrise trả dữ liệu report cho adrepo, adrepo trả cho end user

adrepo **không** có token của khách hàng, mình mới có token và lưu token vào data base của mình. Adrepo redirect user về everrise để ERV lấy token

end user only work with adrepo (sale), end user không biết sự tồn tại của everrise

batch nằm trên ec2, rds (relational database) cũng là service trên aws

- ELT harbest -> `play` framwork (for handling the HTTP request stuff), java, only use scala build tool
- ETL batch -> no framework, java thuần

- batch create queue tạo, tạo rồi batch export
- batch getReport data, getMaster data

- table names: `queue`, `dsp_advertiser`
- Cả 2 phía etl & adrepo đều có table `queue` cái tên giống nhau.

## Code Flow

tìm queue trong table -> xử lý queue, status 2 sucess, show log, upload lên s3

adrepo call web api của everrise, everrise lấy token ủy quyền. Adrepo không lưu token của khách hàng.

- S3 của adrepo được dùng để communicate với ETL (queue, data trả về)
- S3 của etl only used for back up, logs

- adrepo tạo queue -> batch export (của adrepo) đổ lên S3
- ETL lên S3 kiểm tra, batch import (của adrepo) vào table queue (của adrepo)
- cả 2 bên adrepo & everrise đều có batch import và export của riêng mình

Hay nói miệng là "import danh sách queue".

2 batch get data (report & master) chạy xong -> đẩy data lên S3 của adrepo, update table `queue` của mình. Sau đó batch export của mình update queue table lên S3 cho adrepo cập nhật. Mục đích là keep the two queue tables của hai bên đồng nhất với nhau.

## Facts

Giờ Nhật Bản (`JST` - Japan Standard Time) nhanh hơn giờ Việt Nam (`ICT` - Indochina Time) đúng 2 tiếng. Khi ở Việt Nam là 10:00 sáng, thì tại Nhật Bản đã là 12:00 trưa.

Database dùng MySQL

Sợ 2 bạn quên nên nhắc lại 1 lần cho chắc nha
Với source code của công ty  
Không push lên Respository cá nhân (Github, Bitbucket,....)  
Nếu được phép push thì phải setting Private, không được để public  
Tất cả thông tin về dự án (đặc biệt là thông tin tài khỏan, thông tin chứng thực,....) không được sử dụng cho mục đích cá nhân  
Nếu có tạo Repository cho mục đích học tập cá nhân thì không sử dụng tên công ty, tên dự án của công ty, dù là đặc tên cho dự án, cho thư mục, tên file gì đó

## Tạo Task

- làm không được thì báo cáo là không làm được => không báo cáo láo
- task thiếu thông tin, task làm ko được thì kiểm chứng => không làm
- chứng minh task này fraud, không cắm đầu làm task bị fraud
- đọc task ko hiểu thì confirm lại clear task rồi mới làm

- Title:
  - batch or webAPI?
  - Platform?
  - Report data or master data?
  - mục đích củ task (short, concise)

Goal

- Type:
  - feature: add new feature mới
  - refactor: chỉnh sửa code cũ nhưng không thay đổi output của code
  - task

- Không để trống description
  - current problem
  - expected solution

`end date` cập nhật theo tiến độ thực hiện nhưng mỗi lần edit date thì phải báo người quản lý, tránh bị động.

viết báo cáo & báo cáo tiến độ => very important

add child issues để chia nhỏ một task/batch lớn, một child ticket làm một chức năng => smaller pull requests are better than a big big PR

- output task:
  - task điều tra: use bullets (gạch đầu dòng)
  - task code: pull request + screenshots, log file,

## Batch Processing

- Don't confuse batch processing vs stream processing.
  - Batch processing wait for data to accumulate. Tới một mốc thời gian thì xử lý một lượt.
  - Stream thì one by one, vừa có cái mới là xử lý ngay lập tức, không chờ.

Each time you start a batch job, a batch processor instance picks up the job and processes it.

When a batch processor job begins, its batch processor instance is locked until the process is completed or terminated. A **lock file** (`<instanceName>.lock`)) is created in the $home folder. When the batch job is stopped or completed, the lock file is deleted. If the instance crashes, then the lock file remains in the $home folder, but it will not prevent the same batch instance from being started.

## Terminologies

PF: platform

- Dự án này có 2 phần:
  1. ETL Batch
  2. ETL Harbest Web API

creative (N.): video, hình ảnh của một quảng cáo

- một ad có nhiều video or images, mỗi video/hình ảnh có một `creative id` định danh. Nhưng sẽ có chung một `ad id`
- creative name là tên của tấm hình/ video

- master data là field liên quan tới business, report, khách hàng quan tâm
- metadata là field technical nerd, khách hàng không quan tâm

cpa, cpg???

campaign type: Upgraded Smar+, Manual, Smart+

- Tik tok ad has three levels:
  - Campaign level
  - Ad group level: quyết định Quảng cáo sẽ hiển thị ở đâu và cho ai. Một Chiến dịch có thể chứa nhiều Nhóm quảng cáo.
  - ad / creative level: Video, hình ảnh, văn bản quảng cáo (caption), và nút kêu gọi hành động (CTA).

- criteo: cty quảng cáo của france
- AdEbis: Marketing consultant in Japan

master data = metadata

`dspType` là tên của các platform (twitter, facebook)

- Có hai loại báo cáo: `master data` & `report data`
- Mỗi một format báo cáo là một loại `report_type`

- `report data`: kết quả quản cáo, score, thành tích, metric quảng cáo (view, click)
- `master data`: metadata, khách hàng không quan tâm, id quản cáo, type quản cáo
  - master ko chứa metric, report data chứa metric và id để join tới master

redshift là data warehouse

queue = yêu cầu tạo report data & master data cho quảng cáo

- 1 agency (tài khoản đăng nhập) -> nhiều parent account
- 1 parent account -> only 1 platform & token correspond to that platform

- Mỗi một tài khoảng quảng cáo cha (parent account) có 1 oauth id & token riêng của nó
- parent account (tài khoản quản lý cha) = `m input platform oauth` = `dsp_account`
- oauth id = tài khoản parent
- Tài khoản cha không tạo quảng cáo

- Mỗi parent account có thể tạo nhiều tk quản lý con = `advertiser` = `ad account`. Đây chính là đối tượng có thành tích quảng cáo, chạy quảng cáo.

- Don't confuse
  - `Account ID`: Adrepo User ID
  - `ad account ID`
- They are different!

### Trivia

Etl harbest dùng sbt (scala build tool) ko dùng maven ?

## Resources

[Catchup Outline](https://ever-rise.backlog.jp/alias/wiki/566675)

[Release Instruction](https://ever-rise.backlog.jp/alias/wiki/413979#loom-header-2)

