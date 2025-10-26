# Opinion Parent POM

[![CI](https://github.com/inqwise/opinion-parent/actions/workflows/ci.yml/badge.svg)](https://github.com/inqwise/opinion-parent/actions/workflows/ci.yml)
[![Maven Central](https://img.shields.io/maven-central/v/com.inqwise.opinion/opinion-parent.svg?label=Maven%20Central)](https://central.sonatype.com/artifact/com.inqwise.opinion/opinion-parent)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

Opinion is an open-source platform for surveys and feedback workflows. This repository hosts the parent Maven POM that keeps every Opinion module aligned on one build, quality, and release process.

## Purpose
- share a single source for build, dependency, and publishing conventions
- enforce quality gates and release checks across Opinion services
- document the minimum setup needed to work on Opinion modules

## Use In Child Projects
Reference the parent from any module and inherit the shared configuration:

```xml
<parent>
  <groupId>com.inqwise.opinion</groupId>
  <artifactId>opinion-parent</artifactId>
  <version><!-- use the latest release --></version>
</parent>
```

## Build & Release Essentials
- install the parent locally: `mvn clean install`
- run the standard test suite: `mvn test`
- surface dependency and plugin updates: `mvn compile -Pdisplay-updates`
- prepare artifacts for Central publishing: `mvn clean install -Psonatype-oss-release`

Use a current Java LTS release for development and the latest LTS (or newer) when publishing.

## More Resources
- Agent-specific guidance lives in `AGENTS.md`
- Release automation details are in the repository CI workflows
- Security contacts and reporting instructions are documented in `SECURITY.md`
