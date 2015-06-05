## MultiBit HD Maven

A Maven staging repository for use with automated build tools. This allows for faster releaes cycles
that do not depend on Maven Central.

The intention is that released artifacts from here will be migrated to Maven Central if they are not
forks.

## DO NOT RELY ON SNAPSHOTS FOR PRODUCTION !

These snapshots are not actively maintained beyond what Bitcoin Solutions developers need for their
immediate purposes. There is no guarantee which branch they were lifted from or if they are a mangled
fork of the upstream original.

**Always use the final releases from the official upstream repositories in your own code.**

You can rely on the releases, but again it is better to use official upstream repositories for projects
that are forks.

### Configuring your downstream projects to use this repo

To allow Travis builds that use Maven artifacts that are not in Maven Central, provide the following
entry into your parent POM:

```xml
<repositories>
    <!-- Define the MultiBit staging repository for releases and snapshots -->
    <!-- Use https://raw.github.com to provide a consistent result -->
    <repository>
      <id>mbhd-maven-release</id>
      <url>https://raw.github.com/bitcoin-solutions/mbhd-maven/master/releases</url>
      <releases/>
    </repository>
    <repository>
      <id>mbhd-maven-snapshot</id>
      <url>https://raw.github.com/bitcoin-solutions/mbhd-maven/master/snapshots</url>
      <!-- These artifacts change frequently during development iterations -->
      <snapshots>
        <updatePolicy>always</updatePolicy>
      </snapshots>
    </repository>

  </repositories>
```

Your Travis file `.travis.yml` should look like this:

```
language: java
jdk:
  - oraclejdk7
  - openjdk7
```

Your project `README.md` should contain a build status indicator like this (for a repo referenced as `bitcoin-solutions/project`):

```
Build status: [![Build Status](https://travis-ci.org/bitcoin-solutions/project.png?branch=develop)](https://travis-ci.org/bitcoin-solutions/project)
```

### Bitcoin Solutions staff only: Use Ant to quickly update artifacts

Only Bitcoin Solutions staff with commit access need to do this.

First create a `build.properties` file representing your own repo:

    repo=/var/maven/repo

Build the project with `mvn -DcreateChecksum=true clean install` and ensure it succeeds. Ideally it should have sources
and javadocs provided.

Then run the default target for the `build.xml`, for example:

    $ ant

This will then copy all the relevant artifacts into this repo. Then do the usual git commit and push.
