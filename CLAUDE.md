# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

This repository contains **no application source code**. It is a Maven multi-module build that downloads
translations for the [Weasis DICOM viewer](https://github.com/nroduit/Weasis) from Transifex, packages each
module's translated `.properties` files as a plain JAR, and assembles all of them into a distributable `zip`.
The translations attach to a running Weasis install without recompiling Weasis.

This is the `5.0` branch (Weasis 5). `master` builds the Weasis 4.x translations as OSGi fragment bundles; changes
here that 4.x cannot read (key renames, new modules, packaging) stay on this branch.

There are no tests and no lint step — the "source" is fetched at build time from Transifex.

## Build

Prerequisites: JDK 25 (this branch; `master` uses 21) and Maven 3.6.3+ (enforced by `maven-enforcer-plugin`). A Transifex API token is required
to actually fetch translations.

```bash
# Full build: download translations, pack each module, assemble dist
mvn -B clean install -Dtransifex.token=<TOKEN>

# Build a single language module (and parent) without building everything
mvn -B clean install -Dtransifex.token=<TOKEN> -pl weasis-dicom-rt-i18n -am
```

Output: `weasis-i18n-dist/target/dist/weasis-i18n.zip`.

CI (`.github/workflows/maven.yml`) runs this daily, then copies the artifacts into the separate
`nroduit/mvn-repo` GitHub repo and runs `update-repo.py` there to regenerate `maven-metadata.xml` and checksums
(that metadata bump is what makes downstream consumers re-download the snapshot).

## Architecture

### Per-module pipeline
Each `weasis-*-i18n` module mirrors one Weasis bundle and runs the same chain during `package`:
1. **`build-transifex-plugin`** 2.x (`buildLanguagePacks` goal) downloads the language `.properties` files for
   that module into `target/classes/${bundle.package}` (e.g. `org/weasis/core`): mode `onlytranslated` (no
   English fill for the untranslated strings), UTF-8, empty languages not written, a failed language fails
   the build. The project is the root property `transifex.project` (`weasis` until `weasis-5` exists);
   `-Dtransifex.skip=true` builds without a token (empty packages).
2. **`TranslationChecks --drop`** (execution `drop-invalid-translations`, `prepare-package`) removes the
   translations that break a rule (`printf` placeholder, invalid pattern, lone apostrophe).
3. **`maven-jar-plugin`** packages them as a plain JAR (no OSGi manifest: Weasis 5 publishes the packages
   through the class loader shared by its plugins).
4. **`build-xz-plugin`** (`packFiles` goal) compresses the JAR to `.jar.xz`.

The coordinate that defines a module is set in its own `pom.xml` properties:
- `bundle.package` — the resource path the translations land in (matches the Weasis module's package).
- `transifex.skipMissingResources` — `true` for a module whose Transifex resource does not exist yet
  (`weasis-dicom-wsi-i18n`): the module is packaged empty instead of failing the build.

### Build number propagation
`buildnumber-maven-plugin` generates a timestamp build number. Each module appends a line
`<artifactId><output.pkg>=r<buildNumber>` into the shared `weasis-i18n-dist/src/buildNumber.properties` (via
`maven-antrun-plugin`). Weasis 5 downloads a package again when this qualifier changes.

### Distribution module (`weasis-i18n-dist`)
Runs last. It concatenates all per-module `src/*.properties` build-number files into `etc/buildNumber.properties`,
then `maven-assembly-plugin` (descriptor `src/assembly/dist.xml`) collects every reactor module's `*.xz` output
plus the `etc/` contents into the final `weasis-i18n.zip` (no war).

### Module set
The active build is the `<modules>` list in the root `pom.xml` (18 `weasis-*-i18n` modules + `weasis-i18n-dist`).
`weasis-launcher-i18n` also downloads the Java Swing dialogs (Transifex project `java-swing-dialogs`) into `swing/`;
read by the launcher before its first window.

## Adding a new translation module
1. Create a `weasis-<name>-i18n/pom.xml` patterned on an existing one (e.g. `weasis-core-i18n/pom.xml`), setting
   `bundle.package` for the target Weasis module. The Transifex resource named after the artifact id must exist.
2. Add the module to `<modules>` in the root `pom.xml`.
The shared plugin config (transifex download, jar manifest, xz packing) is inherited from the parent
`pluginManagement`, so module POMs only re-declare plugins to attach executions.

## Version note
The reactor/parent version is `5.0.0`, but `weasis-i18n-dist` publishes as `5.0.0-SNAPSHOT`, the version Weasis 5
references (`i18n.version` in `weasis-parent/pom.xml`). The CI publish path is derived from the `weasis-i18n-dist`
version.

## Branches
- `master`: Weasis 4.x, publishes `weasis-i18n-dist-4.0.0-SNAPSHOT` (OSGi fragments).
- `5.0`: Weasis 5, publishes `weasis-i18n-dist-5.0.0-SNAPSHOT` (plain jars, no OSGi manifest).
Scheduled workflows only run from the default branch, so `.github/workflows/maven.yml` on `master` builds both
branches through a matrix (one at a time, both push to `nroduit/mvn-repo`). Keep the workflow identical on both
branches.