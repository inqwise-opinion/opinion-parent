# Opinion Parent POM

[![CI](https://github.com/inqwise/opinion-parent/actions/workflows/ci.yml/badge.svg)](https://github.com/inqwise/opinion-parent/actions/workflows/ci.yml)
[![Maven Central](https://img.shields.io/maven-central/v/com.inqwise.opinion/opinion-parent.svg?label=Maven%20Central)](https://search.maven.org/search?q=g:%22com.inqwise.opinion%22%20AND%20a:%22opinion-parent%22)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Java Version](https://img.shields.io/badge/Java-17%2B-blue.svg)](https://openjdk.java.net/projects/jdk/17/)
[![Maven Version](https://img.shields.io/badge/Maven-3.6.3%2B-blue.svg)](https://maven.apache.org/)
[![JUnit 5](https://img.shields.io/badge/JUnit-5.10.1-green.svg)](https://junit.org/junit5/)

Maven parent POM for the [Opinion platform](https://github.com/inqwise/opinion) - an open-source questionnaire and survey platform for research, market analysis, and feedback collection.

## Overview

This repository contains the parent POM configuration that serves as the foundation for all Opinion platform modules. It defines:

- **Build Configuration**: Standardized Maven plugin settings and versions
- **Dependency Management**: Centralized version management for common dependencies
- **Quality Enforcement**: Maven Enforcer rules for Java and Maven versions
- **Release Configuration**: Profiles for publishing to Maven Central

## Quick Start

### For Child Projects

Add this parent POM to your project:

```xml
<parent>
    <groupId>com.inqwise.opinion</groupId>
    <artifactId>opinion-parent</artifactId>
    <version>1</version>
</parent>
```

### Building

```bash
# Install the parent POM locally
mvn clean install

# Install without tests
mvn clean install -DskipTests
```

## Requirements

- **Java**: 17+ (development), 21+ (for releases)
- **Maven**: 3.6.3+

## Key Features

### Maven Plugins

- **maven-compiler-plugin** (v3.11.0) - Java 17 compilation
- **maven-enforcer-plugin** (v3.3.0) - Version and dependency validation
- **maven-surefire-plugin** (v3.1.2) - JUnit 5 test execution
- **flatten-maven-plugin** (v1.5.0) - CI-friendly version resolution
- **maven-shade-plugin** (v3.5.0) - Fat JAR creation
- **maven-source-plugin** (v3.3.0) - Source JAR generation

### Maven Profiles

#### `sonatype-oss-release`
For publishing to Maven Central:
```bash
mvn clean install -Psonatype-oss-release
```
- Requires Java 21+
- GPG artifact signing
- Javadoc generation
- Enforces no SNAPSHOT dependencies

#### `display-updates`
Check for dependency and plugin updates:
```bash
mvn compile -Pdisplay-updates
```

## Opinion Platform Modules

This parent POM is inherited by:

- **opinion-library-core** - Core libraries
- **opinion-infrastructure** - Infrastructure layer
- **opinion-app-api** - REST API services
- **opinion-app-admin** - Administration interface
- **opinion-app-site** - User-facing site
- **opinion-cms** - Content management system
- **opinion-automation** - Automation tools
- **opinion-marketplace** - Marketplace functionality
- **opinion-payments** - Payment processing
- And more...

## Related Libraries

The Opinion platform leverages several Inqwise libraries:

- [**inqwise-async**](https://github.com/inqwise/inqwise-async) - Blocking I/O to Vert.x non-blocking bridge
- [**inqwise-error**](https://github.com/inqwise/inqwise-error) - Structured error handling
- [**inqwise-walker**](https://github.com/inqwise/inqwise-walker) - Tree walking utilities
- [**inqwise-difference**](https://github.com/inqwise/inqwise-difference) - Diff utilities

## Development

### Clean Build
```bash
mvn clean
```
Removes:
- `**/*.log` - Log files
- `**/generated/**` - Generated sources
- `**/.flattened-pom.xml` - Flattened POMs
- `**/target/**` - Build artifacts

### Version Management

The project uses `maven-version-rules.xml` to configure the Maven Versions Plugin, which checks for dependency and plugin updates while filtering out unstable versions.

#### Configuration Details

The `maven-version-rules.xml` file defines:
- **Ignore Patterns**: Filters out alpha, beta, RC, and milestone releases
- **Dependency-Specific Rules**: Special handling for Vert.x, Netty, Jackson, and other libraries
- **Stability Focus**: Only suggests stable release versions

#### Usage Examples

```bash
# Check for dependency updates (uses maven-version-rules.xml)
mvn versions:display-dependency-updates

# Check for plugin updates (uses maven-version-rules.xml)
mvn versions:display-plugin-updates

# Use the display-updates profile (runs both checks)
mvn compile -Pdisplay-updates

# Check for property updates
mvn versions:display-property-updates

# Update a specific dependency to latest version
mvn versions:use-latest-versions -Dincludes=groupId:artifactId
```

#### Version Rules Reference

The rules file is referenced via URL in the `display-updates` profile:
```xml
<rulesUri>https://raw.githubusercontent.com/inqwise/opinion-parent/main/maven-version-rules.xml</rulesUri>
```

Child projects automatically inherit these rules when using the parent POM.

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This project is licensed under the [Apache License 2.0](LICENSE).

## Contact

- **Organization**: [inqwise](https://www.inqwise.com)
- **Maintainer**: Alex Misyuk (alex@inqwise.com)
- **Project**: https://github.com/inqwise/opinion

---

**Tags**: `questionnaire`, `survey`, `data-analysis`, `market-research`, `education`, `feedback`
