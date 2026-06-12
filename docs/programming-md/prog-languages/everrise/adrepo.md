# Adrepo Notes

## Current Notes

kkk

## Questions

Account_id của `get master queue` là account gì, khác gì oauth id?  
etl agency id?

queue_type 1, queue_type 3 là gì => coi trong class `batch/entity/etlAdrepo/AbstractGetReportQueue`

get_master_queue => cột `type` là gì?

---

- Lấy danh sách agency `/mAgencies/`
- API token trong header là của ai?
- table m_api_token chứa cái gì, khác gì table m_input_platform_auth?
- token_key vs. token_secret?
  
### Task điều tra

ETL-PRD-PROCESS => `PRD` là production?

Tại sao có 2 EC2 chạy batch: process, selenium (imobile)?

table `etl_adrepo.input_adrepo_last_exported_update_time` => dùng để biết queue nào đã được update để mà cập nhật cho phía adrepo (S3)

- `etl_harbest.m_input_platform`
- `m_input_platform_auth`

---

gradle dùng để làm gì

- table m_api_token
  - token_key vs. token_secret?

- Lấy danh sách agency `/mAgencies/`
  - API token trong header là của ai?

## Project ETL Basics

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

Có 2 bên: Adrepo và ETL. ETL chính là bên cty của mình. ETL bao gồm web API & Batch

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
  1. get advertiser list (get ad account, get advertiser master): đối tượng là `oauth id`, cho từng platform
  2. get master data: đối tượng là advertiser id
  3. get report data: đối tượng là advertiser id

- Có 2 level:
  1. Oauth id (tài khoản quản lý, tài khoản cha, `m_input_platform_auth`): dùng đăng nhập vào platform, có token chứng thực của platform đó, oauth id không chạy quảng cáo; chỉ oauth id mới có token, advertiser id không có token, dùng token của oauth id
  2. advertiser id (`ad account`): 1 queue lấy cho 1 advertiser id, là đối tượng chạy quảng cáo

1 user (user của adrepo) có thể tạo nhiều oauth id (tài khoản cha) để chạy quảng cáo trên nhiều platform

- Có hai loại báo cáo: `master data` & `report data`
- Mỗi một format báo cáo là một loại `report_type`

- `report data`: kết quả quản cáo, score, thành tích, metric quảng cáo (view, click)
- `master data`: metadata, khách hàng không quan tâm, id quản cáo, type quản cáo
  - master ko chứa metric, report data chứa metric và id để join tới master

redshift là data warehouse

queue = yêu cầu tạo report data & master data cho quảng cáo

- 1 agency (tài khoản đăng nhập) -> nhiều `parent account`.
- 1 parent account -> only 1 platform & token correspond to that platform

- Mỗi một tài khoảng quảng cáo cha (`parent_account`) có 1 oauth id & token riêng của nó
- parent account (tài khoản quản lý cha) = `m_input_platform_oauth` = `dsp_account`
- oauth id = tài khoản parent
- Tài khoản cha không tạo quảng cáo

- Mỗi parent account có thể tạo nhiều tk quản lý con = `advertiser` = `ad account`. Đây chính là đối tượng có thành tích quảng cáo, chạy quảng cáo.

- Don't confuse
  - `Account ID`: Adrepo User ID
  - `ad account ID`
- They are different!

- server id: `2` cái server chạy cái batch

## Facts

Giờ Nhật Bản (`JST` - Japan Standard Time) nhanh hơn giờ Việt Nam (`ICT` - Indochina Time) đúng 2 tiếng. Khi ở Việt Nam là 10:00 sáng, thì tại Nhật Bản đã là 12:00 trưa.

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

### Setup Batch

Database dùng MySQL

- build no profile thì phải copy file `config.properties` ra bên ngoài
- build có profile thì nó tự vào thư mục `dev` để lấy file `config`

chỉnh đường dẫn data base trong file `config.properties`. Copy file `config.properties` ra ngoài.

chỉnh bỏ dòng exclude entity

- repo fujiyama có 2 databases schema
  - etl adrepo chứa queue từ S3
  - etl harbest : làm với web api

chạy batch cần: file `config.property` & database

đăng nhập vào S3 lấy key trong file `config.property`

bảng `etl_adrepo.consts` phải có data

```bash
mvn install -Dmaven.test.skip=true
mvn -f pom\_batch.xml install
```

Thêm vào `pom.xml`

```xml
<plugin>
  <artifactId>maven-war-plugin</artifactId>
  <version>3.2.0</version>
  <configuration>
    <warSourceExcludes>WEB-INF/classes/**/.,WEB-INF/lib/*.jar</warSourceExcludes>
    <attachClasses>true</attachClasses>
    <classesClassifier>classes</classesClassifier>
  </configuration>
</plugin>

<dependency>
    <groupId>commons-codec</groupId>
    <artifactId>commons-codec</artifactId>
    <version>1.15</version>
</dependency>
```

Thêm vào `pom_batch.xml`

```xml
<dependency>
  <groupId>jp.co.everrise</groupId>
  <artifactId>ag</artifactId>
  <version>3.01.03</version>
  <classifier>classes</classifier>  <!-- Thêm dòng này -->
  <type>jar</type>                   <!-- Đổi từ war sang jar -->
</dependency>
```

lấy not process queue từ trong table thì `dsp_type` phải đúng

Spec & Versions:

dùng junit 4 (cuốn sách bản cũ second edition)

### Setup Web API

`sbt run`

`sbt -jvm-debug 9999 run`

### Passsword & config

mysql: root:root or 123

## ETL Batch

### Batch Basics

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

lấy report data trả về csv, không phải json => nên không thể dùng jackson để mapping report data sang dto được mà phải làm cách cực nhọc hơn

tìm queue trong table -> xử lý queue, status 2 sucess, show log, upload lên s3

adrepo call web api của everrise, everrise lấy token ủy quyền. Adrepo không lưu token của khách hàng.

- S3 của adrepo được dùng để communicate với ETL (queue, data trả về)
- S3 của etl only used for back up, logs

- adrepo tạo queue -> batch export (của adrepo) đổ lên S3
- ETL lên S3 kiểm tra, batch import (của adrepo) vào table queue (của adrepo)
- cả 2 bên adrepo & everrise đều có batch import và export của riêng mình

Hay nói miệng là "import danh sách queue".

2 batch get data (report & master) chạy xong -> đẩy data lên S3 của adrepo, update table `queue` của mình. Sau đó batch export của mình update queue table lên S3 cho adrepo cập nhật. Mục đích là keep the two queue tables của hai bên đồng nhất với nhau.

`m_input_platform` => m là master

### `ERROR_TYPE`

error type của của queue lấy report data

- `0`, `NORMALLY_RESULT`, không tìm thấy auth token trong table
- `1`, `ERROR_CANNOT_HANDLE`, An error requiring further investigation on the ETL side; not call API and not use S3 yet
  - nói chính xác hơn là những lỗi liên quan tới logic xử lý nội bộ, setting thiếu...không liên quan đến việc call API
  - vậy nếu error type = 1 thì có thể kết luận 90% là chưa call api, lỗi nằm trước phần call api
- `2`, `ACCOUNT_INFO_ERROR`, authentication fail (đã call API)
- `3`, `DOWNLOAD_TIMEOUT`, This is usually a problem on the platform side and will resolve itself over time.
  - Retry 3-4 lần vẫn không thành công thì throw lỗi này
- `4`, `DOWNLOAD_UNKNOWN_ERROR`, An API error with an unknown cause. In principle, it will resolve itself over time. Đã call API (chứng thực thành công), lỗi, không cần retry vì chắc chắc vẫn sẽ fail
- `5`, `GENERATE_TSV_ERROR`, If an error occurs during the process of writing data acquired from the platform to a TSV file for S3 upload

error type = `null` nghĩa là queue chưa được chạy, chứ nếu đã chạy thì không thể là `null` được

api trả về bad request (request sai format) thì không cần retry => sẽ không gặp lỗi timeout

[error detail type](https://docs.google.com/spreadsheets/d/11HpIRsqNgSZkRr6mDMRQ8scQSDx76pSoyKUa4kdzQZ4/edit?gid=440079028#gid=440079028)

### Queue Status, Type

- Queue Status:
- 0, `NOT_PROCESS`
- 1, `DOWNLOADING`
- 2, `DOWNLOADED`
- 3, `INSERT_SCHEDULED`
- 4, `INSERTING`
- 5, `INSERTED`
- 8, `DOWNLOAD_ERROR`
- 9, `INSERT_ERROR`
- 10, `VALIDATION_ERROR`

- Queue type:
- `CREATED_BY_MANUALLY`, (0)
- `CREATED_BY_SCREEN`, (1)
- `CREATED_FOR_YESTERDAY`, (2)
- `CREATED_FOR_40_DAYS_AGO`, (3)
- `CREATED_FOR_SYNC_ETL_AND_ADEBIS`, (4)
- `CREATED_FOR_EXTERNAL_SYSTEM_TRANSFER`, (5)

- Condition Mode:
- `ALL` (0)
- `INCLUDE` (1)
- `EXCLUDE` (2)

### Batch Export queue

Batch export queue (status) cho phía adrepo có input là một dsp_type

Vào `input_adrepo_last_exported_update_time` tìm last update time của dsp_type đó (master 1 dòng, report 1 dòng). Ở đây gọi là $A_1, A_2$.

Vào table queue_master và queue_report của platform tương ứng, lấy tất cả queue có updated time $\geq A_1, \geq A_2$

column `queue_id` trong table `input_adrepo_last_exported_update_time` là id của queue có update time gần nhất trong table get_queue & get_report. Mục đích của column này là để có thể sử dụng điều kiện $\geq$ ở bước trên nhưng không sợ lấy duplicate queue

sử dụng time trong `update_micro_time` có micro-second, time trong `updated_at` chỉ chính xác tới second sẽ sinh ra nhiều queue có thời gian giống nhau tới từng giây.

- `input_etl` phía adrepo xuất cho mình
- `input_adrepo` phía mình xuất cho adrepo
- Trong mỗi directory kể trên lại chia ra làm 3 sub-directories:
  - unprocessed: queue phía etl chưa attempt to import into their tables
  - completed: import into table của etl thành công
  - failured: cannot import into table của etl (ko import được vào table thì sẽ không bàn tới chuyện queue status thành công hay thất bại)

### Batch GetAdvertiserList

đối tượng: agency_id list (account khách hàng của adrepo)

### Other Batches

BatchInitializationGetReportQueueError

Class `AbstractBatchMaster` không liên quan đến việc lấy master. It contains logic to parse command-line arguments passed into the batches. It is inherited by batch get master data & batch get advertiser list.

Class `AbstractBatch` contains the Logger objects and the methods to print logs.

### Batch Processing

- Don't confuse batch processing vs stream processing.
  - Batch processing wait for data to accumulate. Tới một mốc thời gian thì xử lý một lượt.
  - Stream thì one by one, vừa có cái mới là xử lý ngay lập tức, không chờ.

Each time you start a batch job, a batch processor instance picks up the job and processes it.

When a batch processor job begins, its batch processor instance is locked until the process is completed or terminated. A **lock file** (`<instanceName>.lock`)) is created in the $home folder. When the batch job is stopped or completed, the lock file is deleted. If the instance crashes, then the lock file remains in the $home folder, but it will not prevent the same batch instance from being started.

- lock file được tạo dựa trên name của batch nên mỗi thời điểm chỉ có duy nhất một batch tên `BatchGetMasterTiktok` chạy được thôi.
- nếu lock file exit thì batch không chạy, batch chạy xong thì delete lock file

## ETL Web API

Play framework uses `Google Guice` as its default dependency injection (DI) framework.

- Khi request gởi tới, `Guice` will `new` a controller object & inject all dependencies mà controller object cần. Sau đó `Guice` hands over the controller object for the Play Framework. 
- Before executing the controller's logic, Play looks at the `.index()` method and sees the `@Security.Authenticated(LoginAuthenticator.class)` annotation sitting right above it.
  - Instead of calling `.index()` immediately, Play pauses the request and calls the `LoginAuthenticator` first.
- If the authenticator fails (for example, if the tokens are missing or invalid), it blocks the request and immediately returns an `error.unauthorized` response. In this scenario, Play never calls `.index()`.
- If the authenticator succeeds, then Play finally triggers the `.index()` method to bind the form data, query the database, and return the `MAgency` list.

## Log4j

Apache Log4j version `1.2`; [documentation](https://logging.apache.org/log4j/1.x/)

In Log4j, a `category` (also commonly referred to as a "logger") is essentially the named channel or routing rule that your Java code uses to send messages.

When you write `log4j.category.<Name> = <LEVEL>, <AppenderAlias>`, you are defining three things:

1. The Name: The string your Java code uses to find this rule (e.g., "AdwordInfoLogger" or `org.seasar`)
2. The Level: The minimum severity of messages this channel will accept (e.g., `INFO`, `ERROR`, `DEBUG`)
3. The Appender Alias: A short, custom label linking this category to a specific destination (e.g., `ADIA` or `C`)

An appender defines the physical destination where the log messages will actually be written, as well as how they should be formatted. In your configuration, appenders are defined using `log4j.appender.<AppenderAlias> = <AppenderClass>`.

```txt
# 1. THE CATEGORY: Creates a channel named "AdwordInfoLogger", accepts "INFO" level messages and above, and routes them to an appender nicknamed "ADIA".
log4j.category.AdwordInfoLogger=INFO,ADIA

# 2. THE APPENDER TYPE: Defines "ADIA" as a file writer that rolls over daily.
log4j.appender.ADIA=org.apache.log4j.DailyRollingFileAppender
log4j.appender.ADIA.encoding=UTF-8
log4j.appender.ADIA.DatePattern='.'yyyy-MM-dd
log4j.appender.ADIA.Append=true

# 3. THE APPENDER DESTINATION: Tells "ADIA" exactly where to save the file on your server.
log4j.appender.ADIA.File=/opt/ag/logs/adword/adword-info.log

# 4. THE APPENDER FORMATTING: Defines the specific layout pattern (Date, Thread, Message) for the text inside the file.
log4j.appender.ADIA.layout=org.apache.log4j.PatternLayout
log4j.appender.ADIA.layout.ConversionPattern=%-5p %d [%t] %m%n
```

In my project, there is only two levels `INFO` & `ERROR`, không có level `DEBUG`.

Class `LogUtils` is a custom class. We only use methods `error()` & `info()`. We do not use the other methods.

## Others Terms

app id: id của phía ETL adrepo (bên mình) khi làm việc với platform để xin token

`etl_harbest.m_agency` của harbest chứa gì => agency = account khách hàng của adrepo

### UTM parameters

Marketers use TML to figure out: "Where did my user come from?", how did people found out about their website?

The marketing team of the company adds utm params to the links leading to their website.

- brand name of the traffic source: youtube, twitter, tiktok, facebook, instagram, etc
- medium (traffic type):
  - organic traffic: coming from a search engine (`google/organic`, `bing/organic`)
  - referral: coming from another website (`facebook/referral`)
  - none: "I'm not sure how they got here" (`(direct)/(none`))
  - `cpc` or `ppc`
- Campaign (purpose of the traffic)

### Google Analytics

`Paid Ads` nghĩa là bạn phải trả tiền để có được lượt hiển thị hoặc truy cập. Còn việc tính tiền thế nào có vài cách khác nhau.

- CPC (Cost Per Click) - Tính tiền theo Lượt Nhấp: chỉ khi nào người dùng click vào link dẫn đến website/app của bạn thì bạn mới bị trừ tiền.
- CPM (Cost Per Mille) - Tính tiền theo Lượt Hiển Thị: cứ quảng cáo đập vào mắt 1,000 người (không cần biết họ có click hay không), bạn sẽ phải trả một khoản tiền cố định.
- CPA / oCPM (Cost Per Action / Conversion) - Tính tiền theo Lượt Chuyển Đổi: Bạn đặt mục tiêu là: "Tôi chỉ muốn trả tiền khi có người Mua hàng hoặc Tải app thành công".

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

`Upgraded Smart+`: một campaign

An `ad` is the smallest advertising unit and is the content presented to the target audience.

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
