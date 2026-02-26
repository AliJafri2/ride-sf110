# REPRODUCE — JavaViewControl (33_javaviewcontrol)

## Prerequisites

- **JDK 8** (OpenJDK or Oracle JDK)
- **Apache Maven 3.6+**
- **Git**

## Steps to Build

```bash
# 1. Clone the repository
git clone <repository-url>
cd 33_javaviewcontrol

# 2. Build and run tests
mvn clean test
```

That's it. All dependencies are resolved from Maven Central automatically.

## Expected Output

- **BUILD SUCCESS**
- 94 tests run, 85 pass, 9 fail
- The 9 failures are pre-existing (documented in docs/PATCHLOG.md) and do not cause a build failure (`testFailureIgnore=true` in surefire config)

## Verification

To verify the build produces a JAR:

```bash
mvn clean package
```

This creates `target/javaviewcontrol-1.0-SNAPSHOT.jar`.

## Notes

- No IDE-specific configuration is required
- No hard-coded absolute paths
- No environment variables need to be set
- Source encoding is `iso-8859-1` (configured in `pom.xml`)
