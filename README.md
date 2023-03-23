# Repo for demonstrating issue with `com.gradle.plugin-publish` plugin with Gradle 8.0.2

## Steps to recreate initial repository:

1. With Gradle v7.5.1 installed run `gradle init` in an empty directory, pick options:
    1. Select type of project to generate: `4` (Gradle Plugin)
    2. Select implementation language: `2` (Java)
    3. Select build script DSL: `2` (Kotlin)
    4. Generate build using new APIs and behavior (some features may change in the next minor release)? (default: no) [yes, no]: `no`

2. Add the `com.gradle.plugin-publish` plugin to `build.gradle.kts`:
    ```kotlin
    id("com.gradle.plugin-publish") version "1.1.0"
    ```
3. Configure the `pluginBundle`:
    ```kotlin
    pluginBundle {
        website = "https://www.creekservice.org/${rootProject.name}/"
        vcsUrl = "https://github.com/creek-service/${rootProject.name}"
    
        tags = listOf("creek", "creekservice")
    }
    ```
4. Confirm build passed by running: 
   ```
   ./gradlew build
   ```
   
## Steps to highlight issue:

1. Clone the repo locally and ensure it builds
2. Upgrade Gradle to `8.0.2` by running:
   ```
   ./gradlew wrapper --gradle-version 8.0.2
   ```
3. Run the build:
   ```
   ./gradlew build
   ```
4. Build fails with the error below:

```
> Configure project :plugin
e: /Users/andy/dev/github.com/big-andy-coates/gradle-8-plugin-issue/plugin/build.gradle.kts:58:1: Unresolved reference: pluginBundle
e: /Users/andy/dev/github.com/big-andy-coates/gradle-8-plugin-issue/plugin/build.gradle.kts:59:5: Unresolved reference: website
e: /Users/andy/dev/github.com/big-andy-coates/gradle-8-plugin-issue/plugin/build.gradle.kts:60:5: Unresolved reference: vcsUrl
e: /Users/andy/dev/github.com/big-andy-coates/gradle-8-plugin-issue/plugin/build.gradle.kts:62:5: Unresolved reference: tags

FAILURE: Build failed with an exception.

* Where:
Build file '/Users/andy/dev/github.com/big-andy-coates/gradle-8-plugin-issue/plugin/build.gradle.kts' line: 58

* What went wrong:
Script compilation errors:

  Line 58: pluginBundle {
           ^ Unresolved reference: pluginBundle

  Line 59:     website = "https://www.creekservice.org/${rootProject.name}/"
               ^ Unresolved reference: website

  Line 60:     vcsUrl = "https://github.com/creek-service/${rootProject.name}"
               ^ Unresolved reference: vcsUrl

  Line 62:     tags = listOf("creek", "creekservice")
               ^ Unresolved reference: tags

4 errors

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.

* Get more help at https://help.gradle.org
```