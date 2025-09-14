# SQL Database notes

SQL is a language used to interact with relational database management systems (RDBMS). Mỗi RDBMS có một flavour SQL khác nhau.

- sql server: SA:MyStrongPass123
- mySQL: root:password
- pg admin: password

## CREATE TABLE & Data types

In SQL Server the default **schema** is `dbo`. There can be multiple schemas inside one database.

Khi nào ta tạo column có dữ liệu kiểu CHAR:

- Khi colum đó có kiểu dữ liệu là kiểu chuỗi (nghĩa là có chữ cái hoặc chữ cái và số lộn xộn với nhau và có maxlength) và có độ dài chuỗi cố định
- Thường dùng cho các mã (mã học sinh, mã lớp, mã đơn hàng,...) hay số cmnd
- Ví dụ: Mã HọcSinh (thường là PK của bảng HOCSINH) có 5 ký tự-có độ dài chuỗi cố định → Char(5); hoặc như số CMND thường có 9 ký tự → char(9)

Nên ưu tiên `CHAR` hơn `VARCHAR` vì nó có độ dài cố định nên sẽ tốt hơn khi: đánh index, tìm kiếm nhanh hơn.

In `char(n)` and `varchar(n)`, the `n` defines the string length in bytes. `n` never defines numbers of characters that can be stored.  
The misconception happens because when using **single-byte encoding**. However, for multibyte encoding such as UTF-8, one character might use two or more bytes.  
For example, in a column defined as `char(10)` can store 10 characters that use single-byte encoding (Unicode range 0 to 127), but fewer than 10 characters when using multibyte encoding (Unicode range 128 to 1,114,111).

- `varchar`: store single-byte characters. Không chứa được tiếng Việt.
- `nvarchar`: store double-byte characters. Chứa được tiếng Việt. Nếu muốn chứa 50 characters thì cần `nvarchar(100)`

Phân biệt giữa NULL và rỗng:

- NULL và rỗng là khác nhau
- NULL có nghĩa là ô đó không có giá trị hoặc chưa có giá trị (ví dụ: không có cái thùng)
- Rỗng có nghĩa là ô đó có giá trị (ví dụ: có cái thùng nhưng không có gì ở trong đó hết)

1 cột được khai là kiểu `varchar` thì có thể nhận được giá trị là rỗng không?  
Được. vì kiểu varchar cho phép từ 0 đến n kí tự (`varchar (50)` nghĩa là tối da 50 kí tự - từ 0 đến 50)

1 cột được khai là kiểu `char` thì có thể nhận được giá trị là rỗng không?  
Không vì `CHAR` thường là một mã số (mã SV, mã môn học) và bắt buột phải có dữ liệu. Ví dụ `CHAR(5)` thì bắt buộc có 5 ký tự.

The CHAR data type in SQL databases automatically pads values with spaces to the declared fixed length of the column.  
This means that if you define a column as CHAR(10) and insert the value `ABC`, it will be stored as 'ABC ' (with seven trailing spaces) to fill the full 10-character length. This behavior is a fundamental characteristic of the CHAR data type, distinguishing it from VARCHAR which stores only the actual length of the string without padding.

Number data types:

- `int`: 4 bytes integer, no decimal, up to 2 billion `2,147,483,647` có thể dùng làm id, primary key.
- `DECIMAL or NUMERIC`: 2 cái này giống nhau, dùng khi cần store decimal. `DECIMAL(12,4)` will stored 4 numbers after the decimal point and `12-4=8` whole numbers like `99,345,678.1234`.
- `FLOAT`: an approximation, dùng OK trừ khi nào cần precision (like accounting systems)

Date data types:

- `datetime`: Supports dates from January 1, 1753, to December 31, 9999.
- `datetime2`: Offers a much broader range, from January 1, 0001, to December 31, 9999. Both of them store date + time (up to mili-seconds)
- `date`: no time portion

## Managing Tables & Modifying Data

Phân biệt giữa việc xóa dữ liệu của bảng bằng `DELETE` và `TRUNCATE`:

- `DELETE` và `TRUNCATE` là để xóa nội dung (dòng) trong bảng. Muốn xóa bảng thì dùng `DROP TABLE`
- 2 cái này khác nhau khi trong bảng có 1 cột có giá trị tự động tăng Nếu như đang có 100 dòng, mà xóa bằng `DELETE` thì sau khi xóa các dòng xong, mình nhập vào 1 dòng mới thì cột đó vẫn tự động tăng thành giá trị 101; còn khi dùng xóa `TRUNCATE` thì cột đó sẽ có giá trị quay về số 1
- Vậy thì `TRUNCATE` mới thực sự là xóa hết dữ liệu (reset dữ liệu về trạng thái ban đầu), còn DELETE chỉ là xóa hết các dòng dữ liệu tạm thời

## SELECT

3 bí kíp bản chất của SQL:

1. Bí kíp số 1: cú pháp của lệnh `SELECT`
2. Bí kíp số 2: Chỉ cần nhớ 1 chữ duy nhất là **DÒNG** (or ROW, Records, bảng ghi)
3. Bí kíp số 3: Chỉ cần nhớ một cụm từ nữa là **BẢNG TẠM**.

Lệnh `SELECT` dùng để lấy ra các dòng (row) phù hợp với một tiêu chí nào đó trong 1 hoặc nhiều bảng.

Cú pháp của lệnh `SELECT` có 6 chữ (và bổ sung thêm 1 `JOIN`). Ngoài `SELECT` chạy sau cùng ra thì thứ tự thực hiện các câu lệnh là:

1. `SELECT` (viết đầu tiên nhưng được chạy sau cùng)
2. Đầu tiên là thực hiện `FROM` rồi tới `JOIN` rồi tới `WHERE`
3. `WHERE` lọc những dòng trong bảng tạm trả về từ phép nối `JOIN` nếu có
4. `GROUP BY`
5. `HAVING`
6. `ORDER BY`

Mnemonic: So Few Junior Workers Go Home On-time

### WHERE

`WHERE` là nơi để đặt các điều kiện **LỌC DÒNG** theo một tiêu chí nào đó. Điều kiện `WHERE` will be checked for each row of the table.  
SQL có một con trỏ **duyệt qua từng dòng** check điều kiện khi `SELECT`.

Từ khóa `LIKE` dùng để tìm kiếm chuỗi 1 cách tương đối (giống regex), nhưng với điều kiện là đi kèm với các ký tự đại diện `%, _, [], [^]`. Nếu không có các ký tự đặc biệt này thì `LIKE` tương đương với `=` (match exactly).

```sql
Select * from SINHVIEN
Where HoTenSV LIKE 'Bilbo'
```

Câu Select trên sẽ không liệt kê `Bilbo Baggins`, only rows with exactly `Bilbo`.

- Percent sign `%` matches any sequence of zero or more characters.
- Underscore sign `_`  matches any single character.

### ORDER BY, GROUP BY, DISTINCT, HAVING

`ORDER BY` sắp xếp **các dòng** của **bảng tạm** sinh ra sau khi lọc `WHERE` dựa vào sự tăng dần hoặc giảm dần của một hay nhiều cột.

Bảng tạm là bảng sinh ra tạm thời trong quá trình xử lý câu lệnh SQL. Một câu lệnh SQL có thể tạo ra nhiều bảng tạm trung gian trong quá trình xử lý. `ORDER BY` **KHÔNG HỀ** sắp xếp cái table gốc ban đầu.  
Và nhớ bí kíp 02, `ORDER BY` sắp xếp các DÒNG dựa vào giá trị trong cột specify chứ không phải sắp xếp cột.

`ORDER BY` sort `ASC` by default unless specify `DESC` (giảm dần).

Độ ưu tiên của các cột trong mệnh đề `ORDER BY` và `GROUP BY`:

- Trong câu query `Order by gioi_tinh ASC, ho_ten DESC`: Ưu tiên sắp xếp column giới tính trước. Nếu nhiều dòng có giới tính giống nhau thì chỉ khi đó mình mới dùng họ tên để sắp xếp.
- `GROUP BY hoten, gioitinh`: thực chất không có cái nào ưu tiên hơn cả; chỉ khi nào cả họ tên và giới tính trùng nhau thì mới đưa về cùng group.

`GROUP BY` nhóm rows trong bảng tạm dựa vào sự giống nhau (về giá trị) của một hoặc nhiều cột thành từng nhóm. Với mỗi nhóm, mình có thể apply an **aggregate function** (hàm gộp) such as `SUM()` to calculate the sum of items or `COUNT()` to get the number of items in the groups.  
Example: retrieve the total payment paid by each customer from table `payment`

Sau khi `GROUP BY`, có bao nhiêu nhóm thì trả về bấy nhiêu dòng kết quả, mỗi dòng tượng trưng cho một nhóm. Đôi khi kết quả trả về sẽ giống `DISTINCT`.

- Không được `SELECT` các cột không có trong mệnh đề `GROUP BY` (nếu trong câu `SELECT` có dùng `GROUP BY`).
- Có thể Select những hằng số, để tạo ra những "no name columns"
- Không nhất thiết phải Select các cột có trong `GROUP BY` vì câu `SELECT` chạy sau cùng chứ không phải Select rồi mới có cái để `GROUP BY`.
- Có thể dùng hàm gộp cho những cột không có trong `GROUP BY`

Aggregate functions perform a calculation on a set of rows and return a single row. Hàm gộp thường đi chung với `GROUPBY` và sẽ không bao giờ bị lỗi. Khi dùng hàm gộp trong `SELECT` mà không có `GROUP BY` thì nó sẽ hiểu rằng cả cái bảng là 1 nhóm được `GROUP BY`. Có 5 hàm gộp:

1. `AVG()`
2. `COUNT()`
3. `MAX()`
4. `MIN()`
5. `SUM()`

Vì hàm gộp dùng để tính toán giá trị **trên từng nhóm** nên phải `GROUP BY` xong mới được gọi aggregate functions:

- Suy ra hàm gộp không được nằm trong `WHERE` mà phải là `HAVING` vì `WHERE` chạy trước `GROUP BY`, `HAVING` chạy sau `GROUP BY`.
- Gọi hàm gộp trong `SELECT` được vì Select chạy sau cùng.

`DISTINCT` liệt kê không trùng lặp rows trong bảng tạm trả về (chứ không phải trong bảng gốc). Nó vẫn giữ lại 1 dòng chứ không xóa hết các dòng trùng lặp. Two or more rows must have exactly the same values in all the selected columns to be viewed as duplicates by `DISTINCT`.  
Example: how many rental rates for films from the film table?

Nếu `WHERE` là điều kiện lọc dòng (rows) thì `HAVING` là điều kiện lọc **dành cho các nhóm (groups)** trong bảng tạm được trả về từ `GROUP BY`. Nó lọc ra những nhóm mình cần lấy. Sau đó mới chạy qua `ORDER BY` rồi cuối cùng chạy `SELECT`.

Trong quá trình chạy từ `FROM` cho tới `ORDER BY` thì xử lý diễn ra hoàn toàn trên các dòng. Tức là các cột vẫn còn nguyên. Chỉ tới khi `SELECT` chạy sau cùng thì nó mới bỏ bớt các cột.  
Cho nên khi `GROUP BY gioi_tinh`, ta vẫn `COUNT(dia_chi)` được mà không lỗi, vì thật ra cột `dia_chi` vẫn còn sống sót sau khi `GROUP BY`.

Vì sao không được phép SELECT những cột không có trong mệnh đề `GROUP BY`?

```sql
Select QueQuan, HoTenSV
From SINHVIEN
Group by QueQuan, GioiTinh
```

Vì mỗi nhóm chỉ được trả về 1 dòng kết quả. Mà mỗi nhóm có rất nhiều SV, mỗi SV có `HoTenSV` khác nhau không biết lấy cái nào để trả về.  
Còn trong một nhóm thì đương nhiên `QueQuan` phải trùng nhau nên có thể Select được.

## JOIN table

`JOIN` nối các **dòng** của bảng này với các dòng của bảng kia. Kết quả trả về là những dòng mới (chứa trong bảng tạm) **chứa đầu đủ các cột** của 2 bảng tham gia.

Một bảng 3 columns `JOIN` với một bảng 2 columns sẽ trả về một bảng tạm 5 cột chứ không phải 4. Hai cái cột dùng để join là hai cột khác nhau.

Cái namespace `SV.` and `L.` chỉ cần thêm vào khi có sự trùng tên cột của 2 bảng tham gia phép nối, còn không thì không cần thêm.

`INNER JOIN` returns `apple` bảng A & `apple` bảng b

The `LEFT JOIN` clause joins a left table with the right table and returns all the rows from the left table that may or may not have corresponding rows in the right table. In case the values do not equal, the left join also creates a new row that contains columns from both tables and adds it to the result set (bảng tạm). However, it fills the columns of the right table with `NULL`  
`LEFT JOIN` is the same as writing `LEFT OUTER JOIN` so you can use them interchangeably.

```sql
-- LEFT JOIN result
apple apple
orange orange
banana null
cucumber null
```

Example:

- `LEFT JOIN`: liệt kê tất cả sinh viên (kể cả sv chưa được phân lớp)
- `RIGHT JOIN`: liệt kể chỉ những người đã được phân vào đúng lớp

Khi dùng `LEFT JOIN` hoặc `RIGHT JOIN` ta sẽ thấy cái cột primary key và cột foreign key để join là 2 cột có cùng tên nhưng data khác nhau.

`FULL OUTER JOIN` returns a result set that contains all rows from both left and right tables, with the matching rows from both sides if available. In case there is no match, the columns of the table will be filled with `NULL`.

The `JOIN ... USING` clause is a shorthand syntax used to simplify JOIN operations when the common column(s) between the two tables being joined have the same name.

```sql
-- ON clause
SELECT *
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
-- USING clause
SELECT *
FROM Orders
INNER JOIN Customers USING (CustomerID);
```

## Set Operations

`UNION, UNION ALL, INTERSECT và EXCEPT` là các phép quan hệ operators that allows you to combine the result sets of two or more SELECT statements into a single result set. `JOIN` only has one `SELECT`.

Các phép quan hệ:

- `UNION`: hợp, bỏ những dòng trùng nhau chỉ giữ lại 1 dòng. To retain the duplicate rows, you use the `UNION ALL` instead.
- `INTERSECT`: giao lấy phần chung
- `EXCEPT`: trừ (hiệu)
- `UNION ALL`: Hợp lấy hết không thương tiếc kể cả những dòng trùng lặp (military conscription)

The UNION and UNION ALL operators may order the rows in the final result set in an unspecified order. For example, it may place rows from the second result set before/after the row from the first result set. To sort rows in the final result set, you specify the `ORDER BY` clause after the second query.

Dùng những phép này khi đọc đề bài thấy chẳng liên quan gì với nhau.

- Find old men who buy fish **and** old women who smoke & drink coffee
- employee who had sell bột giặt OMO **và** chưa bao giờ đi công viên

- Liet kê những SV đã từng thi môn toán **hoặc** có quê Quảng Nam
- Liet ke nhung SV da từng thi môn toán **và** những SV co que o quang nam

```sql
(Select MaSV
From SinhVien SV
Inner Join DIEMTHI DT on SV.MaSV = DT.MaSV
Where MaMon = 'Toan')
UNION
(Select MaSV
From SinhVien SV
Where QueQuan='Quan Nam')
```

2 tập hợp `SELECT` muốn `UNION` lại với nhau phải thỏa mãn điều kiện:

- Cùng số lượng cột
- Các cột tương ứng với nhau thì phải có kiểu dữ liệu tương đồng
- Tên của cột trả về sẽ được lấy theo tập hợp ở trên

Tim những SV đang học tại trường nhưng cũng chính là giảng viên của trường đó?

```sql
(Select HoTenSV AS HoTen, NgaySinh, QueQuan
From SinhVien)
INTERSECT
(Select HoTenGV, NgaySinh, QueQuan
From GiangVien)
```

Phân biệt cách sử dụng giữa INTERSECT và JOIN

- JOIN (phép nối): sau khi JOIN thì dữ liệu có khuynh hướng giữ nguyên dòng và tăng số lượng cột. Tăng theo chiều ngang chứ không theo chiều dọc.
- INTERSECT: sau khi INTERSECT thì dữ liệu có khuynh hướng giữ nguyên số cột và giảm số hàng.

## Sub-query, IN, EXIST

subquery chạy trước trả về bảng tạm, lấy bảng tạm đó query tiếp tục. The main query will use the result of the subquery to filter data in its `WHERE` clause (sub-query cũng có một cái `WHERE` của riêng nó).

Sub-query thường chỉ return một column với nhiều dòng. Main query sẽ dùng `WHERE` của nó để filter trong đám trả về đó.

A subquery can return zero or more rows. If the query returns more than one row, you can use it with the `IN` operator.

The `EXISTS` operator is a boolean operator that checks the existence of rows in a subquery.  
If the subquery returns at least one row, the `EXISTS` operator returns `true`. If the subquery returns no row, the EXISTS returns `false`. Note that if the subquery returns `NULL`, the EXISTS operator returns `true`.

The result of `EXISTS` operator depends on whether any row is returned by the subquery, and **NOT** on the row contents. Therefore, columns that appear in the select_list of the subquery are not important. Thường người ta `SELECT 1` trong sub-query luôn.

To negate the EXISTS operator, you use the `NOT EXISTS` operator. The NOT EXISTS operator returns `true` if the subquery returns no row or `false` if the subquery returns at least one row.

Phân biệt sự khác nhau khi sử dụng `NOT IN` và `NOT EXITS`:

- `value IN (value1,value2,...)`: tìm 1 phần tử nào đó có nằm trong tập hợp nào đó hay không; returns `true` if the value is equal to any value in the list such as value1 and value2
- Ví dụ: như tìm đơn hàng được bán vào ngày 1 2 3 thì mình dùng cái `NGÀY IN (1,2,3)`.

- `NOT EXITS` (ngược lại với phép giao – phép nối bảng JOIN ) kiểm tra xem không nối bảng được hay không.
- Ví dụ tìm những nhân viên mà đã từng bán hàng thì lấy bảng `NHANVIEN JOIN` với bảng `BANHANG` là sẽ ra được những nhân viên bán hàng
- Còn muốn tìm những nhân viên chưa từng bán hàng thì mình sẽ tìm những thằng không nổi được giữa nhân viên và bán hàng. Và trong tình huống kiểm tra thử phép nối diễn ra không thàng công thì dùng `NOT EXITS`.
- I don't understand 3 dòng ở trên...

A **correlated subquery** is a subquery that references the columns from the outer query.

The `ANY` operator returns `true` if the comparison returns true for at least one of the values in the set, and false otherwise.  
`SOME` is a synonym for `ANY`, which means that you can use them interchangeably.

`value > ALL (subquery)` returns true if the value is greater than the biggest value returned by the subquery.

## Database Normalization in SQL - 1NF, 2NF, 3NF, 4NF

k

## Transaction

A transaction is a set of one or more statements that is executed as a unit, so either all of the statements are executed, or none of the statements is executed. This concept is crucial for ensuring data integrity and consistency in relational databases.

Transactions are governed by four key properties, collectively known as **ACID**:

1. Atomicity: A transaction is indivisible (all-or-nothing). Either all its operations are executed, or none are. Atomicity guards against partial updates that could lead to data inconsistencies.
2. Consistency: ?
3. Isolation: ?
4. Durability: Once a transaction is committed, it is permanently recorded in the database. This durability guarantees that the changes persist, even in the event of a system failure.

A typical transaction follows a specific life cycle:

1. Begin
2. Execute opertions
3. Check for Integrity: The system checks for consistency and integrity of the data.
4. Commit/Rollback: If the operations meet the necessary conditions, the transaction is committed, and changes are saved to the database. If any condition fails, the transaction is rolled back, and the database reverts to its previous state.

Transaction and lock

In practice, you’ll use transactions in stored procedures in PostgreSQL and in the application code such as PHP, Java, and Python.

```sql
-- BEGIN TRANSACTION;
-- BEGIN WORK;
BEGIN;

INSERT INTO accounts(name,balance) VALUES('Alice',10000);

COMMIT;
-- ROLLBACK;
```

transaction isolation

### concurrency problem

Two users add the same product to their cart and attempt to purchase it at the same time — but inventory may only have 1 unit left.  
Goal: Prevent overselling and ensure that only one user can successfully buy that last unit.

This is a concurrency problem: two users racing to buy the same last product at the same time.  
This is not something React alone can solve — React just handles the UI. The real solution is at the backend + database level.

- Inventory shows "1 product left".
- User A clicks "Buy".
- User B clicks "Buy" at the same time.
- Both React apps send requests to the backend.
- Without proper handling, both might succeed → overselling.

Database transaction:

- Use a DB that supports transactions (Postgres, MySQL, Mongo with transactions).
- When processing an order:
  1. Begin transaction.
  2. Check stock (must be > 0).
  3. Decrement stock.
  4. Commit.
- If two requests come at the same time, one succeeds, the other fails.

Backend

- If you don’t use DB transactions, at least Backend API must re-check stock before confirming an order.
- Use an atomic update (e.g., Mongo’s findOneAndUpdate with stock > 0).
- Never rely only on what React shows (UI may be outdated).

- Both users hit /api/products/1/buy at the same time.
- Spring opens a transaction.
- The first request locks the row with PESSIMISTIC_WRITE.
- Second request waits until the first finishes and will see stock = 0, the second request fails.

Queue-based approach (advanced):

- All orders go into a queue (e.g., RabbitMQ, Kafka).
- A single worker processes requests one at a time → no race conditions.
- More scalable for high-traffic systems.

Front-end:

- Show "Only 1 left!" for better UX, but don’t trust it 100%.
- After placing an order, show loading state until backend confirms.
- If backend says "Out of stock", show error → "Sorry, someone else just bought it".

## PL/pgSQL & stored procedures

PL/pgSQL procedural language adds many procedural elements, e.g., control structures, loops, and complex computations, to extend standard SQL.

PostgreSQL offers a syntax called dollar-quoted string constant or dollar quoting: `select $$I'm a string constant$$;` => In this example, we don’t have to double up the single quote.

Here’s the basic syntax of the dollar-quoted string constants: `$tag$<string_constant>$tag$`. The `tag` is optional.

PL/pgSQL is a block-structured language. Here’s the syntax of a block in PL/pgSQL:

```sql
[ <<label>> ]
[ declare
    declarations ]
begin
    statements;
	...
end [ label ];
```

The declaration section is optional whereas the body section is required.

The `begin` & `end` here is different from the `begin` transaction.

The `DO` statement is used to execute an anonymous block. It does not belong to the block.

A drawback of **user-defined functions** is that they cannot execute transactions. In other words, inside a user-defined function, you cannot start a transaction, and commit or rollback it. PostgreSQL 11 introduced stored procedures that support transactions.

## Views

A view is a named query stored in the PostgreSQL database server. A view is defined based on one or more tables which are known as **base tables**, and the query that defines the view is referred to as a **defining query**.

After creating a view, you can query data from it as you would from a regular table. Behind the scenes, PostgreSQL will rewrite the query against the view and its defining query, executing it to retrieve data from the base tables.

Views do not store data.

Advantage of using views:

1. Views help simplify complex queries. Instead of dealing with joins, aggregations, or filtering conditions, you can query from views as if they were regular tables. Typically, first, you create views based on complex queries and store them in the database. Then, you can use simple queries based on views instead of using complex queries.
2. Security: Views enable fine-grained control over data access. You can create views that expose subsets of data in the base tables, hiding sensitive information. This is particularly useful when you have applications that require access to distinct portions of the data.

## Indexes

An index is a data structure that increases the data retrieval speed by providing a rapid way to locate **rows** within specific columns in a table.  
`UPDATE` takes more time, but `SELECT` will takes less time (trade-off depend on the table)

```sql
CREATE INDEX contacts_name
ON contacts(name);
```

This statement creates an index named `contacts_name` on the `name` column of the `contacts` table.

After creating an index on the `name` column, PostgreSQL extracts data from the `name` column of the `contacts` table and insert it into the index data structure. This process may take time, depending on the number of rows in the contacts table.  
After creating an index, PostgreSQL must keep it synchronized with the table when doing inserting, updating, or deleting data from the table with the indexes.

Types of Indexes:

- B-tree index (balanced tree) is the default type. B-tree indexes maintain the sorted values, making them efficient for exact matches and range queries.
- Hash indexes maintain 32-bit hash code created from values of the indexed columns. Therefore, hash indexes can only handle simple equality comparisons (=).

## Race conditions

Multiple users try to mofidy the same record in the database at the same time (concurrently).

## PostgeSQL configurations

The password for the postgres user is the one you entered during the PostgreSQL installation.  
PostgesSQL password khác với pgadmin password. Nhưng có thể đặt trùng cũng được.

username: root
password: password (postgres password & pgadmin password)

**psql** is a terminal-based front-end to PostgreSQL. It enables you to type in queries interactively, issue them to PostgreSQL, and see the query results. Alternatively, input can be from a file or from command line arguments.

### command-line tool

`psql` là command-line tool của postgres

`\?` Coi bảng command in psql mode
cái nữa là `help` hiện ra bảng nhỏ hơn or `psql --help` (cái này dùng ở ngoài psql mode nhé)

`\l` show list of database

You can either use lowercase or uppercase. You are recommend to use UPPERCASE for SQL commands. That way, you can tell what is syntax and what is not. Remember to have semi-colon ở cuối command nhé.

`psql -h localhost -p 5432 -U <username> <DB name>` -> connect to a DB  
`-h` là host, nếu connecting to a remote server thì phải type ip ra.  
`-p` là port, default là 5432 như hình 1  
`-U` là username

The other way to connect to a psql DB
`psql` > `\l` > `\c <DB name>`
\c for “connect”. You can switch between DBs using the same command. Ko cần phải exit ra rồi vô cái mới.

## References

[Diagramming in Lucidchart](https://www.youtube.com/watch?v=xsg9BDiwiJE&list=PLUoebdZqEHTxpGCwKrb82cIvHNoNaBb4R) cover basic cardinality rất hay

[database start vip page](https://www.databasestar.com/vip/)

[PostgreSQL 17.0 Documentation](https://www.postgresql.org/docs/17/index.html)
