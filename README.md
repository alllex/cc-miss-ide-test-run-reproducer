# Configuration Cache miss when running tests from IDE

Environment:
- OS: macOS 13.6
- Intellij IDEA 2023.3 EAP (Community Edition) or 2023.2.3 (Ultimate Edition)
- Gradle version: 8.4

Steps to reproduce:
1. Open this project in Intellij IDEA
2. Open the `LibraryTest` file and use the play-button to run the test class from the IDE
3. Check that console output contains `Configuration cache entry stored.` message
4. Rerun the test (without any file changes)

Expected behavior:
* The test run should reuse the configuration cache entry

Actual behavior:
* CC entry is not reused with a message:
```
Calculating task graph as configuration cache cannot be reused because content of 1st init script, '../../../../../private/var/folders/5_/wrppflm15gb11cx18w5pmp8w0000gn/T/ijWrapper42.gradle', has changed.
```

Most likely reason for the issue:
* The Gradle Distribution setting in the IDEA that uses `Wrapper task` instead of the default `Wrapper` value
* This leads to IDEA generating an extra init script that contains unstable paths to temporary files
* This init script is an input to the configuration cache and it changes on every run