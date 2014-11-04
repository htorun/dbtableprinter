Database Table Printer
======================

**DBTablePrinter** is a Java utility class for printing rows from a given database table or a `java.sql.ResultSet` to standard out, formatted to look like a table with rows and columns with borders. It is part of my efforts to learn Java, Javadoc, how to use an IDE (IntelliJ IDEA 13.1.15 in this case), how to apply an open source license and how to use Git and GitHub for version control and publishing an open source software.

## Basic Usage

There are two overloaded static methods: `printTable()` and `printResultSet()`. Here is how to use them:

```java
// Create a connection to the database
Connection conn = DriverManager.getConnection(url, username, password);

// Just pass the connection and the table name to printTable()
DBTablePrinter.printTable(conn, "employees");
```

It should print something like this:

```
Printing 10 rows from table(s) EMPLOYEES
+--------+------------+------------+-----------+--------+-------------+
| EMP_NO | BIRTH_DATE | FIRST_NAME | LAST_NAME | GENDER |  HIRE_DATE  |
+--------+------------+------------+-----------+--------+-------------+
|  10001 | 1953-09-02 | Georgi     | Facello   | M      |  1986-06-26 |
+--------+------------+------------+-----------+--------+-------------+
|  10002 | 1964-06-02 | Bezalel    | Simmel    | F      |  1985-11-21 |
+--------+------------+------------+-----------+--------+-------------+
    .
    .
```

`printTable()` uses `DEFAULT_MAX_ROWS`, which is 10, to limit the number of rows to query and print. This can be changed by calling the method with a third parameter: 

```java
int maxRows = 3;
DBTablePrinter.printTable(conn, "employees", maxRows);
```

`printResultSet()` on the other hand, does not have this limit and prints each row of the given `ResultSet`:

```java
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM EMPLOYEES LIMIT 15");
DBTablePrinter.printResultSet(rs);
```

Joined tables and column labels can also be used with `printResultSet()`:

```java
String sql = "SELECT EMPLOYEES.EMP_NO AS ID, FIRST_NAME, LAST_NAME, SALARY, " +
                    "FROM_DATE, TO_DATE FROM EMPLOYEES LEFT JOIN SALARIES " +
                    "ON EMPLOYEES.EMP_NO = SALARIES.EMP_NO LIMIT 3";
ResultSet rs = stmt.executeQuery(sql);
DBTablePrinter.printResultSet(rs);
```

Should print:

```
Printing 3 rows from table(s) EMPLOYEES, SALARIES
+--------+------------+-----------+--------+-------------+-------------+
|   ID   | FIRST_NAME | LAST_NAME | SALARY |  FROM_DATE  |   TO_DATE   |
+--------+------------+-----------+--------+-------------+-------------+
|  10001 | Georgi     | Facello   |  60117 |  1986-06-26 |  1987-06-26 |
+--------+------------+-----------+--------+-------------+-------------+
|  10001 | Georgi     | Facello   |  62102 |  1987-06-26 |  1988-06-25 |
+--------+------------+-----------+--------+-------------+-------------+
|  10001 | Georgi     | Facello   |  66074 |  1988-06-25 |  1989-06-25 |
+--------+------------+-----------+--------+-------------+-------------+
```
The souce code is heavily commented with Javadoc and inline comments mainly for me to learn how to use them and maybe for a remote possibility that someone finds it useful.

By the way, I used Java 8 during development but I didn't use lambdas, streams etc. so JREs > 1.5 should work (not tested though). I am planning to learn and use those but maybe not for this simple project ;)

## License
Database Table Printer
Copyright (C) 2014  Hami Galip Torun

Email: hamitorun@e-fabrika.net
Project Home: https://github.com/htorun/dbtableprinter

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
