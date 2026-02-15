# REPRODUCE.md

## Project: 32_httpanalyzer

Exact steps to build and test this project on a clean machine.

---

## Prerequisites

- JDK 8 installed (`sudo apt install openjdk-8-jdk`)
- Maven 3.x installed (`sudo apt install maven`)
- Git installed (`sudo apt install git`)

---

## Steps

1. Clone the repository and navigate into the project folder.

2. Run the following command to build and test:
   ```
   mvn clean test
   ```

3. Maven will automatically download all dependencies from Maven Central on first run. The three dependencies not available on Maven Central (`appframework`, `beansbinding`, `swing-worker`) are included in the `lib/` folder in the repository.

---

## Expected Output

- BUILD SUCCESS
- 35 tests run, 34 passed, 1 failure
- The one failure (`ScreenInputFilterEvoSuiteTest.test1`) is a pre-existing bad assertion in the EvoSuite-generated test. See `PATCHLOG.md` for details.

---

## Notes

- The `-Djava.awt.headless=true` flag is already configured in `pom.xml` and does not need to be passed manually. It prevents the JVM from hanging when trying to open GUI windows in headless environments.
- No IDE setup is required. The project builds entirely from the command line.
- No absolute paths or machine-specific configuration is used.
