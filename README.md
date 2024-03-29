# weasis-i18n #

[![License](https://img.shields.io/badge/License-EPL%202.0-blue.svg)](https://opensource.org/licenses/EPL-2.0) [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) ![Maven build](https://github.com/nroduit/weasis-i18n/workflows/Build/badge.svg?branch=master)

This project provides a translation into several languages of
the [Weasis DICOM viewer](https://github.com/nroduit/Weasis). It is managed separately and allows
you to add translations without recompiling Weasis.

## Download the binary package ##

Packages are built automatically once a day and can be downloaded below or from a maven dependency.
* weasis-i18n-dist-4.0.0-SNAPSHOT [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/4.0.0-SNAPSHOT/weasis-i18n-dist-4.0.0-SNAPSHOT.zip) or [war](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/4.0.0-SNAPSHOT/weasis-i18n.war): is compatible with Weasis 4.1.0 and upper

Packages for old Weasis versions (no longer updated)
* weasis-i18n-dist-3.0.0-SNAPSHOT [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/3.0.0-SNAPSHOT/weasis-i18n-dist-3.0.0-SNAPSHOT.zip) or [war](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/3.0.0-SNAPSHOT/weasis-i18n.war): is compatible with Weasis 3.7.0 to 4.0.3
* weasis-i18n-dist-2.0.0-SNAPSHOT [zip](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/2.0.0-SNAPSHOT/weasis-i18n-dist-2.0.0-SNAPSHOT.zip) or [war](https://github.com/nroduit/mvn-repo/raw/master/org/weasis/weasis-i18n-dist/2.0.0-SNAPSHOT/weasis-i18n.war): is compatible from Weasis 1.1.x to 3.6.2

See [how to apply the translations to Weasis](https://nroduit.github.io/en/getting-started/translating/#apply-the-translations).

## Build weasis-i18n ##

Prerequisites: JDK 11, Maven 3 and an account at [Transifex](https://www.transifex.com/signin).

See the [internationalization page](https://nroduit.github.io/en/getting-started/translating/) for
building this project.
