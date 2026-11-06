# weasis-i18n

[![License: EPL 2.0](https://img.shields.io/badge/License-EPL%202.0-blue.svg)](https://opensource.org/licenses/EPL-2.0)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Build](https://github.com/nroduit/weasis-i18n/actions/workflows/maven.yml/badge.svg?branch=5.0)](https://github.com/nroduit/weasis-i18n/actions/workflows/maven.yml)

Translations of the [Weasis DICOM viewer](https://github.com/nroduit/Weasis) into several languages.

This project is maintained separately from Weasis so that translations can be added or updated
**without recompiling Weasis**. This branch, `5.0`, builds the translations of Weasis 5; the translations of
Weasis 4.x are built from `master`.

Each module has its own package, `<module>-i18n.jar.xz`: a plain jar of `messages_<code>.properties`, listed
with its version qualifier in `buildNumber.properties`. Weasis 5 caches the packages and publishes them through
the class loader shared by the plugins. The translation texts themselves live on
[Transifex](https://www.transifex.com/weasis/weasis/) and are pulled in at build time.

Before packaging, each module runs `weasis-i18n-dist/src/check/TranslationChecks.java --drop` on its
downloaded `messages_<lang>.properties`: a translation with an invalid `MessageFormat` pattern, a lone
apostrophe in one or a `printf` placeholder is removed, so Weasis shows the English text for that key
instead of a broken one. Until the `weasis-5` Transifex project exists, the translations follow the 4.x
sources (`%s` where 5.0 has `{0}`), so this removes about 1 800 translations; the build lists them.
`weasis-i18n-dist` then checks the whole package with the same rules and fails on any finding (the file is
a copy of the Weasis `tests` module, which also checks the arguments and the `# max:N` limits against the
English sources).

`weasis-launcher-i18n` holds the launcher messages and the Java Swing dialogs (from the
`java-swing-dialogs` Transifex project, under `swing/`), read by the launcher before its first window.

## Branches

| Branch | Package | Weasis |
| --- | --- | --- |
| `master` | `weasis-i18n-dist-4.0.0-SNAPSHOT` | 4.x |
| `5.0` | `weasis-i18n-dist-5.0.0-SNAPSHOT` | 5 |

The daily build of `master` builds and publishes both branches.

## Download

Packages are built automatically once a day and published to the
[mvn-repo](https://github.com/nroduit/mvn-repo) Maven repository. Download the matching package for your
Weasis version:

| Package | Compatible Weasis | Status | Download |
| --- | --- | --- | --- |
| `weasis-i18n-dist-5.0.0-SNAPSHOT` | 5.0 previews and later (branch `5.0`) | Maintained | [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/5.0.0-SNAPSHOT/weasis-i18n-dist-5.0.0-SNAPSHOT.zip) |
| `weasis-i18n-dist-4.0.0-SNAPSHOT` | 4.1.0 – 4.8.x (branch `master`) | Maintained | [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/4.0.0-SNAPSHOT/weasis-i18n-dist-4.0.0-SNAPSHOT.zip) |
| `weasis-i18n-dist-3.0.0-SNAPSHOT` | 3.7.0 – 4.0.3 | No longer updated | [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/3.0.0-SNAPSHOT/weasis-i18n-dist-3.0.0-SNAPSHOT.zip) · [war](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/3.0.0-SNAPSHOT/weasis-i18n.war) |
| `weasis-i18n-dist-2.0.0-SNAPSHOT` | 1.1.x – 3.6.2 | No longer updated | [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/2.0.0-SNAPSHOT/weasis-i18n-dist-2.0.0-SNAPSHOT.zip) · [war](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/2.0.0-SNAPSHOT/weasis-i18n.war) |

See [how to apply the translations to Weasis](https://nroduit.github.io/en/getting-started/translating/#apply-the-translations).

## Contributing translations

Translations are managed on Transifex, not edited directly in this repository. To add or correct a language,
join the [Weasis project on Transifex](https://www.transifex.com/weasis/weasis/) and see the
[internationalization guide](https://nroduit.github.io/en/getting-started/translating/). New translations are
picked up automatically by the next scheduled build.

## Build from source

Prerequisites:

- JDK 21
- Maven 3.6.3 or later
- A [Transifex](https://www.transifex.com/signin) API token (the build downloads the translation files)

```bash
# Build everything and assemble the distribution
mvn -B clean install -Dtransifex.token=<YOUR_TOKEN>
```

The distributable `weasis-i18n.zip` is produced under
`weasis-i18n-dist/target/dist/`.

To build a single language module (and its parent) instead of the full reactor:

```bash
mvn -B clean install -Dtransifex.token=<YOUR_TOKEN> -pl weasis-dicom-rt-i18n -am
```

See the [internationalization page](https://nroduit.github.io/en/getting-started/translating/) for more details.

## License

Dual-licensed under the [Eclipse Public License 2.0](LICENSE.EPL-2.0) and the
[Apache License 2.0](LICENSE.Apache-2.0).