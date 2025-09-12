# 03: CSW notes

Identity Service

[MEGA](https://mega.nz/folder/muQFVQwB#GnjN8HzVPtxKqULsRLgzLw)

## 03: APIJersey auto-gen entity from DB

Mình không làm API bằng jersey framework.

demo cho môn csw một buổi duy nhất. Những buổi khác demo spring Boot. Tạo 2 project folder API dùng Jersey framework, front-end dùng Thymeleaf call API.

project API\
Tạo project: jersey, spring data jpa, postgres driver\
Tạo table trong DB first rồi generate entity class sau (JPA not hibernate)

Trong thymeleaf, `xmlns`: xml name space. `th` là týp đầu ngữ

[thymeleaf doc](https://www.thymeleaf.org/doc/tutorials/3.1/thymeleafspring.html#preface)

Thymeleaf syntax:
biến (variable) `${}`
url, links `@{}`
attribute `*{}`
message `#{}`

jersey only support get & post method in html form

