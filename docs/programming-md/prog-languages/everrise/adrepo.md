# Adrepo Notes

## Current Task

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

- table m_api_token
  - token_key vs. token_secret?

- Lấy danh sách agency `/mAgencies/`
  - API token trong header là của ai?

## Project Specification, Thiết kế

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

Mình đang lấy dữ liệu report theo Asynchronous (tiktok, twitter)

---

app id: id của phía ETL adrepo (bên mình) khi làm việc với platform để xin token

`etl_harbest.m_agency` của harbest chứa gì => agency = account khách hàng của adrepo

Một `Ad ID` có thể chứa một `Creative ID`, nhưng một `Creative ID` (video gốc) có thể được dùng lại trong nhiều chiến dịch khác nhau.

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

khi chạy script sql để tạo database & insert data trong `DBeaver` thì phải right-click -> `execute script`

When you highlight multiple queries in DBeaver and hit Ctrl + Enter (which is "Execute SQL Statement"), DBeaver behaves in a very specific way: It ignores the highlighted boundaries and tries to merge both of your separate `INSERT` statements into one single, massive query before sending it to MySQL. MySQL then sees two `INSERT INTO` headers in a single query and throws the syntax error `1064`.  
To run both highlighted statements together successfully, you must use DBeaver's Script Execution system instead of the "Statement" execution.

---

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

hoặc phải bấm nút sync project trong tool intelliJ

lấy not process queue từ trong table thì `dsp_type` phải đúng

Spec & Versions:

dùng junit 4 (cuốn sách bản cũ second edition)

### Setup Web API

```bash
sbt run
# or
sbt -jvm-debug 9999 run
```

Để ứng dụng hoạt động thì cần lưu file application.conf theo đường dẫn: `\opt\web-api-conf\application.conf`

- Playframwork 2.6.9
  - [play framwork doc](https://www.playframework.com/documentation/2.5.8/JavaForms) (2.1.x or 2.6)
- Velocity version 1.7; Apache Velocity Template Language (VTL)

- window command promt, power shell will run the `sbt.bat` before the `sbt` in your path environment. It checks your current directory (.) first before looking anywhere else.
- Muốn force chạy `sbt` trong path, không chạy `sbt.bat` thì phải làm vài chiêu trò => poor design

- git bash, unix environment will run `sbt` inside your path. The current directory is completely ignored by default when resolving commands. This is a fundamental security feature designed to prevent malicious scripts from hijacking standard commands (like creating a fake `ls` script in a folder).
- Use `./sbt` to force use the local

Sửa trong `conf/application.conf`

```text
## JDBC Datasource
db {
  # You can declare as many datasources as you want.
  # By convention, the default datasource is named `default`
  default {
    driver = com.mysql.jdbc.Driver
    url = "jdbc:mysql://localhost:3306/etl_harbest?useUnicode=true&characterEncoding=utf-8&autoReconnect=true&useSSL=false"
    # url = "jdbc:mysql://adrepo-etl-dev.cmc5p7xnpwhi.ap-northeast-1.rds.amazonaws.com/etl_harbest?useUnicode=true&characterEncoding=utf-8&autoReconnect=true&useSSL=false"
    # (staging) url = "jdbc:mysql://adrepo-etl-staging.cmc5p7xnpwhi.ap-northeast-1.rds.amazonaws.com/etl_harbest?useUnicode=true&characterEncoding=utf-8&autoReconnect=true&useSSL=false"
    # (production) url = "jdbc:mysql://adrepo-etl.cmc5p7xnpwhi.ap-northeast-1.rds.amazonaws.com/etl_harbest?useUnicode=true&characterEncoding=utf-8&autoReconnect=true&useSSL=false"
    username = "root"
    password = "root"
    # username = "adrepo"
    # password = "password"
    # (production) password = "4y5NKV79QUz3D1UnLN"
    logSql = true
    autocommit = true
  }
}

```

---

3 tables sau phải có dữ liệu: `m_api_token`, `m_login_user`, `m_contract_company`

#### Setup test Web API

Read this wiki <https://ever-rise.backlog.jp/alias/wiki/557867>

add to `conf/AwsCredentials.properties`

```txt
accessKey=hoge
secretKey=fuga
```

### Passsword & config

mysql: root:root or 123

### AWS Deployment

- `/opt/ag/ag_batch/` chứa file `.jar` chạy batch, chứa `config.properties`, chứa file lock, chứa các file config khác như email
- build file jar copy lên `~` của `ec2-user`.  Rồi từ đó chạy `/home/ec2-user/deploy_batch.sh` copy file jar qua bên `/opt/ag/ag_batch/`

```bash
[ec2-user@ip-172-31-22-123 ag_batch]$ pwd
/opt/ag/ag_batch
[ec2-user@ip-172-31-22-123 ag_batch]$ ls
adrepo_batch.jar                                         alert_master_queue_state_receiver.txt  file_download    report_data
alert_import_daily_unique_advertiser_list_mail_to.txt    alert_report_queue_state_receiver.txt  file_upload      tmp
alert_import_daily_unique_advertiser_list_mail_to.txt~   backup_cron                            process_watcher
alert_import_queue_to_adrepo_etl_rds_failed_mail_to.txt  config.properties                      report
```

---

Chỉ có thể truy cập EC2 release từ ip của con forwarding. Có 2 con EC2 forwarding

```bash
1. ETL-STG-PROCESS
// Firstly, connect to forwarding server
ssh -i ~/.ssh/adrepo-test20200330.pem ec2-user@52.192.196.227
// At forwarding server, connect to ETL-STG-PROCESS
ssh -i ~/.ssh/etl-dev-process.pem ec2-user@54.249.169.232

// connect to RDS database
2. // Firstly, connect to forwarding server
ssh -i ~/.ssh/adrepo-test20200330.pem ec2-user@52.192.196.227
// At forwarding server, connect to RDS adrepo-etl-staging
mysql -hadrepo-etl-dev.cmcxxxxxxxxx.ap-northeast-1.rds.amazonaws.com -uadrepo -pYN5kmZWDwxxxxxxxxx
```

có 2 cái forwarding

---

Copy `config.properties` lên `/opt/ag/ag_batch/` & cấp quyền cho `adrepo-batch` và `ubuntu` (user chạy batch `java` trên selenium).

web api chỉ có 1 con staging, batch thì có 2 máy (dev, staging)

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

### Constants, Catalogs

`ERROR_TYPE` của của queue lấy report data

- `0`, `NORMALLY_RESULT`, không tìm thấy auth token trong table
- `1`, `ERROR_CANNOT_HANDLE`, An error requiring further investigation on the ETL side; not call API and not use S3 yet
  - nói chính xác hơn là những lỗi liên quan tới logic xử lý nội bộ, setting thiếu...không liên quan đến việc call API
  - vậy nếu error type = 1 thì có thể kết luận 90% là chưa call api, lỗi nằm trước phần call api
- `2`, `ACCOUNT_INFO_ERROR`, authentication fail (đã call API)
- `3`, `DOWNLOAD_TIMEOUT`, This is usually a problem on the platform side and will resolve itself over time.
  - Retry 3-4 lần vẫn không thành công thì throw lỗi này
- `4`, `DOWNLOAD_UNKNOWN_ERROR`, An API error with an unknown cause. In principle, it will resolve itself over time. Đã call API (chứng thực thành công), lỗi, không cần retry vì chắc chắc vẫn sẽ fail
- `5`, `GENERATE_TSV_ERROR`, If an error occurs during the process of writing data acquired from the platform to a TSV file for S3 upload
- `INSERT_ERROR` (6)

error type = `null` nghĩa là queue chưa được chạy, chứ nếu đã chạy thì không thể là `null` được

api trả về bad request (request sai format) thì không cần retry => sẽ không gặp lỗi timeout

[error detail type](https://docs.google.com/spreadsheets/d/11HpIRsqNgSZkRr6mDMRQ8scQSDx76pSoyKUa4kdzQZ4/edit?gid=440079028#gid=440079028)

---

Queue Status, Type

- Report Queue Status (`entity/etlAdrepo/AbstractGetReportQueue.java`):
  - 0, `NOT_PROCESS`
  - 1, `DOWNLOADING`
  - 2, `DOWNLOADED`
  - 3, `INSERT_SCHEDULED`
  - 4, `INSERTING`
  - 5, `INSERTED`
  - 8, `DOWNLOAD_ERROR`
  - 9, `INSERT_ERROR`
  - 10, `VALIDATION_ERROR`
- Report Queue type:
  - `CREATED_BY_MANUALLY`, (0)
  - `CREATED_BY_SCREEN`, (1)
  - `CREATED_FOR_YESTERDAY`, (2)
  - `CREATED_FOR_40_DAYS_AGO`, (3)
  - `CREATED_FOR_SYNC_ETL_AND_ADEBIS`, (4)
  - `CREATED_FOR_EXTERNAL_SYSTEM_TRANSFER`, (5)

---

- Master Queue Status (`entity/etlAdrepo/GetMasterQueue.java`):
  - 0, `NOT_PROCESS`
  - `DOWNLOADING` (1)
  - `DOWNLOADED` (2)
  - `DOWNLOAD_ERROR` (7)
- Master Queue type (`entity/etlAdrepo/GetMasterQueue.java`)
  - `CREATED_BY_MANUALLY`, (0)
  - `CREATED_BY_BATCH`, (1)
  - `CREATED_BY_SCREEN`, (2)
  - `CREATED_FOR_SYNC_ETL_AND_ADEBIS`, (4)

---

Mỗi platform sẽ có `REPORT_TYPE` khác nhau, không giống nhau.

- File `catalog/DSP_TYPE.java`
- Column `m_input_platform_auth.input_platform_id` thì xem trong `catalog/M_INPUT_PLATFORM.java`.
- Platform nào có trong DSP_TYPE mà không có trong `M_INPUT_PLATFORM` thì tức là không còn support. Những platform trong `M_INPUT_PLATFORM` là còn đang support.

| Platform         | M_INPUT_PLATFORM | DSP_TYPE |
|------------------|------------------|----------|
| BYPASS           | 36               | 3        |
| FACEBOOK         | 2                | 10       |
| TWITTER_API      | 1                | 21       |
| CRITEO_REST      | 28               | 28       |
| NEND             | 37               | 18       |
| LINE             | 27               | 20       |
| OUTBRAIN         | 14               | 34       |
| IMOBILE          | 16               | 16       |
| GOOGLE_ANALYTICS | 3                | ??       |
| GOOGLE_ADWORDS   | 17               | ??       |
| DBM              | 18               | 11       |
| LOGICAD          | 19               | 19       |
| FREAK_OUT        | 8                | 8        |
| SCALE_OUT        | 9                | 9        |

### Batch GetMaster

`BatchGetMasterFreakOut`

upload lên S3 tại `adrepo-development/master/{agency id}/{etl input platform auth id}/{master type}/csg.gz`

### Batch DownloadReport

`BatchDownloadReportTwitterAPI`

upload lên S3 tại `adrepo-development/report_data/{agency id}/{report type id}/csg.gz`

### Batch Export Import Queue

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

- Khi test `BatchExportEtlMasterQueueToAdrepo`
- Copy một dòng trong `get_master_queue` vào `input_adrepo_last_exported_update_time`
- Sau đó vào edit cái row vừa copy trong table `get_master_queue.update_micro_time` tăng thời gian lên
- Upload file lên s3 `adrepo-development/etl_fetch_data_queue`

---

`BatchImportAdrepoMasterQueueToEtl`

import từ `adrepo-development/etl`

[file design](https://docs.google.com/spreadsheets/d/1rtNHZ6TDrQqjUVwkYF7y5VLVKWIY4B4_2GIS-Hr8QSU/edit?gid=765059006#gid=765059006)

### Batch GetAdvertiserList

đối tượng: agency_id list (account khách hàng của adrepo)

`BatchGetAdvertiserListFreakOut`

- for each agency => lấy `mInputPlatformAuths` theo agency id & dsp_type
- for each `mInputPlatformAuths` => get advertiser list
- export file to S3 `adrepo-development/master/harbest/{agency_id}/{input_platform_id}/advertiser/`

Trong `.run()` của batch chính có export status lên S3 `adrepo-development/master/get_advertiser_master_status/Freakout/` (vậy batch này update lên 2 chỗ trên S3)

```java
            batchGetAdvertiserListFreakOut.exportStartedStateFileToS3();
            batchGetAdvertiserListFreakOut.run();
            batchGetAdvertiserListFreakOut.exportCompletedStateFileToS3();
```

### Other Batches

`BatchInitializationGetReportQueueError`

Class `AbstractBatchMaster` không liên quan đến việc lấy master. It contains logic to parse command-line arguments passed into the batches. It is inherited by batch get master data & batch get advertiser list.

Class `AbstractBatch` contains the Logger objects and the methods to print logs.

`BatchRemoveTemporaryFile` cho chạy start & finish

`BatchBackupEtlRds` chạy sau cùng vì sẽ reset database

`BatchAlertGetReportQueueState`, `BatchAlertGetMasterQueueState` => gởi email trong file `/opt/ag/ag_batch/alert_report_queue_state_receiver.txt`

### Batch Processing

- Don't confuse batch processing vs stream processing.
  - Batch processing wait for data to accumulate. Tới một mốc thời gian thì xử lý một lượt.
  - Stream thì one by one, vừa có cái mới là xử lý ngay lập tức, không chờ.

Each time you start a batch job, a batch processor instance picks up the job and processes it.

When a batch processor job begins, its batch processor instance is locked until the process is completed or terminated. A **lock file** (`<instanceName>.lock`)) is created in the $home folder. When the batch job is stopped or completed, the lock file is deleted. If the instance crashes, then the lock file remains in the $home folder, but it will not prevent the same batch instance from being started.

- lock file được tạo dựa trên name của batch nên mỗi thời điểm chỉ có duy nhất một batch tên `BatchGetMasterTiktok` chạy được thôi.
- nếu lock file exit thì batch không chạy, batch chạy xong thì delete lock file

### Entity

`AbstractEntity` contains fields: `deleted`, `createdBy`, `createdAt`, `updateBy`

Inside entity classes, we can have ENUM classes.

`GetMasterQueue` contain no fields, only ENUM and methods. The fields is inside `AbstractGetMasterQueue`

## ETL Web API

Play framework uses `Google Guice` as its default dependency injection (DI) framework.

- Khi request gởi tới, `Guice` will `new` a controller object & inject all dependencies mà controller object cần. Sau đó `Guice` hands over the controller object for the Play Framework.
- Before executing the controller's logic, Play looks at the `.index()` method and sees the `@Security.Authenticated(LoginAuthenticator.class)` annotation sitting right above it.
  - Instead of calling `.index()` immediately, Play pauses the request and calls the `LoginAuthenticator` first.
- If the authenticator fails (for example, if the tokens are missing or invalid), it blocks the request and immediately returns an `error.unauthorized` response. In this scenario, Play never calls `.index()`.
- If the authenticator succeeds, then Play finally triggers the `.index()` method to bind the form data, query the database, and return the `MAgency` list.

Hàm ''generateBaseJson()'' dùng ''base.vm''

---

- web api chỉ có 2 chức năng chính:
  1. đăng ký POST (và edit `PUT`) thông tin chứng thực cho tài khoảng cha
  2. lấy danh sách ad account của tài khoảng cha

- batch cũng có chạy mỗi ngày lấy danh sách ad account của tài khoản cha cập nhật lên s3 giống y chang bên web api
- chủ yếu để cập nhật danh sách ad account luôn mới nhất

### Controller & Form

`Forms` are responsible for capturing, filtering, and validating raw data coming from HTTP requests (the "outside world") before it touches your core application logic.

Forms contain specific validation logic to ensure user input is safe and correct.

You use form `Factory.form` to manually command the play framework to map HTTP data to a Java object.

Form Validation

### Authenticator

When a request hits a protected controller, the Play Framework automatically calls `BaseLoginAuthenticator.getUsername(Http.Context)`. This method in turn calls `LoginAuthenticator.getLoginUserId()`:

- Success (True): If the method returns any valid `String loginUserId`, the framework considers the user authenticated and allows the controller logic to proceed.
  - This `loginUserId` string is not used anywhere in the controller body. It is only for the authentication steps.
  - Việc lấy thông tin user đăng nhập được thực hiện với service trong controller. Lấy ra từ `Http.Context.current()`.
- Failure (False): If the method returns `null` (because the user ID is missing or the role check fails), the framework considers the authentication failed.

if `getLoginUserId()` inside `getUsername(ctx)` returns `null`, the Play Framework intercepts that `null` and automatically triggers `BaseAuthenticator.onUnauthorized(Http.Context)`. This method then returns your standard `401 Unauthorized` JSON response and completely halts the request.

### Velocity Template Engine

The `BaseController` uses the Apache Velocity engine (specifically the `base.vm` template) to format and generate the foundational JSON structure for all of your API responses.

Template engines like Apache Velocity are most commonly known for generating dynamic HTML for web pages. However, fundamentally, a template engine is just a text-generation tool. It merges dynamic data into a static blueprint to output plain text—which means it can just as easily generate XML, SQL, or, in the case of your API, JSON.

`base.vm` acts as the global, foundational wrapper for every single response your API sends back to the client (who sent request).

### Ebean ORM

kkk

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

## Common Terms

`Paid Ads` nghĩa là bạn phải trả tiền để có được lượt hiển thị hoặc truy cập. Còn việc tính tiền thế nào có vài cách khác nhau.

- CPC (Cost Per Click) - Tính tiền theo Lượt Nhấp: chỉ khi nào người dùng click vào link dẫn đến website/app của bạn thì bạn mới bị trừ tiền.
- CPM (Cost Per Mille) - Tính tiền theo Lượt Hiển Thị: cứ quảng cáo đập vào mắt 1,000 người (không cần biết họ có click hay không), bạn sẽ phải trả một khoản tiền cố định.
- CPA / oCPM (Cost Per Action / Conversion) - Tính tiền theo Lượt Chuyển Đổi: Bạn đặt mục tiêu là: "Tôi chỉ muốn trả tiền khi có người Mua hàng hoặc Tải app thành công".

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

## Tiktok

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

- `app_id`: An Application identifier used when making API calls. To get app_id, follow the instructions in Get Started - Create a developer app.
- `secret`: Each application has a unique secret key. The `app_id` and `secret` are obtained together when your application has been approved.

### Async Report

You can run a report in `synchronous mode` or `asynchronous mode` (mình dùng async).

With synchronous mode, you make an API request and the data will be returned in the response almost **instantly**. In `asynchronous mode`, you make an API request to create a task for getting the data. You need to wait some time for the task to complete. When the task is completed, you make **another API request** to download the data.

### Task Check field

Requests gởi tới API của tiktok để tạo task download report:

```json
{code}
=============  3501 =============
POST /open_api/v1.3/report/task/create/?data_level=AUCTION_AD&dimensions=["stat_time_day", "ad_id_v2"]&start_date=2026-06-24&end_date=2026-06-24&output_format=CSV_DOWNLOAD&advertiser_id=7126757427202080769&report_type=BASIC HTTP/1.1
Host: business-api.tiktok.com
Access-Token: dfa540f
Content-Type: application/json
Content-Length: 1918
{
"metrics": ["campaign_id","campaign_name","adgroup_id","adgroup_name","ad_name","spend","cpc","cpm","impressions","clicks","ctr","reach","cost_per_1000_reached","frequency","conversion","cost_per_conversion","conversion_rate","real_time_conversion","real_time_cost_per_conversion","real_time_conversion_rate","video_play_actions","video_watched_2s","video_watched_6s","video_views_p100","video_views_p75","video_views_p50","video_views_p25","average_video_play","average_video_play_per_user","follows","likes","comments","shares","profile_visits","profile_visits_rate","clicks_on_music_disc","real_time_app_install","app_install","registration","purchase","app_event_add_to_cart","checkout","view_content","next_day_open","add_payment_info","add_to_wishlist","launch_app","complete_tutorial","create_group","join_group","create_gamerole","spend_credits","achieve_level","unlock_achievement","sales_lead","in_app_ad_click","in_app_ad_impr","loan_apply","loan_credit","loan_disbursement","login","ratings","search","start_trial","subscribe","vta_conversion","cost_per_vta_app_install","vta_app_install","vta_registration","cost_per_vta_registration","vta_purchase","cost_per_vta_purchase","cta_conversion","cost_per_cta_app_install","cta_app_install","cta_registration","cost_per_cta_registration","cta_purchase","cost_per_cta_purchase","complete_payment_roas","complete_payment","page_browse_view","button_click","online_consult","user_registration","product_details_page_browse","web_event_add_to_cart","on_web_order","initiate_checkout","add_billing","page_event_search","form","download_start","on_web_add_to_wishlist","on_web_subscribe","onsite_shopping_roas","onsite_shopping","onsite_initiate_checkout_count","onsite_on_web_detail","onsite_add_to_wishlist","onsite_add_billing","onsite_on_web_cart","onsite_form","onsite_download_start","ix_page_view_count","ix_button_click_count","ix_product_click_count"]
}

=============  3502 =============
POST /open_api/v1.3/report/task/create/?data_level=AUCTION_AD&dimensions=["stat_time_day", "ad_id_v2", "gender", "age"]&start_date=2026-06-24&end_date=2026-06-24&output_format=CSV_DOWNLOAD&advertiser_id=7126757427202080769&report_type=AUDIENCE HTTP/1.1
Host: business-api.tiktok.com
Access-Token: dfa540f1
Content-Type: application/json
Content-Length: 271
{
"metrics": ["campaign_id","campaign_name","adgroup_id","adgroup_name","ad_name","spend","cpc","cpm","impressions","clicks","ctr","conversion","cost_per_conversion","conversion_rate","real_time_conversion","real_time_cost_per_conversion","real_time_conversion_rate"]
}

=============  3503 =============
POST /open_api/v1.3/report/task/create/?data_level=AUCTION_AD&dimensions=["stat_time_day", "ad_id_v2", "platform"]&start_date=2026-06-24&end_date=2026-06-24&output_format=CSV_DOWNLOAD&advertiser_id=7126757427202080769&report_type=AUDIENCE HTTP/1.1
Host: business-api.tiktok.com
Access-Token: dfa540f1370
Content-Type: application/json
Content-Length: 271
{
"metrics": ["campaign_id","campaign_name","adgroup_id","adgroup_name","ad_name","spend","cpc","cpm","impressions","clicks","ctr","conversion","cost_per_conversion","conversion_rate","real_time_conversion","real_time_cost_per_conversion","real_time_conversion_rate"]
}
```

### Tiktok Terms

- tiktok có 3 loại: manual, smart plus, upgraded smart plus
  - manual có 3 level: campaign, ad group, ad
  - smart plus & upgraded smart plus cũng có 3 level tương tự

---

Sub-Platforms

`TikTok for Business` is a global platform that provides products and solutions to help brands of all sizes drive business impact. Don't confuse with `Tiktok API for Business Developer`.

`TikTok Business Center` is a one-stop business hub that enables organizations to centralize assets management, permission allocation, also allows advertisers to manage multiple TikTok ad accounts among multiple users in a safe, efficient way.

- `tiktok for business developer` (`business-api.tiktok.com`, the Developer Engine):
  - What it is: The technical entry point (API) for software engineers and systems.
  - The Goal: Automating tasks programmatically by writing code to talk directly to TikTok’s servers.

- `ads.tiktok.com` (The Visual UI, `TikTok Ads Manager`)
  - It is the user interface designed for browser use.
  - The Goal: Creating and tracking ad campaigns manually by clicking buttons, uploading media via forms, and reading visual charts.
  - TikTok Ads Manager is an ad platform where advertisers can create and manage TikTok ad campaigns and ad creatives, view and analyze ad performance reports.
  - Ngoài ra, tiktok còn có các UI khác như: TikTok Business Center (`business.tiktok.com`), TikTok Shop Seller Center (`seller.tiktok.com`)

Almost everything you do with the `TikTok API for Business Developer` can be done manually using TikTok's Graphical User Interfaces (GUIs).  
The TikTok API for Business isn't a separate, secret advertising system. Instead, it is just a "programmatic bridge" that allows developers to write code to click buttons, upload files, and pull reports automatically rather than doing it by hand inside TikTok's web dashboards.

- TikTok API for Business is separated into the following APIs:
  - Marketing API: Interact with `TikTok Ads Manager` functionality at scale, allowing developers to programmatically query data, create and manage ads, and perform a wide variety of other tasks.

---

## Twitter

asynchronous report tạo `job`

The `synchronous` endpoint supports **short time ranges** and is ideal for real-time campaign optimizations. The asynchronous endpoints support much longer time ranges and are, thus, intended for fetching much more data, ideal for generating reporting or historical backfills.

- `User Account` (identified by `user_id` & `@username`)
  - This is a regular X account user for posting, normal usage. One or more X users can have access to an Ads Account.
  - To create an Ads Account, you use your regular X User Account. User account own ads_account
  - One X user can manage multiple `ads_accounts`.
  - The `OAuth Access Token` belongs to a specific `@username` (the X user who authorized the app). That token can access an `ads_account` if that `@username` has permission on the Ads Account.

- `Ads Account` (identified by `account_id` e.g. `18ce54d4x5t`)
  - This is the top-level advertising account. It holds campaigns, line items, funding instruments, creatives, audiences, etc. Most API endpoints use `:account_id` in the URL (e.g., `/stats/accounts/:account_id`).
  - An Ads Account can only be created by one X user account (your `@username`). That creator becomes the initial `Account Administrator`.
  - However, after creation, the Ads Account is not limited to just that one user. The owner (or any Account Administrator) can grant access to multiple other X user accounts (`@usernames`) with different permission level (e.g., Account Administrator, Ad Manager, Campaign Analyst, etc.) => One `ads_account` can be managed by multiple `@username`

- `Developer account` (identified by the same `user_id` & `@username`)
  - Your account on the X Developer Platform
  - Only used to create Apps & get API keys (Consumer Key etc.)
  - You need a regular X account to create a Developer Account.

- `User token level Rate Limits`: for OAuth access token; One token accessing many accounts shares quota
  - One token = one set of user-level quotas.
  - This is the default/fallback limit.
  - One token can have access to one or more `ad_accounts` because one `@username` can have access to multiple Ads Accounts.
  - An `OAuth User Access Token` is Granted by the User Account (`@username`) To the Developer App
  - The token can only access Ads Accounts that your `@username` has permission to.
- `ads_account level Rate Limits`
  - A subset of endpoints are enabled to use ad account level rate limiting.

Developers should utilize the `ad_account level rate limit` when it is returned in response headers and only utilize user level limit when the ad account limit is NOT found.

- In the X Ads API, `entity` refers to the type of object you want to fetch data for (especially in analytics/stats endpoints). An entity is any object in the Ads hierarchy that can have performance metrics associated with it.
- `Ads Account`: Top-level container,Your business account
- `Campaign`: High-level goal + overall budget
- `Line Item` (Ad Group): Targeting + Bidding + Budget
  - You can have many Line Items inside one Campaign (different targeting, bids, creatives).
  - X recommends pulling stats (metrics) at the Line Item level (`entity=LINE_ITEM`) because it's the most granular and useful level.
- `Promoted Tweet`: The actual ad creative,Specific Tweet being promoted
- `PROMOTED_ACCOUNT`

- Yes, an active entity (e.g. a Line Item, Promoted Tweet, Campaign, etc.) can have multiple placements.
- Khi get active entities nó return placement, khi tạo job lấy metric thì mình add parameter placement mình muốn

- `&placement=ALL` => returns impressions for this entity from ALL placements
- `&placement=ALL_ON_TWITTER` => returns only impressions from inside X

- For most analytics/stats endpoints → You can request up to 20 entity_ids per request.
- `PROMOTED_TWEET` is one of the most commonly batched entities.
- Instead of requesting 100 Promoted Tweets in one call (which would fail), you process them in groups of 20.
- Example: If you have 87 Promoted Tweets, you will make 5 jobs (4 jobs of 20 + 1 job of 7).

---

```json
{
      "data_type": "stats",
      "time_series_length": 24,
      "data": [
        {
          "id": "dvcz7",
          "id_data": [
            {
              "segment": null,
              "metrics": {
                "impressions": [
                  0,0,2792,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
                ],
                "engagements": [
                  0,0,60,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
                ],
                "video_total_views": [
                  0,0,1326,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
                ]
              }
            }
          ]
        }
      ],
      "request": {}
    }
```

The length of the array depends on the `granularity` you requested and the time range (`start_time` to `end_time`).

- The first number in the array = the first hour after your `start_time`.
- Each next number = the next hour.

Example (if you requested `start_time=2026-03-05T00:00:00Z` and `granularity=HOUR`):

- Index 0 → 2026-03-05 00:00 ~ 01:00 → 0 impressions
- Index 1 → 2026-03-05 01:00 ~ 02:00 → 0 impressions
- Index 2 → 2026-03-05 02:00 ~ 03:00 → 2792 impressions
- ... and so on.

- If you want daily totals → use `granularity=DAY`
- If you want overall total → use `granularity=TOTAL` (returns a single number instead of array)
- You can sum the array yourself to get the total: 2792 in this case.

## Other Platforms

k

## Other Project Rules

Dù hiện tại nhiều IDE vẫn hiểu (vẫn render được) khi sử dụng tag `<code>` nhưng theo a mới tìm hiểu thì từ giờ chúng ta sẽ chuyển sang format chuẩn sau đây.

Khi trong Javadoc có các ký tự : `<, >, &` thì nên bọc bằng `{@code ...}`  
Ví dụ: `@return {@code List<Object>}`

Ở một số thư viện/ngôn ngữ lớn còn 1 style khác là escape HTML  
ví dụ: `@return <code>List&lt;Object&gt;</code>`

nhưng kiểu này a thấy khó nhớ => không xài

## Resources

[Catchup Outline](https://ever-rise.backlog.jp/alias/wiki/566675)

[Release Instruction](https://ever-rise.backlog.jp/alias/wiki/413979#loom-header-2)

[tiktok marketing API](https://business-api.tiktok.com/portal/docs/marketing-api/v1.3)

[Setup for Windows 10](https://ever-rise.backlog.jp/alias/wiki/559246#loom-header-14)

[catchup outline](https://ever-rise.backlog.jp/alias/wiki/566675)

[build project](https://ever-rise.backlog.jp/alias/wiki/559969)

enctypted token tiktok: `DtxDD2ml1zIpmf3vNYy3b+DRWMdp0TVE7cUBT7nfPdDDplajKjyHNSLkYhQWWXxt`

