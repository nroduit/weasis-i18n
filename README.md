# weasis-i18n

[![License: EPL 2.0](https://img.shields.io/badge/License-EPL%202.0-blue.svg)](https://opensource.org/licenses/EPL-2.0)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

Translations for the [Weasis DICOM viewer](https://github.com/nroduit/Weasis). Maintained as a separate project so translations can be added or updated without recompiling Weasis.

## Download

Packages are rebuilt automatically once a day and can be consumed either as a binary download or as a Maven dependency.

**Current** — compatible with Weasis 4.1.0 and later:
- [`weasis-i18n-dist-4.0.0-SNAPSHOT.zip`](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/4.0.0-SNAPSHOT/weasis-i18n-dist-4.0.0-SNAPSHOT.zip)
- [`weasis-i18n.war`](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/4.0.0-SNAPSHOT/weasis-i18n.war)

**Legacy** (no longer updated):
- 3.0.0-SNAPSHOT for Weasis 3.7.0 – 4.0.3 — [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/3.0.0-SNAPSHOT/weasis-i18n-dist-3.0.0-SNAPSHOT.zip) · [war](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/3.0.0-SNAPSHOT/weasis-i18n.war)
- 2.0.0-SNAPSHOT for Weasis 1.1.x – 3.6.2 — [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/2.0.0-SNAPSHOT/weasis-i18n-dist-2.0.0-SNAPSHOT.zip) · [war](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/2.0.0-SNAPSHOT/weasis-i18n.war)

See [how to apply the translations to Weasis](https://nroduit.github.io/en/getting-started/translating/#apply-the-translations).

## Build

Prerequisites: JDK 21, Maven 3.6.3+ and a [Transifex](https://www.transifex.com/signin) API token.

```sh
mvn clean install -Dtransifex.token=<your-token>
```

The build downloads the latest translations from Transifex and packages each Weasis module as an OSGi language fragment. See the [internationalization page](https://nroduit.github.io/en/getting-started/translating/) for the full documentation.

