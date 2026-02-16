# PATCHLOG.md

## Project: 32_httpanalyzer

This file documents every change made beyond the standard build configuration.

---

## Patch 1: Remove EvoSuiteRunner from all test files

- The 18 EvoSuite-generated test files in `evosuite-tests/` each contained `import org.evosuite.junit.EvoSuiteRunner` and `@RunWith(EvoSuiteRunner.class)`.
- The class `EvoSuiteRunner` no longer exists in the modern EvoSuite runtime available on Maven Central (version 1.0.6). It was renamed in a later version of EvoSuite.
- The import and annotation were removed from all 18 test files so they could compile and run with the standard JUnit runner.
- No test logic was changed. The tests themselves are identical - only the runner annotation was removed.

## Patch 2: Known failing test - ScreenInputFilterEvoSuiteTest.test1

- `ScreenInputFilterEvoSuiteTest.test1` fails with `expected:<88> but was:<226>`.
- The test feeds a byte value of `-30` (which Java reads as `226` unsigned) into `ScreenInputFilter.read()`. The filter only replaces values below 32 with `88`. Since `226` is not below `32`, the filter correctly returns `226`. The test assertion of `88` is wrong.
- This is a bad assertion in the EvoSuite-generated test itself, not a bug in the application code.
- The test was not modified per project requirements (do not fix bugs unrelated to the build).
- `maven-surefire-plugin` is configured with `testFailureIgnore=true` so the build completes and reports the failure without stopping.

---

## Correction: Ant EvoSuite baseline was recoverable

- It was initially documented that the Ant EvoSuite tests were never runnable due to the missing `../lib/evosuite.jar`.
- The `lib/` folder containing `evosuite.jar` and `junit-4.11.jar` was later found in the original SF110 download. It was intended to live one level above each project as a shared folder.
- The `build.xml` also had two bugs preventing the EvoSuite tests from running even with the JAR present. Both bugs were fixed (see below).
- The Ant EvoSuite tests now compile and run successfully after placing the `lib/` folder at `~/ride_research/lib/` and fixing the `build.xml` bugs.

## Patch 3: Fix bugs in build.xml evosuite-test target

- The `evosuite-test` target had `depends="compile-tests"` which compiled the empty `src/test/java/` folder instead of the EvoSuite tests. Changed to `depends="compile-evosuite"`.
- The `<batchtest>` inside `evosuite-test` searched `${test.java}` (empty folder) instead of `${evosuite.java}` (where the EvoSuite tests live). Changed to `${evosuite.java}`.
- The `evosuite-test` target's runtime classpath was missing `evosuite.lib`. The tests compiled fine (compile-evosuite did include it) but the forked JUnit process could not load `EvoSuiteRunner` at runtime. Added `<path refid="evosuite.lib" />` to the `<classpath>` block inside `evosuite-test`.
- The forked JUnit process also needed `-Djava.awt.headless=true` to prevent GUI tests from hanging when trying to open Swing windows in a headless environment. Added `<jvmarg value="-Djava.awt.headless=true"/>` to the `<junit>` task.
- These were all bugs in the original `build.xml`, not introduced during migration.

## Ant evosuite-test final result

- After all four fixes above, `ant evosuite-test` runs to completion: BUILD SUCCESSFUL.
- 35 tests run across 13 test classes: 34 pass, 1 fails.
- The 1 failure is `ScreenInputFilterEvoSuiteTest.test1` - the same bad EvoSuite assertion documented in Patch 2.
- 5 test files are empty (EvoSuite generated no test methods for those classes). They report "No runnable methods" errors, which is correct behavior from EvoSuiteRunner when handed an empty test class.
