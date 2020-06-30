# Data Types
PostgreSQL has a rich set of native data types available to users. Users can add new types to PostgreSQL using the CREATE TYPE command.

Table follow shows all the built-in general-purpose data types. Most of the alternative names listed in the “Aliases” column are the names used internally by PostgreSQL for historical reasons. In addition, some internally used or deprecated types are available, but are not listed here.

## Numeric Type
| Name             | Aliases    | Description                                                |
| ---------------- | ---------- | ---------------------------------------------------------- |
| smallint         | 2 bytes    | small-range (-32768 to +32767)                             |
| integer          | 4 bytes    | typical (-2147483648 to +2147483647)                       |
| bigint           | 8 bytes    | large-range (-9223372036854775808 to +9223372036854775807) |
| decimal          | variable   | user-specified precision, exact up to 131072 digits before the decimal point; up to 16383 digits after the decimal point                                    |
| numeric          | variable   | user-specified precision, exact 	up to 131072 digits before the decimal point; up to 16383 digits after the decimal point                                    |
| real             | 4 bytes    | inexact (6 decimal digits precision)                       |
| double precision | 8 bytes    | inexact (15 decimal digits precision)                      |
| smallserial      | 2 bytes    | small autoincrementing (1 to 32767)                        |
| serial           | 4 bytes    | typical autoincrementing (1 to 2147483647)                 |
| bigserial        | 8 bytes    | large autoincrementing (1 to 9223372036854775807)          |

**Obs1:**  To declare a column of type numeric use the syntax:
```
NUMERIC(precision, scale)
```
Or
```
NUMERIC(precision)
```

**Obs2:** The data types smallserial, serial and bigserial are not true types, but merely a notational convenience for creating unique identifier columns (similar to the AUTO_INCREMENT property supported by some other databases). In the current implementation, specifying:
```
CREATE TABLE tablename (
    colname SERIAL
);
```

## Text Type
| Name                             | Description                |
| -------------------------------- | -------------------------- |
| character varying(n), varchar(n) | variable-length with limit |
| character(n), char(n)            | fixed-length, blank padded |
| text                             | variable unlimited length  |

Both char(n) and varchar(n) can store up to n characters in length. If you try to store a longer string in the column that is either char(n) or varchar(n), PostgreSQL will issue an error.

If you do not specify the n integer for the varchar data type, it behaves like the text data type. The performance of the varchar (without n) and text are the same.

## Date/Time Type
PostgreSQL supports the full set of SQL date and time types. The operations available on these data types are described in [Section 9.9](https://www.postgresql.org/docs/12/functions-datetime.html). Dates are counted according to the **Gregorian calendar**, even in years before that calendar was introduced. 

| Name | Storage Size | Description | Low Value | High Value | Resolution |
| ----------------------------------- | -------- | ---------------------------------- | ---------------- | --------------- | ------------- |
| timestamp [(p)] [without time zone] | 8 bytes  | both date and time (no time zone)  | 4713 BC          | 294276 AD       | 1 microsecond |
| timestamp [(p)] with time zone      | 8 bytes  | both date and time, with time zone | 4713 BC          | 294276 AD       | 1 microsecond |
| date                                | 4 bytes  | date (no time of day)              | 4713 BC          | 5874897 AD      | 1 day         |
| time [(p)] [without time zone]      | 8 bytes  | time of day (no date)              | 00:00:00         | 24:00:00        | 1 microsecond |
| time [(p)] with time zone           | 12 bytes | time of day (no date)              | 00:00:00+1459    | 24:00:00-1459   | 1 microsecond |
| interval [fields] [(p)]             | 16 bytes | time interval                      | -178000000 years | 178000000 years | 1 microsecond |

time, timestamp, and interval accept an optional precision value p which specifies the number of fractional digits retained in the seconds field. By default, there is no explicit bound on precision. The allowed range of p is from 0 to 6.

The interval type has an additional option, which is to restrict the set of stored fields by writing one of these phrases:
```
YEAR
MONTH
DAY
HOUR
MINUTE
SECOND
YEAR TO MONTH
DAY TO HOUR
DAY TO MINUTE
DAY TO SECOND
HOUR TO MINUTE
HOUR TO SECOND
MINUTE TO SECOND
```

For more infos, click [here.](https://www.postgresql.org/docs/12/datatype-datetime.html)

## Boolean Type
| Name        |  Description           |
| ----------- | ---------------------- |
| boolean     | state of true or false |

## Others

| Name    | Description                   |
| ------- | ----------------------------- |
| cidr    | IPv4 or IPv6 network address  |
| uuid 	  | universally unique identifier |
| tsquery | text search query             |