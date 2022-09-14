# Maven Multi Module Template

Template for a multi module project using Maven 3.2.x and Java 11+.

## Features

- [Maven Wrapper](https://maven.apache.org/wrapper/)
- Inherits from [JBoss Parent POM](https://github.com/jboss/jboss-parent-pom)
- Multi module:
  - Parent POM with basic info, properties and minimal plugin definitions
  - Build config containing checkstyle rules, formatter definition and license template
  - BOM declaring project dependencies
  - Parent code module with dependency and plugin management  
  - Code module with some dummy code
- [WildFly](https://github.com/wildfly/wildfly-checkstyle-config) [checkstyle](https://checkstyle.sourceforge.io/) configuration
- [Maven Enforcer Plugin](https://maven.apache.org/enforcer/maven-enforcer-plugin/) with rules enforcing
  - secure repositories over HTTPS
  - Java >=11
  - Maven >= 3.2.5
- [Maven License Plugin](https://mycila.carbou.me/license-maven-plugin/)
- [Maven Formatter Plugin](https://code.revelc.net/formatter-maven-plugin/)
- [Maven ImpSort Plugin](https://code.revelc.net/impsort-maven-plugin/)
- Sample dependencies [Eclipse Collections](https://www.eclipse.org/collections/) and [JUnit 5](https://junit.org/junit5/)
- [SLF4J](https://www.slf4j.org/) and [Logback](https://logback.qos.ch/) support
- Empty JUnit 5 unit test
- Release profile which generates and signs source and JavaDoc JARs

## Get Started

1. Clone or copy repo
2. Adjust Maven coordinates in `pom.xml` files
3. Adjust `repo.scm.connection` and `repo.scm.url` in `pom.xml`
6. Adjust or remove unnecessary plugins / configuration in `pom.xml`
5. Add dependencies, code and tests

## Scripts

This repository contains various scripts to automate tasks.

### `format.sh`

Formats the codebase by applying the following maven goals:

- [`license-maven-plugin:format`](https://mycila.carbou.me/license-maven-plugin/#goals)
- [`formatter-maven-plugin:format`](https://code.revelc.net/formatter-maven-plugin/format-mojo.html)
- [`impsort-maven-plugin:sort`](https://code.revelc.net/impsort-maven-plugin/sort-mojo.html)

The goals use the plugin configuration in [code-parent/pom.xml](code-parent/pom.xml#L84) and the resources in [build-config/src/main/resources/etc](build-config/src/main/resources/etc).  

### `validate.sh`

Validates the codebase by applying the following maven goals:

- [`enforcer:enforce`](https://maven.apache.org/enforcer/maven-enforcer-plugin/enforce-mojo.html)
- [`checkstyle:check`](https://maven.apache.org/plugins/maven-checkstyle-plugin/check-mojo.html)
- [`license-maven-plugin:check`](https://mycila.carbou.me/license-maven-plugin/#goals)
- [`formatter-maven-plugin:validate`](https://code.revelc.net/formatter-maven-plugin/validate-mojo.html)
- [`impsort-maven-plugin:check`](https://code.revelc.net/impsort-maven-plugin/check-mojo.html)

The goals use the plugin configuration in [code-parent/pom.xml](code-parent/pom.xml#L84) and the resources in [build-config/src/main/resources/etc](build-config/src/main/resources/etc).  
