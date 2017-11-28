- [Web Application Directory Structure](#web-application-directory-structure)

## Web Application Directory Structure

http://tomcat.apache.org/tomcat-8.5-doc/appdev/deployment.html

| Directory                        | Description                                                                  |
|----------------------------------|------------------------------------------------------------------------------|
| /webapp/ (root)                  | Everything public visible (*.html, *.jsp, ...)                               |
| /webapp/WEB-INF/                 | Everything in this folder (and subfolders) is not public visible             |
| /webapp/WEB-INF/web.xml          |  XML file describing servlets and other components                           |
| /webapp/WEB-INF/classes/         | Java class files (and associated resources) required for the application     |
| /webapp/WEB-INF/lib/             |  JAR files                                                                   |
| /lib/                            |  JAR files here are visible to all web applications and internal Tomcat      |
