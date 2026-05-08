# Adrepo Notes

## Current Notes

`co.everrise.batch.tiktok.BatchGetMasterTiktok`

`co.everrise.batch.catalog.REPORT_TYPE` enum

## Questions

Ngoài level creative ra còn có level nào khác nữa

Module `co.everrise.ag` chứa gì?

dao vs dto

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

## Facts

Giờ Nhật Bản (`JST` - Japan Standard Time) nhanh hơn giờ Việt Nam (`ICT` - Indochina Time) đúng 2 tiếng. Khi ở Việt Nam là 10:00 sáng, thì tại Nhật Bản đã là 12:00 trưa.

Database dùng MySQL

## Tạo Task

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

## Terminologies

PF: platform

EH WebAPI: Etl Harbest WebAPI

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

master data = metadata

### Trivia

Etl harbest dùng sbt (scala build tool) ko dùng maven ?

## Resources

[Catchup Outline](https://ever-rise.backlog.jp/alias/wiki/566675)
