# BUILD_NOTES — JavaViewControl (33_javaviewcontrol)

## Project Overview

JavaViewControl (jvc) is a lightweight Java servlet-based MVC framework by PMDesigns. It dispatches HTTP requests to "generator" controller classes via reflection, using a JavaCC-generated template parser for view rendering. Part of the SF110 corpus.

## Original Ant Build

**Build file:** `build.xml`

**Source/target level:** Java 1.6, encoding `iso-8859-1`

**Directory layout:**
- `src/main/java` — main source code (17 Java files)
- `src/test/java` — empty (no hand-written tests)
- `evosuite-tests/` — 14 auto-generated EvoSuite test files
- `lib/` — runtime and compile-time JARs
- `test-lib/` — empty

**Ant targets:**
- `compile` — compiles main source against `lib/` JARs
- `compile-tests` — compiles `src/test/java` (empty, no-op)
- `compile-evosuite` — compiles EvoSuite tests against main classes + `lib/` JARs
- `evosuite-test` — runs EvoSuite tests with JUnit (was broken, see PATCHLOG.md)
- `jar` — packages compiled classes into `javaviewcontrol.jar`

**Key observations:**
- Source code was already in Maven-standard layout (`src/main/java`)
- No hand-written tests existed
- The `evosuite-test` target had 3 bugs and never ran any tests (fixed in PATCHLOG.md)
- `pmd.jar` in `lib/` was not Apache PMD — it was a pre-compiled copy of this project itself

## Maven Conversion

**Build command:** `mvn clean test`

**Changes made:**
1. Created `pom.xml` with dependencies from Maven Central
2. Copied EvoSuite tests from `evosuite-tests/` into `src/test/java/`
3. Removed `@RunWith(EvoSuiteRunner.class)` and sandbox scaffolding from all test files (incompatible with Maven Surefire — see PATCHLOG.md)
4. Compiler source/target set to `1.8` per project requirements (upgraded from 1.6)
5. Encoding preserved as `iso-8859-1`

**What was NOT changed:**
- No application source code was modified
- No library versions were upgraded (same versions as original JARs)
- No bugs were fixed in application logic

## Test Results

The original Ant build (after fixing the broken `evosuite-test` target) ran 95 tests: 86 passed and 9 failed. The Maven build runs 94 tests: 85 pass and 9 fail. The difference of 1 test is due to removing `test2` from `JVCBootstrapGeneratorEvoSuiteTest`, which only tested EvoSuite's sandbox security manager and not application code. The same 9 parser-related failures exist in both builds and are pre-existing snapshot mismatches documented in PATCHLOG.md.
