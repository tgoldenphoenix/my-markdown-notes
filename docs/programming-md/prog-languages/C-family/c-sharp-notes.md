# C# notes

`namespace` is used to organize your code, and it is a container for classes and other namespaces. `System` is a namespace

`class` is a container for data and methods, which brings functionality to your program. Every line of code that runs in C# must be inside a class.

namespace(System) > classes(Program) > method (Main) > object

Cách phân chia code: class Program contains the Main method. Other classes contain the actual codes to be executed.

using system namespace. `Console` is a class of the `System` namespace, which has a WriteLine() method that is used to output/print text.

class can have objects inside them

getter, setter được gọi là `properties`. A property is like a combination of a variable and a method, and it has two methods: a get and a set method. Properties khác với variable/fields. [Read more](https://www.w3schools.com/cs/cs_properties.php)

react rsf template

Use `Console.WriteLine("");` to debug

## Cách push lên git

[căn bản](https://www.w3schools.com/git/git_pull_from_remote.asp?remote=github)
git fetch origin
[cách rebase](https://jvns.ca/blog/2024/02/01/dealing-with-diverged-git-branches/#way-1-git-status)
git rebase --continue

## eProject kì 3

anhao:Anhao123!:8002
anhao-admin2:Anhao123!:9002

bỏ role admin trong form user, chụp screenshot cho review 3

Reference links: [tham khảo react](https://js.devexpress.com/React/), [database](https://drive.google.com/file/d/1GWCUn5NecD3_TNJ5RfoOl8Fz8F2DFNN_/view?usp=sharing), link [google drive](https://drive.google.com/drive/folders/1K-w0JtENh1Du_xdsBnRzwwg1N87HDY7R) của thầy
[link doc mới](https://drive.google.com/drive/folders/1U_jQXW8H6EhNHEVIX7jeb8wXjlPNfWs2)

Các file đã thay đổi:

Account có cái accounttype (Saving,checking,student, business,retirement)

bao nhiêu giờ thì cho login lại nếu sai 3 lần (token???)

sau khi chuyển khoản có thể gởi mail thông báo cho recipient

4 lớp theo thứ tự từ trong ra ngoài: Domain -> Application -> Infrastructure -> API (UI)
1. Domain: Interface (dùng repository trong Infrastructure) + Model (entities). Là lớp trong cùng
2. Application: chứa services (dùng Interfaces trong Domain) + DTOs 
3. Infrastructure: databaseContext (thư viện EntityFrameworkCore) + repository (dùng thư viện databaseContext để implement logic)
4. Lớp API là lớp trong cùng rồi từ API đổ ra UI REACT. Lớp API chứa Controllers dùng Service trong Application
Cần phân biệt Interface repository và repository là 2 file khác nhau. Có một dòng .AddScope trong `API/Program.cs` link 2 file này lại với nhau.

Add-Migration InitialCreate -OutputDir Data/Migrations -Project OnlineBanking.Infrastructure -StartupProject OnlineBankingAPI

dotnet ef database update InitialCreate --project OnlineBanking.Infrastructure --startup-project OnlineBankingAPI

**Constrains**
- User: dob phải trên 16 tuổi, phone unique, email unique
- Account: accountNumber phải 13 char

useEffect dependency [here](https://react.dev/reference/react/useEffect#examples-dependencies)

react fetch object từ API -> nguyên tắc DOB (dob) còn FirstName (firstName)
Còn từ react thêm data vào API không cần viết hoa, all lower case cũng được.

Khi update thì date time là string có `""` còn int và bool thì không có `""`

Cái nút pagination làm cho contrast hơn chút.

Ấn độ chấm điểm bằng document nên phải viết document tốt

Table of conent project có cái gì thì mới bỏ vô nhé

trong form quan trọng nhất là name và value

 về gắn icon, làm status pending

## ASP\.NET Core MVC

asp\.net core mvc tutorial [here](https://learn.microsoft.com/vi-vn/aspnet/core/tutorials/first-mvc-app/start-mvc?view=aspnetcore-6.0&tabs=visual-studio)

## Migration

Migrate db code first .NET

B1: cd 'project name'. Phải cd 2 lần for some reason
B2: dotnet tool install --global dotnet-ef --version ‘version tương thích với project đã tạo’ (version 8)
B3: export PATH="$PATH: 'project name' .dotnet/tools"
B4: dotnet ef migrations add 'project database' (M0DBB3) chọn tên gì cũng được. Cái tên trong appsetting mới quan trọng
B5: dotnet ef database update

Remove-Migration
Add-Migration ProductTBUpdate

Add-Migration UserTB

## Terminology

Class member: method & fields [Read more](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/members)

## Notes on class

trong sql string phải dùng single quote

[add migration](https://stackup.hashnode.dev/ef-migrations-visual-studio-mac)

Đi thi ko dùng dependency injection, dùng cách buổi 1
Project làm dotnet API + React front-end (or làm MVC). Khuyến cáo làm API

Tên method trong `ProductController.cs` phải giống `asp-action=""` attribute bên file View `cshtml`. 

`asp-route-id=""` trong View phải giống cái parameter name trong controller.

**TODO**: Về làm cái name search mang qua gắn lại bấm search không mất value (B2)

partial view

Automatic Properties (Short Hand). `prop` khi tạo Model

## Questions?

ICollection là gì?

## Các bước tạo project mới

1. Tạo Model
2. Tạo DatabaseContext trong Model
3. Vào `Program.cs` Add services to the container. Services Phải đặt trên "app"
4. Tạo controller

## References

[This seri of articles](https://www.w3schools.com/cs/cs_syntax.php) from w3school covers the basics of C#

[What's happening to Visual Studio for Mac?](https://learn.microsoft.com/en-us/visualstudio/mac/what-happened-to-vs-for-mac?view=vsmac-2022)

[ASP.NET Core official documentation](https://learn.microsoft.com/vi-vn/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-7.0)

How to code-first migrate database on M1 [here](https://stackup.hashnode.dev/ef-migrations-visual-studio-mac)