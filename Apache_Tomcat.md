- [Web Application Directory Structure](#web-application-directory-structure)

## Web Application Directory Structure

http://tomcat.apache.org/tomcat-8.5-doc/appdev/deployment.html

| Directory          | Description                                                                  |
|--------------------|------------------------------------------------------------------------------|
| / (root)           | Everything public visible (*.html, *.jsp, ...)                               |
| /WEB-INF/          | Everything in this folder (and subfolders) is not public visible             |
| /WEB-INF/web.xml   |  XML file describing servlets and other components                           |
| /WEB-INF/classes/  | Java class files (and associated resources) required for the application     |
| /WEB-INF/lib/      |  JAR files                                                                   |
