## MultiBit HD Maven

A Maven staging repository for use with automated build tools

To allow Travis builds that use Maven artifacts that are not in Maven Central, provide the following
entry into the parent POM:

    <repositories>
      <!-- Define the MultiBit staging repository for snapshots -->
      <repository>
        <id>mbhd-maven</id>
        <url>https://raw.githubusercontent.com/bitcoin-solutions/mbhd-maven/master/snapshots</url>
        <snapshots/>
      </repository>
    </repositories>

The Travis file should look like this:

    language: java
    jdk:
      - oraclejdk7
      - openjdk7

The project README.md should contain a build status indicator like this:

    Build status: [![Build Status](https://travis-ci.org/bitcoin-solutions/project.png?branch=develop)](https://travis-ci.org/bitcoin-solutions/project)
