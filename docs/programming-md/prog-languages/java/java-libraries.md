# Java Libraries

## Date time

The class `java.util.Date` represents a specific instant in time, with millisecond precision.

The class `java.sql.Timestamp extends Date` is a thin wrapper around `java.util.Date` that allows the JDBC API to identify this as an SQL `TIMESTAMP` value.

Class `LocalDateTime`: A date-time without a time-zone in the ISO-8601 calendar system, such as `2007-12-03T10:15:30` (`yyyy-MM-dd HH:mm:ss`). The `T` in the middle stands for "Time".

`java.time.LocalDate` is a date without a time-zone in the ISO-8601 calendar system, such as 2007-12-03.

`LocalDate` represents **only a calendar date** (year, month, day), while `LocalDateTime` represents **both a calendar date and a wall-clock time** (year, month, day, hour, minute, second, nanosecond). Neither class stores a timezone or UTC offset.

---

You need `.toLocalDateTime()` simply as an intermediate bridge because `java.sql.Timestamp` does not have a `.toLocalDate()` method of its own.

## I/O Streams

k

## HttpClient

kk

## Jackson

In Jackson (the default JSON library in Play Framework and Spring Boot), **serialization** converts a Java object into JSON, and **deserialization** parses JSON back into a Java object.

---

Jackson relies on standard JavaBeans conventions (which is why **Lombok `@Getter` / `@Setter` / `@NoArgsConstructor`** pair with it automatically)

Both Jackson and Swagger inspect a class's **JavaBean properties** (combining fields, getters, and setters) rather than just raw fields.

## Swagger

`@ApiModelProperty` is an annotation from **Swagger 2 / Springfox** used on Java DTO/model fields or getters to document their schema, description, example values, and validation constraints in generated OpenAPI/Swagger UI documentation.

## Lombok

Lombok's `@Getter` and `@Setter` work at **compile time**.

**Class-level vs. Field-level:** Placing `@Getter`/`@Setter` on a class generates methods for all non-static fields. Placing it on a single field generates them only for that field (and overrides class-level settings).

## Thymeleaf

Trong thymeleaf, `xmlns`: xml name space. `th` là týp đầu ngữ

Thymeleaf syntax:
biến (variable) `${}`
url, links `@{}`
attribute `*{}`
message `#{}`
