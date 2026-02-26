# PATCHLOG — JavaViewControl (33_javaviewcontrol)

All code and configuration changes made beyond the Maven migration itself.

---

## 1. Fix `evosuite-test` target in `build.xml`

The original `evosuite-test` Ant target was non-functional and never actually ran any tests. It had three bugs: it depended on `compile-tests` instead of `compile-evosuite`, it scanned the empty `src/test/java` directory instead of `evosuite-tests/`, and it was missing `evosuite.jar` from the classpath. All three were corrected. No application logic was changed.

---

## 2. Remove EvoSuite runner scaffolding from test files

Every EvoSuite test class used `@RunWith(EvoSuiteRunner.class)`, a custom JUnit runner from EvoSuite 0.5.3. This runner's classloader is incompatible with Maven Surefire, causing all tests to fail with `ClassNotFoundException` at startup. The annotation and its imports were removed from all 14 test files. The actual test methods are standard JUnit 4 and run correctly without the custom runner.

`JVCBootstrapGeneratorEvoSuiteTest.java` also had EvoSuite sandbox setup/teardown methods and a test (`test2`) that only verified the sandbox security manager blocked file writes. Since the sandbox was removed, `test2` was also removed as it had no meaning without it. No other test logic was changed.

After this change, Maven runs 94 tests: 85 pass and 9 fail.

---

## Pre-Existing Test Failures (Not Fixed)

9 tests fail in both the Ant and Maven builds. All are in `JVCParserEvoSuiteTest` (6 failures) and `JVCParserTokenManagerEvoSuiteTest` (3 failures). These tests were auto-generated against a prior compiled version of the project (found in `pmd.jar`) that differs slightly from the current source code. The assertions capture exact implementation details like whitespace handling, buffer sizes, and token values that no longer match. These are snapshot mismatches, not application bugs, and were not fixed per project guidelines.
