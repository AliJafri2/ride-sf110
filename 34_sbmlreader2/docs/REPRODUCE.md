# How to Reproduce the Build — SBMLReader2 (SF110 #34)

## What you need

- JDK 8 (for example, Eclipse Temurin or AdoptOpenJDK 8)
- Apache Maven 3.6 or newer

## How to build

Clone the repository and run Maven from the project directory:

```bash
git clone <repo-url>
cd 34_sbmlreader2
mvn clean test
```

That's it. All dependencies are either on Maven Central or bundled in the `repo/` directory,
so there are no extra installation steps.

## What to expect

The build will compile 6 source files and run 14 EvoSuite-generated tests across 5 test
classes. 12 tests pass and 2 have pre-existing errors (an XML parse failure and a missing
native library). The build finishes with `BUILD SUCCESS` because `testFailureIgnore` is
enabled in the surefire configuration.

## Additional notes

- The `repo/` directory is a project-local Maven repository. It contains five JAR files that
  are not available on Maven Central. Maven picks them up automatically through a repository
  declaration in the `pom.xml`.
- The build targets Java 1.6 with iso-8859-1 encoding, matching the original Ant configuration.
- The original EvoSuite tests in `evosuite-tests/` are preserved unchanged. The copies in
  `src/test/java/` have the `EvoSuiteRunner` scaffolding removed so they work with Maven
  Surefire (see PATCHLOG.md for details).
