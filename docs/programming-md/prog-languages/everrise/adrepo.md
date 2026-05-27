# Adrepo Notes

## Current Notes

`co.everrise.batch.tiktok.BatchGetMasterTiktok`

`co.everrise.batch.catalog.REPORT_TYPE` enum

1 ad nhiều creative

## Questions

Account_id của `get master queue` là account gì, khác gì oauth id?  
etl agency id?

`TiktokRequestDto` là DTO nhưng lại chứa toàn là constant?

queue_type 1, queue_type 3 là gì => coi trong class `batch/entity/etlAdrepo/AbstractGetReportQueue`

google analytic for beginner?

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

ETL chỉ cung cấp data as is returned from API. ETL không biến đổi dữ liệu. Khách hàng muốn biến đổi thì tự họ làm, ETL không làm.  
rule dự án: api trả về cái gì thì trả cho khách hàng cái đó, không xào nấu thêm phức tạp

dùng junit 4 (cuốn sách bản cũ second edition)

## ETL Batch

This is a maven project and it has two parts: ag.war and adrepo_batch.jar. Vì vậy nên nó có 2 file `pom.xml`.

`ag` là code cũ, chạy web, không cần quan tâm

- If using profile. Please check resources in `src/main/resources/conf/${profile.resource.folder}`. Verify the following files:
  - `config.properties`
  - `config.dicon`: file cấu hình của Seasar2 Framework, dependency injection config => không quan tâm `ag` nên không quan tâm file này luôn
  - `log4j.properties`: thư viện `Log4j`, quyết định cách mà `LOG_INFO` và LOG_ERROR (mà bạn đã hỏi) sẽ hoạt động.

location jdk java 8: `C:\Users\anhao\.jdks\corretto-1.8.0_492`

- Khi lấy data có 3 thông tin cần quan tâm:
  1. đối tượng: oauth id, advertiser id
  2. data type (master data or report data or ad list)
  3. token chứng thực

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

## Tạo Task & báo cáo tiến độ

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

- Tạo ticket con thì dẫn link đến ticket cha và tên theo format `{id cha} {title cha} / {titple con}`
- Chỉ dẫn link ticket parent, không quan tâm cấp cao hơn (grand parent)

- Các mốc thời gian báo cáo hằng ngày
  - 08h30 sáng (sau khi họp sáng công ty, trước buổi họp vào lúc 09h00): công việc dự định thực hiện trong ngày, tiến độ hiện tại.
  - 15h00 chiều (trước buổi họp vào lúc 15h30): tiến độ hiện tại.
  - 17h00 chiều: tiến độ hiện tại.

## Build & Setup the Project

- build no profile thì phải copy file `config.properties` ra bên ngoài
- build có profile thì nó tự vào thư mục `dev` để lấy file `config`

chỉnh đường dẫn data base trong file `config.properties`. Copy file `config.properties` ra ngoài.

chỉnh bỏ dòng exclude entity

- repo fujiyama có 2 databases schema
  * etl adrepo chứa queue từ S3
  * etl harbest : làm với web api

chạy batch cần: file `config.property` & database

đăng nhập vào S3 lấy key trong file `config.property`

bảng `etl_adrepo.consts` phải có data

```
mvn install -Dmaven.test.skip=true
mvn -f pom\_batch.xml install
```

Thêm vào `pom.xml`

```
<dependency>
    <groupId>commons-codec</groupId>
    <artifactId>commons-codec</artifactId>
    <version>1.15</version>
</dependency>
```

lấy not process queue từ trong table thì `dsp_type` phải đúng

## Passsword

mysql: root:root or 123

## Batch Processing

- Don't confuse batch processing vs stream processing.
  - Batch processing wait for data to accumulate. Tới một mốc thời gian thì xử lý một lượt.
  - Stream thì one by one, vừa có cái mới là xử lý ngay lập tức, không chờ.

Each time you start a batch job, a batch processor instance picks up the job and processes it.

When a batch processor job begins, its batch processor instance is locked until the process is completed or terminated. A **lock file** (`<instanceName>.lock`)) is created in the $home folder. When the batch job is stopped or completed, the lock file is deleted. If the instance crashes, then the lock file remains in the $home folder, but it will not prevent the same batch instance from being started.

- lock file được tạo dựa trên name của batch nên mỗi thời điểm chỉ có duy nhất một batch tên `BatchGetMasterTiktok` chạy được thôi.
- nếu lock file exit thì batch không chạy, batch chạy xong thì delete lock file

## UTM parameters

Marketers use TML to figure out: "Where did my user come from?", how did people found out about their website?

The marketing team of the company adds utm params to the links leading to their website.

- brand name of the traffic source: youtube, twitter, tiktok, facebook, instagram, etc
- medium (traffic type):
  - organic traffic: coming from a search engine (`google/organic`, `bing/organic`)
  - referral: coming from another website (`facebook/referral`)
  - none: "I'm not sure how they got here" (`(direct)/(none`))
  - `cpc` or `ppc`
- Campaign (purpose of the traffic)

## Google Analytics

`Paid Ads` nghĩa là bạn phải trả tiền để có được lượt hiển thị hoặc truy cập. Còn việc tính tiền thế nào có vài cách khác nhau.

- CPC (Cost Per Click) - Tính tiền theo Lượt Nhấp: chỉ khi nào người dùng click vào link dẫn đến website/app của bạn thì bạn mới bị trừ tiền.
- CPM (Cost Per Mille) - Tính tiền theo Lượt Hiển Thị: cứ quảng cáo đập vào mắt 1,000 người (không cần biết họ có click hay không), bạn sẽ phải trả một khoản tiền cố định.
- CPA / oCPM (Cost Per Action / Conversion) - Tính tiền theo Lượt Chuyển Đổi: Bạn đặt mục tiêu là: "Tôi chỉ muốn trả tiền khi có người Mua hàng hoặc Tải app thành công".

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

- criteo: cty quảng cáo của france
- AdEbis: Marketing consultant in Japan

master data = metadata

- `dspType` là tên của các platform (twitter, facebook)
- `advertiser id`
- `oauth id` 

- Có 3 loại batch chính trong project:
  1. get advertiser list (get ad account, get advertiser master): đối tượng là `oauth id`
  2. get master data: đối tượng là advertiser id
  3. get report data: đối tượng là advertiser id

- Có 2 level:
  1. Oauth id (tài khoản quản lý): dùng đăng nhập vào platform, có token chứng thực của platform đó, oauth id không chạy quảng cáo; chỉ oauth id mới có token, advertiser id không có token, dùng token của oauth id
  2. advertiser id: 1 queue lấy cho 1 advertiser id, là đối tượng chạy quảng cáo

1 user có thể tạo nhiều oauth id để chạy quảng cáo trên nhiều platform

- Có hai loại báo cáo: `master data` & `report data`
- Mỗi một format báo cáo là một loại `report_type`

- `report data`: kết quả quản cáo, score, thành tích, metric quảng cáo (view, click)
- `master data`: metadata, khách hàng không quan tâm, id quản cáo, type quản cáo
  - master ko chứa metric, report data chứa metric và id để join tới master

redshift là data warehouse

queue = yêu cầu tạo report data & master data cho quảng cáo

- 1 agency (tài khoản đăng nhập) -> nhiều `parent account`.
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

- server id: `2` cái server chạy cái batch

### Tiktok

- Tik tok ad has four levels để có thể lấy report data:
  1. `Campaign level`
  2. `Ad group` level: quyết định Quảng cáo sẽ hiển thị ở đâu và cho ai. Một Chiến dịch có thể chứa nhiều Nhóm quảng cáo.
  3. `ad level`
  4. `creative level`: Video, hình ảnh, văn bản quảng cáo (caption), và nút kêu gọi hành động (CTA).
  
- `Creative` là toàn bộ những gì người dùng thực sự nhìn thấy và nghe thấy trên màn hình điện thoại của họ: video, ad text, CTA button, landing page. `Creative` là một "gói" (package) bao gồm tất cả các yếu tố hiển thị với người dùng, chứ không phải chỉ là cái file video riêng lẻ.

- Chế độ Thủ công (Standard Ad) truyền thống: 1 Ad = 1 Creative.
  - Khi bạn tạo một Quảng cáo (Ad), bạn chọn một tổ hợp duy nhất (1 Video + 1 Text + 1 CTA).
  - Nếu bạn muốn thử một Video khác hoặc một dòng Text khác, bạn phải tạo một Ad mới (`Ad ID` mới) nằm trong cùng một `Ad Group`.
  - Như vậy, trong một Ad Group có thể có nhiều Ad, mỗi Ad mang một Creative khác nhau.

- Chế độ Tự động (`Smart Creative`)
  - Khi bật tính năng Smart Creative, bạn không tạo từng Ad riêng lẻ. Thay vào đó, bạn tải lên "nguyên liệu" (Assets): Tải lên 5 Video khác nhau, viết 5 dòng Text khác nhau, chọn vài nút CTA.
  - Hệ thống TikTok sẽ tự động kết hợp ngẫu nhiên các thành phần này để tạo ra hàng chục biến thể Creative khác nhau (Ví dụ: Video 1 + Text 3, Video 2 + Text 1...).
  - Tất cả các biến thể này thường nằm chung dưới 1 Ad ID duy nhất của chiến dịch thông minh. Hệ thống sẽ tự tìm ra "tổ hợp thắng cuộc" (Winning Combination) để phân phối nhiều nhất. => In this case, one ad id has many creative id.

- Cấp độ `Ad Group`: Chắc chắn có nhiều Creative (thông qua việc tạo nhiều Ad).
- Cấp độ `Ad`: Chỉ có nhiều Creative nếu dùng tính năng Smart Creative/ACO.

`Advertiser access token` của tiktok does not expire.

With the `auth_code` you receive after authorization by the advertiser, you can make a request to the following endpoint to get an `access_token` for subsequent API requests.

The `auth_code` is valid for 1 hour and can be used only once. After the `auth_code` expires, you need to start over and perform the authorization steps again.

### Trivia

Etl harbest dùng sbt (scala build tool) ko dùng maven ?

Một `Ad ID` có thể chứa một `Creative ID`, nhưng một `Creative ID` (video gốc) có thể được dùng lại trong nhiều chiến dịch khác nhau.

- tiktok có 3 loại: manual, smart plus, upgraded smart plus
  - manual có 3 level: campaign, ad group, ad
  - smart plus & upgraded smart plus cũng có 3 level tương tự
## Resources

[Catchup Outline](https://ever-rise.backlog.jp/alias/wiki/566675)

[Release Instruction](https://ever-rise.backlog.jp/alias/wiki/413979#loom-header-2)

[tiktok marketing API](https://business-api.tiktok.com/portal/docs/marketing-api/v1.3)

[Setup for Windows 10](https://ever-rise.backlog.jp/alias/wiki/559246#loom-header-14)

[catchup outline](https://ever-rise.backlog.jp/alias/wiki/566675)

[build project](https://ever-rise.backlog.jp/alias/wiki/559969)