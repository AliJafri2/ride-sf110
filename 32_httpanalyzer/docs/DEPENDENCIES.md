# DEPENDENCIES.md

## Project: 32_httpanalyzer

This file lists all external dependencies used by the project, where they came from, and how they were resolved in the Maven build.

---

## Main Dependencies (required to compile and run the application)

- **httpclient 4.0.1** - Apache library for making HTTP requests. Found on Maven Central.
- **httpcore 4.0.1** - Core HTTP transport library used by httpclient. Found on Maven Central.
- **httpmime 4.0.1** - Apache library for handling MIME multipart HTTP requests. Found on Maven Central.
- **apache-mime4j 0.6** - Apache library for parsing MIME messages. Found on Maven Central.
- **commons-codec 1.3** - Apache library for encoding and decoding data (Base64, hex, etc.). Found on Maven Central.
- **commons-logging 1.1.1** - Apache logging abstraction library. Found on Maven Central.
- **appframework 1.0.3** - JDesktop Application Framework for building Swing desktop apps. Not available on Maven Central. Referenced directly from `lib/appframework-1.0.3.jar` using system scope.
- **beansbinding 1.2.1** - JDesktop library for binding UI components to data. Not available on Maven Central. Referenced directly from `lib/beansbinding-1.2.1.jar` using system scope.
- **swing-worker 1.1** - Background task library for Swing applications. Not available on Maven Central. Referenced directly from `lib/swing-worker-1.1.jar` using system scope.

---

## Test Dependencies (only required to compile and run tests)

- **junit 4.13.2** - The JUnit testing framework used to run all EvoSuite-generated tests. Found on Maven Central.
- **evosuite-standalone-runtime 1.0.6** - The EvoSuite runtime library required to support EvoSuite-generated tests. Found on Maven Central. Note: this is a newer version than what originally generated the tests. See `PATCHLOG.md` for the compatibility fix that was needed.

---

## Dependencies Not on Maven Central

Three dependencies (`appframework`, `beansbinding`, `swing-worker`) could not be found on Maven Central. These are old JDesktop/Sun libraries that predate Maven Central's widespread adoption and were never published there. They are included in the `lib/` folder which is committed to the repository, so the build remains reproducible on a clean machine as long as the repository is cloned in full.
