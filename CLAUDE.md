# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

This repository contains **no application source code**. It is a Maven multi-module build that downloads
translations for the [Weasis DICOM viewer](https://github.com/nroduit/Weasis) from Transifex, packages each
module's translated `.properties` files as an OSGi **fragment bundle** JAR, and assembles all the bundles into a
distributable `zip`/`war`. The translations attach to a running Weasis install without recompiling Weasis.

There are no tests and no lint step — the "source" is fetched at build time from Transifex.

## Build

Prerequisites: JDK 21 and Maven 3.6.3+ (enforced by `maven-enforcer-plugin`). A Transifex API token is required
to actually fetch translations.

```bash
# Full build: download translations, pack each module, assemble dist
mvn -B clean install -Dtransifex.token=<TOKEN>

# Build a single language module (and parent) without building everything
mvn -B clean install -Dtransifex.token=<TOKEN> -pl weasis-dicom-rt-i18n -am
```

Output: `weasis-i18n-dist/target/dist/weasis-i18n.zip` and `weasis-i18n.war`.

CI (`.github/workflows/maven.yml`) runs this daily, then copies the artifacts into the separate
`nroduit/mvn-repo` GitHub repo and runs `update-repo.py` there to regenerate `maven-metadata.xml` and checksums
(that metadata bump is what makes downstream consumers re-download the snapshot).

## Architecture

### Per-module pipeline
Each `weasis-*-i18n` module mirrors one Weasis bundle and runs the same chain during `package`:
1. **`build-transifex-plugin`** (`buildLanguagePacks` goal) downloads the language `.properties` files for that
   module into `target/classes/${bundle.package}` (e.g. `org/weasis/core`).
2. **`maven-jar-plugin`** packages them as a JAR whose manifest declares it an OSGi fragment via
   `Fragment-Host: ${frament.host}` (the host Weasis bundle), so it merges into the host at runtime.
3. **`build-xz-plugin`** (`packFiles` goal) compresses the JAR to `.jar.xz`.

The two coordinates that define a module are set in its own `pom.xml` properties:
- `bundle.package` — the resource path the translations land in (matches the host bundle's package).
- `frament.host` — the host Weasis bundle + version this fragment attaches to.
(Note the typo `frament.host` is the actual property name used throughout — keep it consistent.)

### Build number propagation
`buildnumber-maven-plugin` generates a timestamp build number. Each module appends a line
`<artifactId><output.pkg>=r<buildNumber>` into the shared `weasis-i18n-dist/src/buildNumber.properties` (via
`maven-antrun-plugin`). The version also goes into each bundle's manifest as `Bundle-Version: ${project.version}.r${buildNumber}`.

### Distribution module (`weasis-i18n-dist`)
Runs last. It concatenates all per-module `src/*.properties` build-number files into `etc/buildNumber.properties`,
then `maven-assembly-plugin` (descriptor `src/assembly/dist.xml`) collects every reactor module's `*.xz` output
plus the `etc/` contents into the final `weasis-i18n.zip` / `weasis-i18n.war`.

### Module set
The active build is the `<modules>` list in the root `pom.xml` (17 `weasis-*-i18n` modules + `weasis-i18n-dist`).
**`weasis-launcher-i18n`, `docking-frames`, and `java-swing-dialogs` exist on disk but are NOT in the parent
module list** — they are not built by the default reactor. Add them to `<modules>` if they need to be built.

## Adding a new translation module
1. Create a `weasis-<name>-i18n/pom.xml` patterned on an existing one (e.g. `weasis-core-i18n/pom.xml`), setting
   `bundle.package` and `frament.host` for the target Weasis bundle.
2. Add the module to `<modules>` in the root `pom.xml`.
The shared plugin config (transifex download, jar manifest, xz packing) is inherited from the parent
`pluginManagement`, so module POMs only re-declare plugins to attach executions.

## Version note
The reactor/parent version is `4.0.0`, but `weasis-i18n-dist` publishes as `4.0.0-SNAPSHOT`. This snapshot is the
one referenced by the README download links and the CI publish path (`org/weasis/weasis-i18n-dist/4.0.0-SNAPSHOT`).