# ExamFlow — Examination Management System

A full-stack student/exam/results manager using HTML/CSS/JavaScript + Spring Boot + JDBC. The project now starts in **temporary H2 mode by default**, so you can use the website without an Oracle listener. The Oracle JDBC driver and a separate Oracle configuration are retained for switching back later.

## Requirements
- JDK 17 or newer (your installed Java can be newer)
- Maven 3.9+ (or use the Maven command that already works in your VS Code setup)
- Internet access the first time Maven downloads dependencies

## 1. Run the website in temporary mode (no Oracle listener required)
1. Extract the ZIP.
2. Open the `ExaminationManagementSystem` folder in VS Code (the folder containing `pom.xml`).
3. Open **Terminal → New Terminal**.
4. Run `mvn spring-boot:run` (or use your full Maven path if `mvn` is not on PATH).
5. Wait for Spring Boot to start, then open **http://localhost:8080**.

The app creates an in-memory H2 database and loads sample students, examinations, and results each time it starts. Changes are available while the app is running; restarting resets the demo data.

Optional H2 database console: **http://localhost:8080/h2-console**
- JDBC URL: `jdbc:h2:mem:emsdb`
- User Name: `sa`
- Password: leave blank

## 2. Switch back to Oracle XE 11g later
When your Oracle listener is running and the Oracle schema has been created, run the app with the `oracle` Spring profile. From the project root, use:

`mvn spring-boot:run -Dspring-boot.run.profiles=oracle`

The Oracle configuration is in `src/main/resources/application-oracle.properties`; its default URL is `jdbc:oracle:thin:@localhost:1521:XE`, appropriate for many Oracle XE 11g installations. Update the URL, username, and password if your database uses different settings. The app does not automatically create Oracle tables.

To use the standard Oracle SQL*Plus schema, review `database.sql` first. It creates `ems_user` and sample tables/data. Run it as an Oracle administrator, adjusting the `CONNECT` service if needed. Do not run the CREATE USER line again if that user already exists. For real use, replace the sample password.

## Features
- Dashboard metrics
- Add/list/delete students
- Add/list/delete examinations
- Add/list/delete results
- Automatic grade and pass/fail calculation
- Database connection indicator

## JDBC and ODBC note
The app accesses its selected database through JDBC. ODBC is a separate connectivity route; it requires a compatible Oracle ODBC driver and Windows ODBC data source (DSN) if your lab requires a separate ODBC demonstration. This web app does not use ODBC at runtime.

## Troubleshooting
- `mvn` not recognized: use the full Maven path that worked previously.
- Port 8080 busy: change `server.port` in the active properties file and use the matching URL.
- Oracle `connection refused`: use default H2 mode for the demo, or ask the administrator to start/fix the Oracle listener before using the Oracle profile.
