# Bearsampp Module phpPgAdmin - Gradle Build Documentation

## Table of Contents

- [Overview](#overview)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Build Tasks](#build-tasks)
- [Configuration](#configuration)
- [Architecture](#architecture)
- [Troubleshooting](#troubleshooting)

---

## Overview

The Bearsampp Module phpPgAdmin project uses a **pure Gradle build system** (no wrapper, no Ant). This provides:

- **Modern Build System**     - Native Gradle tasks and conventions
- **Better Performance**       - Incremental builds and caching
- **Simplified Maintenance**   - Pure Groovy/Gradle DSL
- **Enhanced Tooling**         - IDE integration and dependency management
- **Cross-Platform Support**   - Works on Windows, Linux, and macOS

### Project Information

| Property          | Value                                    |
|-------------------|------------------------------------------|
| **Project Name**  | module-phppgadmin                        |
| **Group**         | com.bearsampp.modules                    |
| **Type**          | phpPgAdmin Module Builder                |
| **Build Tool**    | Gradle 8.x+                              |
| **Language**      | Groovy (Gradle DSL)                      |

---

## Quick Start

### Prerequisites

| Requirement       | Version       | Purpose                                  |
|-------------------|---------------|------------------------------------------|
| **Java**          | 8+            | Required for Gradle execution            |
| **Gradle**        | 8.0+          | Build automation tool                    |
| **7-Zip**         | Latest        | Archive compression                      |

### Basic Commands

```bash
# Display build information
gradle info

# List all available tasks
gradle tasks

# Verify build environment
gradle verify

# Build a release (interactive)
gradle release

# Build a specific version (non-interactive)
gradle release -PbundleVersion=7.14.7

# Clean build artifacts
gradle clean
```

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/bearsampp/module-phppgadmin.git
cd module-phppgadmin
```

### 2. Verify Environment

```bash
gradle verify
```

This will check:
- Java version (8+)
- Required files (build.properties)
- Directory structure (bin/)
- Build dependencies (7-Zip)

### 3. List Available Versions

```bash
gradle listVersions
```

### 4. Build Your First Release

```bash
# Interactive mode (prompts for version)
gradle release

# Or specify version directly
gradle release -PbundleVersion=7.14.7
```

---

## Build Tasks

### Core Build Tasks

| Task                  | Description                                      | Example                                  |
|-----------------------|--------------------------------------------------|------------------------------------------|
| `release`             | Build and package release (interactive/non-interactive) | `gradle release -PbundleVersion=7.14.7` |
| `releaseAll`          | Build all available versions                     | `gradle releaseAll`                      |
| `clean`               | Clean build artifacts                            | `gradle clean`                           |
| `cleanAll`            | Clean all including downloads                    | `gradle cleanAll`                        |

### Verification Tasks

| Task                      | Description                                  | Example                                      |
|---------------------------|----------------------------------------------|----------------------------------------------|
| `verify`                  | Verify build environment and dependencies    | `gradle verify`                              |
| `validateProperties`      | Validate build.properties configuration      | `gradle validateProperties`                  |

### Information Tasks

| Task                | Description                                      | Example                    |
|---------------------|--------------------------------------------------|----------------------------|
| `info`              | Display build configuration information          | `gradle info`              |
| `listVersions`      | List available bundle versions in bin/           | `gradle listVersions`      |
| `listReleases`      | List all available releases from modules-untouched | `gradle listReleases`  |
| `checkModulesUntouched` | Check modules-untouched integration          | `gradle checkModulesUntouched` |

### Task Groups

| Group            | Purpose                                          |
|------------------|--------------------------------------------------|
| **build**        | Build and package tasks                          |
| **verification** | Verification and validation tasks                |
| **help**         | Help and information tasks                       |

---

## Configuration

### build.properties

The main configuration file for the build:

```properties
bundle.name     = phpPgAdmin
bundle.release  = 2025.10.31
bundle.type     = apps
bundle.format   = 7z
```

| Property          | Description                          | Example Value  |
|-------------------|--------------------------------------|----------------|
| `bundle.name`     | Name of the bundle                   | `phpPgAdmin`   |
| `bundle.release`  | Release version                      | `2025.10.31`   |
| `bundle.type`     | Type of bundle                       | `apps`         |
| `bundle.format`   | Archive format                       | `7z`           |

### gradle.properties

Gradle-specific configuration:

```properties
# Gradle daemon configuration
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.caching=true

# JVM settings
org.gradle.jvmargs=-Xmx2g -XX:MaxMetaspaceSize=512m
```

### Directory Structure

```
module-phppgadmin/
├── .gradle-docs/          # Gradle documentation
│   ├── README.md          # Main documentation
│   └── TASKS.md           # Task reference
├── bin/                   # phpPgAdmin version bundles
│   ├── phpPgAdmin7.14.7/
│   └── ...
├── bearsampp-build/       # External build directory (outside repo)
│   ├── tmp/               # Temporary build files
│   │   ├── bundles_prep/apps/phpPgAdmin/  # Prepared bundles
│   │   ├── bundles_build/apps/phpPgAdmin/ # Build staging
│   │   ├── downloads/phpPgAdmin/          # Downloaded sources
���   │   └── extract/phpPgAdmin/            # Extracted archives
│   └── apps/phpPgAdmin/   # Final packaged archives
│       └── 2025.10.31/    # Release version
│           ├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z
│           ├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z.md5
│           └── ...
├── build.gradle           # Main Gradle build script
├── settings.gradle        # Gradle settings
├── build.properties       # Build configuration
└── releases.properties    # Available phpPgAdmin releases
```

---

## Architecture

### Build Process Flow

```
1. User runs: gradle release -PbundleVersion=7.14.7
                    ↓
2. Validate environment and version
                    ↓
3. Download phpPgAdmin source from modules-untouched
                    ↓
4. Extract to temporary directory
                    ↓
5. Copy configuration files from bin/phpPgAdmin7.14.7/
                    ↓
6. Prepare bundle in tmp/bundles_prep/
                    ↓
7. Copy to tmp/bundles_build/ (non-zip version)
                    ↓
8. Package into archive in bearsampp-build/apps/phpPgAdmin/{bundle.release}/
   - The archive includes the top-level folder: phpPgAdmin{version}/
                    ↓
9. Generate hash files (MD5, SHA1, SHA256, SHA512)
```

### Packaging Details

- **Archive name format**: `bearsampp-phpPgAdmin-{version}-{bundle.release}.{7z|zip}`
- **Location**: `bearsampp-build/apps/phpPgAdmin/{bundle.release}/`
  - Example: `bearsampp-build/apps/phpPgAdmin/2025.10.31/bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z`
- **Content root**: The top-level folder inside the archive is `phpPgAdmin{version}/` (e.g., `phpPgAdmin7.14.7/`)
- **Structure**: The archive contains the phpPgAdmin version folder at the root with all files inside

**Archive Structure Example**:
```
bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z
└── phpPgAdmin7.14.7/       ← Version folder at root
    ├── index.php
    ├── conf/
    │   └── config.inc.php
    ├── classes/
    ├── lang/
    └── ...
```

**Verification Commands**:

```bash
# List archive contents with 7z
7z l bearsampp-build/apps/phpPgAdmin/2025.10.31/bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z | more

# You should see entries beginning with:
#   phpPgAdmin7.14.7/index.php
#   phpPgAdmin7.14.7/conf/config.inc.php
#   phpPgAdmin7.14.7/...
```

**Hash Files**: Each archive is accompanied by hash sidecar files:
- `.md5` - MD5 checksum
- `.sha1` - SHA-1 checksum
- `.sha256` - SHA-256 checksum
- `.sha512` - SHA-512 checksum

Example:
```
bearsampp-build/apps/phpPgAdmin/2025.10.31/
├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z
├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z.md5
├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z.sha1
├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z.sha256
└── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z.sha512
```

### Version Resolution

The build system resolves download URLs in the following order:

1. **modules-untouched** - Remote phpPgAdmin.properties from GitHub
2. **releases.properties** - Local fallback file
3. **Standard URL format** - Constructed URL as last resort

---

## Troubleshooting

### Common Issues

#### Issue: "Dev path not found"

**Symptom:**
```
Dev path not found: E:/Bearsampp-development/dev
```

**Solution:**
This is a warning only. The dev path is optional for most tasks. If you need it, ensure the `dev` project exists in the parent directory.

---

#### Issue: "Bundle version not found"

**Symptom:**
```
Bundle version not found in bin/ or bin/archived/
```

**Solution:**
1. List available versions: `gradle listVersions`
2. Use an existing version: `gradle release -PbundleVersion=7.14.7`

---

#### Issue: "7-Zip not found"

**Symptom:**
```
7-Zip not found. Please install 7-Zip or set 7Z_HOME environment variable.
```

**Solution:**
1. Install 7-Zip from https://www.7-zip.org/
2. Or set `7Z_HOME` environment variable to your 7-Zip installation directory

---

#### Issue: "Java version too old"

**Symptom:**
```
Java 8+ required
```

**Solution:**
1. Check Java version: `java -version`
2. Install Java 8 or higher
3. Update JAVA_HOME environment variable

---

### Debug Mode

Run Gradle with debug output:

```bash
gradle release -PbundleVersion=7.14.7 --info
gradle release -PbundleVersion=7.14.7 --debug
```

### Clean Build

If you encounter issues, try a clean build:

```bash
gradle cleanAll
gradle release -PbundleVersion=7.14.7
```

---

## Additional Resources

- [Gradle Documentation](https://docs.gradle.org/)
- [Bearsampp Project](https://github.com/bearsampp/bearsampp)
- [phpPgAdmin Project](http://phppgadmin.sourceforge.net/)

---

## Support

For issues and questions:

- **GitHub Issues**: https://github.com/bearsampp/module-phppgadmin/issues
- **Bearsampp Issues**: https://github.com/bearsampp/bearsampp/issues
- **Documentation**: https://bearsampp.com/module/phppgadmin

---

**Last Updated**: 2025-01-31  
**Version**: 2025.10.31  
**Build System**: Pure Gradle (no wrapper, no Ant)

**Notes**:
- This project deliberately does not ship the Gradle Wrapper. Install Gradle 8+ locally and run with `gradle ...`.
- Legacy Ant files have been removed. The build system is pure Gradle.
