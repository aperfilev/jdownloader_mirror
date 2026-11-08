# Maven Setup for JDownloader Mirror

This workspace contains Maven POMs for the four SVN dumps:

- `svn_utils` – AppWork utilities library
- `svn_browser` – JD Browser components
- `svn_MyJDownloaderClient` – MyJDownloader client
- `svn_trunk` – JDownloader2 desktop application

## Build

From the workspace root:

```bash
mvn -T 1C clean install
```

This compiles all modules in dependency order:
`svn_utils` → `svn_browser` → `svn_MyJDownloaderClient` → `svn_trunk`.

## Run

After a successful build:

```bash
mvn -pl svn_trunk exec:java -Dexec.mainClass="org.jdownloader.launcher.JDLauncher"
```

Or run the packaged JAR:

```bash
java -jar svn_trunk/target/svn-trunk-1.0-SNAPSHOT.jar
```

### Notes

* The original project is Eclipse-based with Java 1.6 sources and many third-party jars under `svn_trunk/ressourcen/libs`.
  The Maven POMs above set up source roots and inter-module dependencies. For a fully self-contained build you will need to:
  1. Add the jars from `ressourcen/libs`, `ressourcen/libs_ext`, `ressourcen/libs/laf`, etc. as Maven dependencies or as system-scoped files.
  2. Copy resource folders `ressourcen`, `translations`, `themes` into `src/main/resources` or configure additional resources.
  3. The project uses several non-standard source roots (`src-closed`, `translations`, `themes`). `build-helper-maven-plugin` is used for `svn_trunk`.

* Java 1.6 is required for source/target compatibility. Modern JDKs may need `--release 6` flags or toolchain configuration.

* The Eclipse `.classpath` references local projects `/JDBrowser`, `/MyJDownloaderClient`, `/AppWorkUtils`. These are mapped to the Maven modules above.

* This setup provides a starting point for compilation and execution. Further dependency resolution for third-party libraries may be required for a clean build.
