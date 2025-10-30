# First Gradle Project

Small example Java project built with Gradle.

This README explains how to build and run the project, what is configured in `build.gradle` (including the JAR manifest `Main-Class`), and a few troubleshooting tips.

## Project structure (relevant parts)

- `build.gradle` - Gradle build script (configures `jar` manifest to set the main class).
- `gradlew`, `gradlew.bat` - Gradle wrapper (use these to run Gradle without installing Gradle globally).
- `src/main/java/org/example/Main.java` - The project's `Main` class (entry point).
- `build/` - Generated build outputs (created after running the build).

## Prerequisites

- Java Development Kit (JDK) 8 or later installed and `java`/`javac` available on PATH.
- On Windows use the included wrapper: `gradlew.bat` (no Gradle install required).

## Quick start (Windows - cmd.exe)

Open a command prompt in the project root (`c:\Users\rajgu\Codes\First-Gradle-project`) and run:

Build the project and run tests:

```cmd
gradlew.bat build
```

This will compile sources, run tests (JUnit 5 is configured) and produce a JAR under `build\libs`.

Build only the JAR (optional):

```cmd
gradlew.bat jar
```

Run the generated JAR:

```cmd
java -jar build\libs\First-Gradle-project-1.0-SNAPSHOT.jar
```

Alternatively, run the `Main` class directly from compiled classes (no JAR required):

```cmd
java -cp build\classes\java\main org.example.Main
```

(Adjust paths if you changed the project group/name or version.)

## About `build.gradle` and the JAR manifest

This project uses the Java plugin and configures the JAR manifest to include a `Main-Class` attribute. That tells the Java launcher which class has the `public static void main(String[] args)` method to start the application.

Key excerpt from `build.gradle`:

- `plugins { id 'java' }` — applies the Java plugin.
- `repositories { mavenCentral() }` — standard central repository for dependencies.
- `dependencies { testImplementation 'org.junit.jupiter:junit-jupiter' }` — JUnit 5 for tests.
- `jar { manifest { attributes( 'Main-Class': 'org.example.Main' ) } }` — writes `Main-Class: org.example.Main` into the generated JAR manifest.

Because the manifest contains `Main-Class: org.example.Main`, running the JAR with `java -jar ...` will invoke `org.example.Main.main(...)` automatically.

If you change the package or class name of the entry point, update the `Main-Class` value in `build.gradle` accordingly. Example:

```groovy
jar {
    manifest {
        attributes('Main-Class': 'com.myorg.App')
    }
}
```

Then rebuild the JAR.

## Running with Gradle (alternative)

If you prefer to run the main class via Gradle, you can add the `application` plugin and configure the `mainClass` (not currently included in this project):

```groovy
plugins {
    id 'application'
}

application {
    mainClass = 'org.example.Main'
}
```

Then run:

```cmd
gradlew.bat run
```

## Tests

This project uses JUnit 5 (configured via the BOM in `build.gradle`). Run tests with:

```cmd
gradlew.bat test
```

Test reports will be available under `build\reports\tests`.

## Common troubleshooting

- Ignored files still tracked by Git: updating `.gitignore` does not remove files already tracked. To stop tracking a file or folder (for example `.idea/`), run:

```cmd
git rm -r --cached .idea
git commit -m "Stop tracking .idea"
git push
```

Or to refresh the index for all ignored files:

```cmd
git rm -r --cached .
git add .
git commit -m "Apply .gitignore"
```

- If `java -jar` reports `no main manifest attribute`, ensure you rebuilt the JAR after setting `Main-Class` in `build.gradle` (`gradlew.bat clean jar` then re-run).

- If Gradle fails to download dependencies, check your network/proxy settings or run the build again; the Gradle wrapper will download the Gradle distribution the first time it runs.

## Where to change project metadata

- `group` and `version` are set in `build.gradle`. Changing them affects the JAR coordinates and the output filename.

## License

This README is provided as-is. Add a license file if you want to publish or share the project.

---

If you'd like, I can also:
- Add an `application` plugin to `build.gradle` to enable `gradlew run`.
- Add a `gitignore` file that includes `.idea/` and typical Java/Gradle ignores.
- Add a small README badge or CI instructions.

Tell me which of those you'd like and I'll make the changes.
