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

<https://www.tutorialspoint.com/jackson_annotations/index.htm>