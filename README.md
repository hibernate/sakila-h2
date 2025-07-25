<!--
  ~ Copyright 2012 - 2025 Red Hat, Inc.
  ~
  ~ Licensed under the Apache License, Version 2.0 (the "License");
  ~ you may not use this file except in compliance with the License.
  ~ You may obtain a copy of the License at
  ~
  ~     http://www.apache.org/licenses/LICENSE-2.0
  ~
  ~ Unless required by applicable law or agreed to in writing, software
  ~ distributed under the License is distributed on an "AS IS" basis,
  ~ WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  ~ See the License for the specific language governing permissions and
  ~ limitations under the License.
  -->
# Sakila Sample Database for H2

## Introduction

This project is a recreation for the [H2 database engine](https://www.h2database.com/html/main.html)
of the [Sakila sample database](https://dev.mysql.com/doc/sakila/en/) 
that was originally created for [MySQL](https://dev.mysql.com).

## Project contents

### sakila.sql

Currently, the [sakila.sql](sakila.sql) file contains the code to recreate the Sakila
database schema and populate the database with sample data. The file was created
using the files `sakila-schema.sql` and `sakila-data.sql`. These files are available
in the sakila-db archive downloadable from the 
[MySQL website](https://downloads.mysql.com/docs/sakila-db.zip) and were licensed 
under the [new BSD license](http://www.opensource.org/licenses/bsd-license.php).

### sakila.mv.db

The [sakila.mv.db](sakila.mv.db) file is the result of executing the [sakila.sql](sakila.sql)
script from above. Thus contains all the tables and data for the Sakila sample database.

### runh2.sh

[runh2.sh](runh2.sh) is a convenience shell script that can be used to run the H2 Sakila 
Sample database from a command shell. The H2 engine will be started in server mode. 
The script uses the [sakila.mv.db](sakila.mv.db) above and the [H2 jar file](h2-2.3.232.jar) 
below.

### h2-\<version>.jar

[h2-\<version>.jar](h2-2.3.232.jar) is the executable jar file to run the 
[H2 database engine](https://www.h2database.com/html/main.html). It also contains the
JDBC drivers needed to connect to the database from your Java applications. It is
used by the [runh2.sh](runh2.sh) script above.
The [H2 database engine](https://www.h2database.com/html/main.html) is dual licensed under 
the [Mozilla Public License Version 2.0](https://www.mozilla.org/en-US/MPL/2.0/) and the 
[Eclipse Public License Version 1.0](https://www.eclipse.org/legal/epl/epl-v10.html).

### README.md

This file 


## How to use

### Server mode

You can use the [runh2.sh](runh2.sh) shell script to start the database in server mode.
Just issue `./runh2.sh` from a command line window.

```shell
me@machine sakila-h2 % ./runh2.sh 
Starting h2 server - jdbc url: jdbc:h2:tcp://localhost/./sakila
TCP server running at tcp://localhost:9092 (only local connections)
Web Console server running at http://localhost:8082 (only local connections)
```

Now you can connect to it from Java code using JDBC, e.g. as follows:

```java
Connection connection = DriverManager.getConnection("jdbc:h2:tcp://localhost/./sakila", "sa", "");
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery("select * from ACTOR");
ResultSetMetaData resultSetMetaData = resultSet.getMetaData();
int columnCount = resultSetMetaData.getColumnCount();
for (int i = 1; i <= columnCount; i++) {
    System.out.println("Column " + i + " -> " + resultSetMetaData.getColumnName(i));
}
```

### Embedded Mode

In this case you just use the path to the folder containing the[sakila.mv.db](sakila.mv.db) 
file in the JDBC and append `sakila` as this is the name of the database to your 
connection string. Assuming this file is for instance located in the folder called 
`Databases` under your
home directory, the code above would look as follows:

```java
Connection connection = DriverManager.getConnection("jdbc:h2:~/Databases/sakila", "sa", "");
Statement statement = connection.createStatement();
ResultSet resultSet = statement.executeQuery("select * from ACTOR");
ResultSetMetaData resultSetMetaData = resultSet.getMetaData();
int columnCount = resultSetMetaData.getColumnCount();
for (int i = 1; i <= columnCount; i++) {
    System.out.println("Column " + i + " -> " + resultSetMetaData.getColumnName(i));
}
```

### More Options

Since the database is a H2 database, all the documentation available from the 
[H2 website](https://www.h2database.com/html/main.html) is valid.

## Database Structure and Contents

Full documentation about the structure and the contents of the Sakila Sample Database
can be found on the [MySQL Sakila Database website](https://dev.mysql.com/doc/sakila/en/sakila-structure.html).

