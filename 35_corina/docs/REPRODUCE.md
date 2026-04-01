# REPRODUCE.md - Corina (SF110 #35)

## Prerequisites

You will need JDK 8 installed and available on your `PATH`. The project is configured to compile against Java 8 source and target, and using a newer JDK may cause compatibility issues with the legacy code. You will also need Apache Maven 3.6 or later. No other tools or environment setup is required — all dependencies are bundled in the repository.

## Building and Running Tests

Clone the repository and navigate to the `35_corina` directory. Then run:

```bash
mvn clean test
```

Maven will resolve all dependencies automatically. Four dependencies are downloaded from Maven Central (iText, Commons Logging, Log4j, and JUnit). The remaining 22 library JARs are served from the project-local `repo/` directory and require no internet access. The build compiles the main source, compiles the tests, and then runs all 312 tests via the Surefire plugin.

## Expected Output

Compilation will succeed with no errors. The test run will report some failures and errors — specifically 16 failures and 27 errors out of 312 tests — but the overall build result will be `BUILD SUCCESS`. These failures are pre-existing issues in the EvoSuite-generated tests caused by GUI components that require a display, filesystem path assumptions, and network dependencies. They are not caused by the Maven migration. The Surefire plugin is configured with `testFailureIgnore=true` to allow the build to succeed despite them.

## Packaging

To produce a JAR file without running tests, run:

```bash
mvn clean package -DskipTests
```

This produces `target/corina-1.0-SNAPSHOT.jar` containing all compiled main classes and resources.

## Suppressing GUI Popups

Because Corina is a Swing application, some EvoSuite tests instantiate real GUI components during their execution. On a desktop environment this causes visible windows to briefly appear. To suppress this, pass the headless flag:

```bash
mvn clean test -Djava.awt.headless=true
```

Note that this may cause a small number of additional test failures for tests that genuinely require a display, but it will prevent popup windows from appearing. In CI environments such as GitHub Actions, the build runs headless automatically.
