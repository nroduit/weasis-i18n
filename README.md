# weasis-i18n

[![License: EPL 2.0](https://img.shields.io/badge/License-EPL%202.0-blue.svg)](https://opensource.org/licenses/EPL-2.0)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Build](https://github.com/nroduit/weasis-i18n/actions/workflows/maven.yml/badge.svg?branch=master)](https://github.com/nroduit/weasis-i18n/actions/workflows/maven.yml)

Translations of the [Weasis DICOM viewer](https://github.com/nroduit/Weasis) into several languages.

This project is maintained separately from Weasis so that translations can be added or updated
**without recompiling Weasis**. Each language pack is packaged as an OSGi *fragment bundle* that attaches
to its matching Weasis bundle at runtime. The translation texts themselves live on
[Transifex](https://www.transifex.com/weasis/weasis/) and are pulled in at build time.

## Download

Packages are built automatically once a day and published to the
[mvn-repo](https://github.com/nroduit/mvn-repo) Maven repository. Download the matching package for your
Weasis version:

| Package | Compatible Weasis | Status | Download |
| --- | --- | --- | --- |
| `weasis-i18n-dist-4.0.0-SNAPSHOT` | 4.1.0 and later | Maintained | [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/4.0.0-SNAPSHOT/weasis-i18n-dist-4.0.0-SNAPSHOT.zip) · [war](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/4.0.0-SNAPSHOT/weasis-i18n.war) |
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

The distributable `weasis-i18n.zip` and `weasis-i18n.war` are produced under
`weasis-i18n-dist/target/dist/`.

To build a single language module (and its parent) instead of the full reactor:

```bash
mvn -B clean install -Dtransifex.token=<YOUR_TOKEN> -pl weasis-dicom-rt-i18n -am
```

See the [internationalization page](https://nroduit.github.io/en/getting-started/translating/) for more details.

## License

Dual-licensed under the [Eclipse Public License 2.0](LICENSE.EPL-2.0) and the
[Apache License 2.0](LICENSE.Apache-2.0).