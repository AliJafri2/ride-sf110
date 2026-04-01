# DEPENDENCIES.md - Corina (SF110 #35)

## Overview

Corina depends on 26 external JAR files plus JUnit and EvoSuite for testing. The original Ant build stored all of these in a `lib/` directory and referenced them via classpath entries. During the Maven migration, each dependency was identified and resolved either from Maven Central or from a project-local repository.

## Dependencies Resolved from Maven Central

Four dependencies were found on Maven Central at the exact versions used by the project.

iText 1.3 (`com.lowagie:itext:1.3`) provides the PDF export functionality. The source code imports `com.lowagie.text.*` classes such as `Document`, `PdfContentByte`, and `PdfWriter` to generate PDF output from the application.

Commons Logging 1.0.4 (`commons-logging:commons-logging:1.0.4`) is the logging abstraction layer. The source imports `org.apache.commons.logging.Log` and `LogFactory` throughout the codebase to produce log output.

Log4j 1.2.8 (`log4j:log4j:1.2.8`) serves as the runtime logging backend. It is not directly imported in the source code but is loaded by Commons Logging at runtime to handle the actual log output.

JUnit 4.11 (`junit:junit:4.11`) is the test framework, declared with test scope. The original Ant build referenced it from a parent `../lib/` directory. The user-written tests use JUnit 3 style (`extends TestCase`), while the EvoSuite tests use JUnit 4 annotations.

## Dependencies Resolved from Project-Local Repository

The remaining dependencies could not be found on Maven Central, either because they were never published there, because the exact legacy versions predate Maven Central, or because they are proprietary Sun/Oracle extensions. These 22 JARs plus the EvoSuite runtime were installed into a file-based Maven repository in the `repo/` directory, which the `pom.xml` references via a `<repository>` element.

JFontChooser (`com.ozten:jfontchooser`) is a small standalone Swing font chooser dialog. It was never published to any public repository. The source imports `com.ozten.font.JFontChooser`.

The Java Communications API (`javax.comm:comm`) is a Sun proprietary extension for serial and parallel port access. It was distributed as part of the Java Communications optional package and was never published to Maven Central. The source imports `javax.comm.*` classes like `CommPortIdentifier` and `SerialPort`.

Smooth Metal (`smooth:smoothmetal`) is a niche Swing Look and Feel theme. It is not imported directly but is loaded at runtime via `UIManager.setLookAndFeel("smooth.windows.SmoothLookAndFeel")`.

edtFTPj (`com.enterprisedt:edtftpj`) is an FTP client library by Enterprise Distributed Technologies. The version bundled with the project is very old (circa 2002) and predates its Maven Central availability. The source imports `com.enterprisedt.net.ftp.*`.

RXTX (`org.rxtx:rxtxcomm`) provides serial communication via the `gnu.io.*` package. It requires native libraries and the old bundled version does not match any Maven Central artifact cleanly. The source imports `gnu.io.*` and also loads `gnu.io.RXTXCommDriver` via `Class.forName` at runtime.

Apache Batik (`org.apache.batik:batik-all`) is an SVG toolkit. The original JAR is a single bundled file from 2002, predating the modern split into separate Maven artifacts (`batik-svggen`, `batik-dom`, etc.). The source imports `org.apache.batik.dom.GenericDOMImplementation` and `org.apache.batik.svggen.SVGGraphics2D`.

Apache Avalon (`org.apache:avalon-framework`) is a legacy application framework that served as a transitive dependency for Apache FOP. It is not directly imported in the source code.

Apache FOP (`org.apache.fop:fop`) is a formatting objects processor. Like Batik, the bundled version is a single old JAR that does not correspond to any Maven Central release. It is not directly imported in the source.

JFor (`net.sf.jfor:jfor`) is an XSL-FO to RTF converter. It was never published to Maven Central and is not directly imported in the source.

Apache Crimson (`crimson:crimson`) and GNU JAXP (`gnu:gnujaxp`) are legacy XML parsers that predate the XML support built into the JDK. Neither is directly imported, as the JDK provides `org.w3c.dom` and `org.xml.sax`.

Apache BCEL (`org.apache.bcel:bcel`) is the Byte Code Engineering Library. The bundled version is old and is not directly imported in the source code.

BeanShell (`bsh:bsh`) is a Java scripting interpreter. It is not directly imported in the source.

FindBugs (`com.google.code.findbugs:findbugs`) is a static analysis tool. The bundled version is an old standalone JAR, likely used as a build-time tool in the original development workflow. It is not imported in the source.

JavaHelp is represented by two JARs. The `jh.jar` (`javax.help:javahelp-jh`) and `jhall.jar` (`javax.help:javahelp`) together provide the JavaHelp system, a Sun proprietary framework for building help systems in Java applications. The source imports `javax.help.HelpBroker` and `javax.help.HelpSet`.

JLFGR (`com.sun:jlfgr`) is the Java Look and Feel Graphics Repository, a collection of toolbar and menu icons from Sun. It contains only image resources, no code, and is not imported in the source.

Jython (`org.python:jython`) is a Python interpreter for the JVM. Kawa (`kawa:kawa`) is a Scheme implementation for the JVM. Neither is directly imported in the source code; they were likely used for scripting extensibility.

Saxon (`net.sf.saxon:saxon`) and Xalan (`xalan:xalan`) are XSLT processors. The bundled versions are old single-JAR distributions. Neither is directly imported in the source.

PostgreSQL JDBC (`postgresql:postgresql`) and HSQLDB (`hsqldb:hsqldb`) are database drivers. They are not imported via `import` statements but are loaded dynamically at runtime using `Class.forName("org.postgresql.Driver")` and `Class.forName("org.hsqldb.jdbcDriver")` in several places throughout the code.

EvoSuite (`org.evosuite:evosuite`) provides the test runtime needed by the EvoSuite-generated tests. It is declared with test scope. A few tests reference classes like `org.evosuite.testcase.CodeUnderTestException` and `org.evosuite.runtime.System` from this JAR.

## Directly Imported External Packages

To summarize, the following external packages appear in `import` statements in the production source code: `com.enterprisedt.net.ftp.*` from ftp.jar, `com.lowagie.text.*` from itext-1.3.jar, `com.ozten.font.JFontChooser` from JFontChooser.jar, `gnu.io.*` from RXTXcomm.jar, `javax.comm.*` from comm.jar, `javax.help.*` from jhall.jar, `org.apache.batik.*` from batik.jar, and `org.apache.commons.logging.*` from commons-logging.jar. All other lib JARs are either loaded at runtime via reflection, used as transitive dependencies, or were part of the original development toolchain without being referenced in the compiled source.
