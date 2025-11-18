<p align="center"><a href="https://bearsampp.com/contribute" target="_blank"><img width="250" src="img/Bearsampp-logo.svg"></a></p>

[![GitHub release](https://img.shields.io/github/release/bearsampp/module-phppgadmin.svg?style=flat-square)](https://github.com/bearsampp/module-phppgadmin/releases/latest)
![Total downloads](https://img.shields.io/github/downloads/bearsampp/module-phppgadmin/total.svg?style=flat-square)

# Bearsampp Module - phpPgAdmin

This is a module of [Bearsampp project](https://github.com/bearsampp/bearsampp) involving phpPgAdmin, a web-based administration tool for PostgreSQL.

## Build System

This module uses a **pure Gradle build system** (no wrapper, no Ant) for building and packaging phpPgAdmin releases.

### Prerequisites

| Requirement       | Version       | Purpose                                  |
|-------------------|---------------|------------------------------------------|
| **Java**          | 8+            | Required for Gradle execution            |
| **Gradle**        | 8.0+          | Build automation tool                    |
| **7-Zip**         | Latest        | Archive compression                      |

### Quick Start

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

# Build all versions
gradle releaseAll

# Clean build artifacts
gradle clean
```

### Available Tasks

| Task                      | Description                                      |
|---------------------------|--------------------------------------------------|
| `release`                 | Build release package (interactive/non-interactive) |
| `releaseAll`              | Build all available versions                     |
| `clean`                   | Clean build artifacts                            |
| `cleanAll`                | Clean all including downloads                    |
| `info`                    | Display build configuration                      |
| `verify`                  | Verify build environment                         |
| `listVersions`            | List available bundle versions                   |
| `listReleases`            | List all available releases                      |
| `validateProperties`      | Validate build.properties                        |
| `checkModulesUntouched`   | Check modules-untouched integration              |

### Configuration

The build is configured through `build.properties`:

```properties
bundle.name     = phpPgAdmin
bundle.release  = 2025.10.31
bundle.type     = apps
bundle.format   = 7z
```

### Build Output

Archives are created in: `bearsampp-build/apps/phpPgAdmin/{bundle.release}/`

Example:
```
bearsampp-build/apps/phpPgAdmin/2025.10.31/
├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z
├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z.md5
├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z.sha1
├── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z.sha256
└── bearsampp-phpPgAdmin-7.14.7-2025.10.31.7z.sha512
```

## Documentation

Complete build documentation is available in [.gradle-docs/](.gradle-docs/):

- [README.md](.gradle-docs/README.md) - Main documentation
- [TASKS.md](.gradle-docs/TASKS.md) - Task reference

## Module Information

https://bearsampp.com/module/phppgadmin

## Issues

Issues must be reported on [Bearsampp repository](https://github.com/bearsampp/bearsampp/issues).

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.
