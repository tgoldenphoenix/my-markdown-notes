# Clean code

SQL nên viết `IN ()` không viết `NOT IN ()`

`IN (1,2,NULL)` sẽ ra `=1, =2, ==NULL` mà `==NULL` ra `unknown`; phải dùng `IS NULL`
