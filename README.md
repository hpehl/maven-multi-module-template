# Maven Multi Module Template

Template for a multi module project using Maven 3.2.x and Java 11+.

# Features

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
- Support for [keeping a changelog](https://keepachangelog.com/en/1.0.0/)
- Sample code of conduct and contribution guide
- Release profile which generates and signs source and JavaDoc JARs
- Deployment to Maven Central using a GitHub workflow

# Get Started

1. Clone or copy the repo
2. Adjust Maven coordinates in `pom.xml` files
3. Adjust `repo.scm.connection` and `repo.scm.url` in `pom.xml`
4. Adjust organization in `pom.xml`
5. Adjust or remove unnecessary plugins/configuration in `pom.xml`
6. Add dependencies, code, and tests

If you want to keep the contribution guide, make the following adjustments:

- Replace all URLs and paths containing `maven-multi-module-template` in `CONTRIBUTING.md`

# Release Management & Deployment

There are many options for how to release and deploy new versions. This project takes an opinionated approach using a bash script to kick off a new release and a GitHub workflow for doing the actual release and deploying all artifacts to Maven Central.

The project already fulfills all requirements and recommendations for deploying to Maven Central:

- the POM contains all [required metadata](https://central.sonatype.org/publish/requirements/#sufficient-metadata)
- [source and Javadoc JARs](https://central.sonatype.org/publish/requirements/#supply-javadoc-and-sources) are produced
- all artifacts are [signed](https://central.sonatype.org/publish/requirements/#sign-files-with-gpgpgp)
- the [Nexus Staging Maven Plugin](https://central.sonatype.org/publish/publish-maven/#nexus-staging-maven-plugin-for-deployment-and-release) is used for deployment

The project uses https://s01.oss.sonatype.org/ for the deployment to Maven Central. It's configured as property `repo.sonatype.url` in the POM. Please change the URL to your needs.

## Release Script

The script `release.sh` starts a new release. You should make the following adjustments:

- specify your git remotes using the array `GIT_REMOTES`. If your working on a forked repository this should probably be `("origin" "upstream")`
- adjust the variable `WORKFLOW_URL` to your needs

```shell
./release.sh <release-version> <next-version>
```

The release script verifies

- that you don't have uncommitted changes
- that both `release-version` and `next-version` are semantic versions
- that `next-version` is greater than `release-version`
- that no tag `v<release-version>` exists

If everything is fine, the script

1. bumps the project version to `<release-version>`
2. updates the header and links in the changelog (there should already be entries made by you!)
3. commits & pushes the changes
4. creates & pushes a new tag `v<release-version>`
5. bumps to the next snapshot version `<next-version>-SNAPSHOT`
6. commits & pushes changes

By pushing the tag to GitHub, the release workflow kicks in.

## Release Workflow

The release workflow is defined in `release.yml`. It operates fully automated and relies on several secrets that have to be configured:

- `OSSRH_USERNAME`: The username for the Sonatype JIRA
- `OSSRH_PASSWORD`: The password for the Sonatype JIRA
- `MAVEN_GPG_PASSPHRASE`: The passphrase for your private GPG key
- `MAVEN_GPG_PRIVATE_KEY`: The private key in ASCII format. You can use a command like `gpg --armor --export-secret-keys <key-id> | pbcopy` to export and copy the private key to the clipboard (on macOS).

The release workflow builds and deploys the project. Upon successful execution, a new GitHub release is created. The new release is named "Maven Multi Module Template <release-version>.Final". Adjust that to your needs by modifying the value `name` of the step `ncipollo/release-action` in `release.yml`.

# Scripts

This repository contains various scripts to automate tasks.

## `format.sh`

Formats the codebase by applying the following maven goals:

- [`license-maven-plugin:format`](https://mycila.carbou.me/license-maven-plugin/#goals)
- [`formatter-maven-plugin:format`](https://code.revelc.net/formatter-maven-plugin/format-mojo.html)
- [`impsort-maven-plugin:sort`](https://code.revelc.net/impsort-maven-plugin/sort-mojo.html)

The goals use the plugin configuration in [pom.xml](pom.xml) and the resources in [etc](etc).

## `validate.sh`

Validates the codebase by applying the following maven goals:

- [`enforcer:enforce`](https://maven.apache.org/enforcer/maven-enforcer-plugin/enforce-mojo.html)
- [`checkstyle:check`](https://maven.apache.org/plugins/maven-checkstyle-plugin/check-mojo.html)
- [`license-maven-plugin:check`](https://mycila.carbou.me/license-maven-plugin/#goals)
- [`formatter-maven-plugin:validate`](https://code.revelc.net/formatter-maven-plugin/validate-mojo.html)
- [`impsort-maven-plugin:check`](https://code.revelc.net/impsort-maven-plugin/check-mojo.html)

The goals use the plugin configuration in [pom.xml](pom.xml) and the resources in [etc](etc).

## `release.sh`

Starts a new release (see [above](#release-management--deployment)). 
