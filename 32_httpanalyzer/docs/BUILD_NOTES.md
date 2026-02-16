# BUILD_NOTES.md

## Project: 32_httpanalyzer

---

## Original Ant Build

- The project originally used Apache Ant as its build system.
- Running `ant compile` worked successfully. The only warning was about `includeantruntime` not being set, which is harmless.
- Running `ant test` completed successfully but ran zero tests because `src/test/java/` is empty - this project has no hand-written unit tests.
- Running `ant compile-evosuite` initially failed because the EvoSuite runtime JAR was missing. The `build.xml` expected it at `../lib/evosuite.jar`. The `lib/` folder was later found in the original SF110 download and placed at `~/ride_research/lib/`.
- Running `ant evosuite-test` had four bugs in the original `build.xml` that were all fixed: wrong compile dependency, wrong test directory, missing `evosuite.jar` on the runtime classpath, and missing headless JVM flag. See `PATCHLOG.md` for full details.
- After fixing those bugs, `ant evosuite-test` runs successfully: 35 tests run, 34 passed, 1 known failure (same bad assertion as Maven).

---

## Maven Migration

- The source code was already in the standard Maven layout (`src/main/java/`), so no file restructuring was needed.
- The EvoSuite tests were copied from `evosuite-tests/` to `src/test/java/` so Maven could find and run them.
- A `pom.xml` was created with all dependencies. Most dependencies were found on Maven Central. Three dependencies (`appframework`, `beansbinding`, `swing-worker`) were not available on Maven Central and are referenced directly from the `lib/` folder using system scope.
- The EvoSuite tests were generated with an old version of EvoSuite that used a class called `EvoSuiteRunner`. This class no longer exists in the modern EvoSuite runtime available on Maven Central. The import and annotation were removed from all 18 test files so they could compile and run with the standard JUnit runner.
- This project is a Swing GUI application. When running tests in a headless environment (such as WSL2 or CI), the JVM hangs trying to open GUI windows. The `maven-surefire-plugin` was configured with `-Djava.awt.headless=true` to prevent this.
- Running `mvn clean test` produces 35 tests run, 34 passed, 1 failed. The one failure is in `ScreenInputFilterEvoSuiteTest.test1` and is a pre-existing bad assertion in the EvoSuite-generated test, not a bug introduced by the migration. See `PATCHLOG.md` for details.
- The compiler produces warnings about `sun.misc.BASE64Encoder/Decoder` and deprecated APIs in `HttpAnalyzerView.java`. These are pre-existing issues in the original code and were not touched per project requirements.
