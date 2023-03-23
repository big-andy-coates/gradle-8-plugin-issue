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
4. 