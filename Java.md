
- [Packages](#packages)
  - [Apache HttpComponents](#apache-httpcomponents)
  - [CloudBoost](#cloudBoost)
  - [SQLITE JDBC Drivers](#sqlite-jdbc-drivers)
- [Datatypes](#datatypes)
- [Access Modifiers](#access-modifiers)
- [Snippets](#snippets)
- [Links](#links)

# Packages

## Apache HttpComponents

https://hc.apache.org/

## CloudBoost

https://cloudboost.io

http://mvnrepository.com/artifact/io.cloudboost/JavaSDK/1.0.7

Maven POM:

```xml
<!-- https://mvnrepository.com/artifact/io.cloudboost/JavaSDK -->
<dependency>
    <groupId>io.cloudboost</groupId>
    <artifactId>JavaSDK</artifactId>
    <version>1.0.7</version>
</dependency>
```

## SQLITE JDBC Drivers

https://bitbucket.org/xerial/sqlite-jdbc/downloads/

# Datatypes

| Type    | Value range                                                                | Size   |
|---------|----------------------------------------------------------------------------|--------|
| boolean | true or false                                                              | 1 Bit  |
| char    | 16-Bit-Unicode (0x0000 … 0xFFFF)                                           | 16 Bit |
| byte    | –2^7 to 2^7 – 1 (–128 … 127)                                               | 8 Bit  |
| short   | –2^15 to 2^15 – 1 (–32.768 … 32.767)                                       | 16 Bit |
| int     | –2^31 to 2^31 – 1 (–2.147.483.648 … 2.147.483.647)                         | 32 Bit |
| long    | –2^63 to 2^63 – 1 (–9.223.372.036.854.775.808 … 9.223.372.036.854.775.807) | 64 Bit |
| float   | 1,40239846E–45f … 3,40282347E+38f                                          | 32 Bit |
| double  | 4,94065645841246544E–324 … 1,79769131486231570E+308                        | 64 Bit |

# Access Modifiers

| Modifier    | Class | Package | Subclass (same pkg) | Subclass (diff pkg) | World |
|-------------|-------|---------|---------------------|---------------------|-------|
| public      | +     | +       | +                   | +                   | +     |
| protected   | +     | +       | +                   | +                   |       |
| no modifier | +     | +       | +                   |                     |       |
| private     | +     |         |                     |                     |       |

# Snippets

## Get User Home Directory

```java
String userHome = System.getProperty("user.home");
```

## Read User input from console

```java
String name = new java.util.Scanner( System.in ).nextLine();
int age = new java.util.Scanner( System.in ).nextInt();
double value = new java.util.Scanner( System.in ).nextDouble();
```

# Links

* Swing / Threading

  * https://www.javamex.com/tutorials/threads/invokelater.shtml
