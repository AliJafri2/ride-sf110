# PATCHLOG.md - Corina (SF110 #35)

## Overview

All changes made during this migration are strictly build-related. No production source code was modified. The application logic, algorithms, and original class files are identical to the original SF110 corpus.

## Moving User Tests to the Test Source Root

The original project contained nine hand-written `UnitTests.java` files scattered throughout `src/main/java` alongside production code. This was a common convention before Maven standardized project layouts. The files existed in the packages `corina`, `corina.cross`, `corina.formats`, `corina.index`, `corina.manip`, `corina.map`, `corina.site`, `corina.ui`, and `corina.util`.

In Maven, JUnit is declared with test scope, which means it is only available on the classpath during test compilation and execution — not during main source compilation. Keeping these files in `src/main/java` caused compilation to fail because the main source phase could not resolve `junit.framework.TestCase` and its assertion methods. Each file was moved to the corresponding package under `src/test/java`, preserving the package declaration and all code exactly as written.

## Copying EvoSuite Tests into the Maven Test Source Root

The EvoSuite-generated tests lived in a separate `evosuite-tests/` directory at the project root. Maven's Surefire plugin only discovers tests under `src/test/java`, so the entire contents of `evosuite-tests/` were copied into `src/test/java` while preserving the package directory structure. The original `evosuite-tests/` directory was left in place and unmodified.

## Stripping EvoSuiteRunner from Generated Tests

Every EvoSuite-generated test file originally contained three things beyond the actual test methods: an `import org.evosuite.junit.EvoSuiteRunner` statement, an `import org.junit.runner.RunWith` statement, a `@RunWith(EvoSuiteRunner.class)` annotation on the class, and an `initEvoSuiteFramework()` setup method annotated with `@BeforeClass` that set EvoSuite properties like `REPLACE_CALLS = true`.

The `EvoSuiteRunner` is a custom JUnit runner that instruments class loading and applies sandboxing at test execution time. When run outside the EvoSuite harness — as is the case with a standard `mvn test` invocation — this runner fails to initialize and throws `ClassNotFoundException`, causing every test in the file to report an initialization error rather than actually running.

The fix was to remove all four of those elements from every test file, allowing the tests to run under the default JUnit 4 runner. A small number of test files also referenced other EvoSuite classes such as `org.evosuite.testcase.CodeUnderTestException` and `org.evosuite.runtime.System` directly within test logic. These references were left in place because the `evosuite.jar` is included as a test-scoped dependency in `pom.xml`, making those classes available at test compile and runtime.

## Java Version Change from 1.6 to 1.8

The original Ant build compiled with `source="1.6"` and `target="1.6"`. The Maven build compiles with source and target set to 1.8, as required by the project guidelines for this migration effort. No language features beyond Java 1.6 were introduced into the source code — the change affects only the compiler target level, not the code itself. All original API usage is preserved as written.

## Creating the Maven Build Files

A `pom.xml` was created from scratch to replace the Ant `build.xml`. A project-local Maven repository was created in the `repo/` directory to serve the 22 JAR files that are not available on Maven Central. This directory is referenced in the `pom.xml` as a `<repository>` with a `file://` URL, so Maven resolves these dependencies automatically without requiring any manual installation steps on a fresh machine.
