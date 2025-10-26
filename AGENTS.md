# AGENTS.md

This file provides guidance to AI agents (Warp, Cursor, Copilot, Claude, etc.) when working with code in this repository.

## Project Overview

**opinion-parent** is a Maven parent POM for the Opinion platform - an open-source questionnaire and survey platform. This repository defines the build configuration, dependency management, and plugin settings inherited by all Opinion project modules.

- **Group ID**: `com.inqwise.opinion`
- **Artifact ID**: `opinion-parent`
- **Java Version**: 17 (development), 21+ (releases)
- **Maven Version**: 3.6.3+
- **Testing Framework**: JUnit 5

## Essential Maven Commands

### Building & Testing
```bash
# Clean and install the parent POM
mvn clean install

# Install without running tests
mvn clean install -DskipTests

# Run tests only
mvn test

# Run a single test
mvn test -Dtest=ClassName#methodName

# Build with Maven Enforcer checks
mvn clean compile
```

### Dependency Management
```bash
# Display dependency updates (using display-updates profile)
mvn compile -Pdisplay-updates

# Display plugin updates
mvn versions:display-plugin-updates

# Check for dependency conflicts
mvn dependency:tree
```

### Release & Deployment
```bash
# Build for Sonatype release
mvn clean install -Psonatype-oss-release

# Verify release (requires Java 21+)
mvn verify -Psonatype-oss-release
```

### Cleanup
```bash
# Clean all generated files (including logs and .flattened-pom.xml)
mvn clean
```

## Architecture & Build Configuration

### Parent POM Structure

This is a **parent POM only** - it contains no source code, only build configuration. It serves as the foundation for all Opinion platform modules including:

- `opinion-library-core` - Core libraries
- `opinion-infrastructure` - Infrastructure layer
- `opinion-app-api` - API services
- `opinion-app-admin` - Administration module
- `opinion-app-site` - User-facing site
- `opinion-cms` - Content management
- `opinion-automation` - Automation tools
- `opinion-marketplace`, `opinion-payments`, etc.

### Key Maven Plugins

1. **maven-compiler-plugin** (v3.14.1)
   - Compiles for Java 17
   - Generated sources: `src/main/generated`, `src/test/generated`

2. **maven-enforcer-plugin** (v3.6.2)
   - Enforces Maven 3.6.3+ and Java 17+
   - Prevents duplicate dependencies
   - Release profile enforces Java 21+

3. **maven-surefire-plugin** (v3.5.4)
   - Runs JUnit 5 tests

4. **flatten-maven-plugin** (v1.5.0)
   - Resolves CI-friendly version properties
   - Creates `.flattened-pom.xml` during build

5. **maven-shade-plugin** (v3.5.0)
   - Creates fat JARs with `-fat.jar` suffix
   - Configured in pluginManagement for child projects

6. **maven-source-plugin** (v3.3.1)
   - Attaches source JARs during build

### Maven Profiles

#### `sonatype-oss-release`
Used for publishing to Maven Central:
- Requires Java 21+
- Enforces no SNAPSHOT dependencies
- Signs artifacts with GPG
- Generates Javadoc
- Uses central-publishing-maven-plugin (v0.9.0)

#### `display-updates`
Displays available dependency and plugin updates:
- Follows version rules from `maven-version-rules.xml`
- Ignores alpha, beta, RC, and milestone versions

### Version Rules

The `maven-version-rules.xml` file filters out unstable versions:
- Ignores alpha, beta, RC, milestone builds
- Special rules for Vert.x dependencies
- Excludes Netty updates (managed by Vert.x)

## Development Guidelines

### When Creating Child Projects

Child projects should:
1. Declare `opinion-parent` as `<parent>` with appropriate version
2. Inherit build configuration - don't override unless necessary
3. Use dependency management from parent where available
4. Follow the Java 17 source compatibility
5. Place generated sources in `src/main/generated` and `src/test/generated`

### Testing

- Use JUnit 5 (`junit-jupiter-engine` v5.10.1)
- Tests run via `maven-surefire-plugin`
- Test a single class: `mvn test -Dtest=ClassName`
- Test a single method: `mvn test -Dtest=ClassName#methodName`

### Dependency Strategy

The parent POM is lightweight and does not define many dependencies directly. Child projects should:
- Declare their own dependencies explicitly
- Use version properties when available (e.g., `${junit5.version}`)
- Leverage shared plugins configured in `<pluginManagement>`

### Build Artifacts

Generated during build:
- `.flattened-pom.xml` - Flattened POM for consumption
- `target/` directory with compiled classes
- `*-fat.jar` - Shaded JARs (if child project uses shade plugin)
- Source JARs (e.g., `opinion-parent-sources.jar`)

### Clean Build

The maven-clean-plugin is configured to remove:
- `**/*.log` - Log files
- `**/generated/**` - Generated source directories
- `**/.flattened-pom.xml` - Flattened POMs
- `**/target/**` - All build artifacts

## Related Projects

The Opinion platform includes several supporting libraries:
- **inqwise-async** - Bridges blocking Java I/O with Vert.x non-blocking streams
- **inqwise-error** - Structured error handling with error tickets
- **inqwise-walker** - Tree walking utilities
- **inqwise-difference** - Diff utilities

These are part of the broader Inqwise ecosystem and follow similar reactive, event-driven patterns using Vert.x.

## License

Apache License 2.0
