# DEPENDENCIES — JavaViewControl (33_javaviewcontrol)

This document lists all external dependencies used by the project, how they were originally included, and how they were resolved for the Maven build.

## Runtime / Compile Dependencies

### Servlet API

The original build included two JARs: `lib/servlet-api.jar` and `lib/servlet.jar`, both from the Servlet 2.3 era. These provide the `javax.servlet` classes that the project's dispatcher servlet depends on. In Maven, this is replaced by `javax.servlet:servlet-api:2.5` with `provided` scope, meaning it's available at compile time but the servlet container (like Tomcat) supplies it at runtime. Version 2.5 is fully backward compatible with the 2.3 APIs used in this code.

### Apache Commons FileUpload 1.2.1

Originally `lib/commons-fileupload-1.2.1.jar`. Used by `JVCRequestContext` for handling multipart form data and file uploads. Replaced with the exact same version from Maven Central: `commons-fileupload:commons-fileupload:1.2.1`.

### Log4j 1.2.15

Originally `lib/log4j-1.2.15.jar`. Used by the `Log` class for application logging. Replaced with the exact same version from Maven Central: `log4j:log4j:1.2.15`. This version's POM on Maven Central declares optional dependencies on `javax.mail`, `javax.jms`, `com.sun.jdmk:jmxtools`, and `com.sun.jmx:jmxri` that are not available on Maven Central and are not needed by this project. All four are excluded in `pom.xml`.

## Test Dependencies

### JUnit 4.11

Originally `lib/junit-4.11.jar`. Standard test framework. Replaced with the exact same version from Maven Central: `junit:junit:4.11` with `test` scope.

### EvoSuite 0.5.3 (removed)

Originally `lib/evosuite.jar`, a fat JAR containing the full EvoSuite 0.5.3 runtime. This provided the `EvoSuiteRunner` custom JUnit runner and `Sandbox` security manager used in the auto-generated test scaffolding. After removing the EvoSuite scaffolding from all test files (see PATCHLOG.md), no test code references EvoSuite classes, so this dependency was removed entirely.

## JARs Not Needed as Dependencies

### JavaCC (`lib/javacc.jar`)

JavaCC is a parser generator tool. It was used to generate `JVCParser.java`, `JVCParserTokenManager.java`, and related files from a grammar file (`JVCParser.jj`). Since the generated Java source files are already committed in `src/main/java`, JavaCC is not needed at compile time or runtime.

### pmd.jar (`lib/pmd.jar`)

Despite the name, this is not Apache PMD. Inspection of its contents shows it contains pre-compiled `.class` files from this project itself (the `com.pmdesigns.jvc` package). It appears to be a prior build artifact. It is not needed as a dependency.

## Summary

All dependencies were resolved from Maven Central. No system-scoped JARs or local repository installs are required. No dependency versions were changed from the originals, except the servlet API which was updated from 2.3 to 2.5 for Maven Central availability (fully backward compatible).
