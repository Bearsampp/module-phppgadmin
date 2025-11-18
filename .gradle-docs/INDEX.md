# Documentation Index - phpPgAdmin Module

Complete documentation index for the Bearsampp phpPgAdmin module Gradle build system.

## Core Documentation

### [README.md](README.md)
Main documentation covering:
- Overview and project information
- Quick start guide
- Installation instructions
- Build tasks overview
- Configuration details
- Architecture and build process
- Troubleshooting guide

### [TASKS.md](TASKS.md)
Complete task reference including:
- Build tasks (`release`, `releaseAll`)
- Verification tasks (`verify`, `validateProperties`, `checkModulesUntouched`)
- Information tasks (`info`, `listVersions`, `listReleases`)
- Cleanup tasks (`clean`, `cleanAll`)
- Task execution examples
- Common task combinations

---

## Quick Reference

### Essential Commands

```bash
# Get build information
gradle info

# List all tasks
gradle tasks

# Verify environment
gradle verify

# Build a release
gradle release -PbundleVersion=7.14.7

# Build all versions
gradle releaseAll

# Clean build artifacts
gradle clean
gradle cleanAll
```

### Key Files

| File                  | Purpose                                      |
|-----------------------|----------------------------------------------|
| `build.gradle`        | Main Gradle build script                     |
| `settings.gradle`     | Gradle project settings                      |
| `build.properties`    | Build configuration (bundle info)            |
| `gradle.properties`   | Gradle-specific configuration                |
| `releases.properties` | Local phpPgAdmin release URLs (fallback)     |

### Directory Structure

```
module-phppgadmin/
├── .gradle-docs/          # Documentation
│   ├── INDEX.md           # This file
│   ├── README.md          # Main documentation
│   └── TASKS.md           # Task reference
├── bin/                   # phpPgAdmin version bundles
│   ├── phpPgAdmin7.14.7/
│   └── archived/          # Archived versions
├── build.gradle           # Gradle build script
├── settings.gradle        # Gradle settings
├── build.properties       # Build configuration
└── releases.properties    # Release URLs
```

---

## Documentation by Topic

### Getting Started
- [Quick Start](README.md#quick-start) - Get up and running quickly
- [Installation](README.md#installation) - Step-by-step setup
- [Prerequisites](README.md#prerequisites) - Required software

### Building
- [Build Tasks](TASKS.md#build-tasks) - Core build tasks
- [Release Process](README.md#architecture) - How releases are built
- [Build Output](README.md#packaging-details) - Where files are created

### Configuration
- [build.properties](README.md#buildproperties) - Main configuration
- [gradle.properties](README.md#gradleproperties) - Gradle settings
- [Directory Structure](README.md#directory-structure) - Project layout

### Verification
- [Verification Tasks](TASKS.md#verification-tasks) - Environment checks
- [Verify Command](TASKS.md#verify) - Check build environment
- [Validate Properties](TASKS.md#validateproperties) - Check configuration

### Information
- [Information Tasks](TASKS.md#information-tasks) - Get build info
- [List Versions](TASKS.md#listversions) - Available versions
- [List Releases](TASKS.md#listreleases) - Remote releases

### Troubleshooting
- [Common Issues](README.md#troubleshooting) - Solutions to problems
- [Debug Mode](README.md#debug-mode) - Verbose output
- [Clean Build](README.md#clean-build) - Start fresh

---

## Build System Features

### Pure Gradle
- No Gradle Wrapper (install Gradle 8+ locally)
- No Ant dependencies (pure Gradle implementation)
- Modern Groovy DSL
- Incremental builds and caching

### Automatic Downloads
- Downloads phpPgAdmin sources from modules-untouched
- Fallback to local releases.properties
- Automatic extraction and preparation

### Version Management
- Interactive version selection
- Non-interactive mode with `-PbundleVersion`
- Support for archived versions
- Build all versions at once

### Hash Generation
- Automatic hash file generation
- MD5, SHA1, SHA256, SHA512
- Sidecar files for each archive

### Integration
- modules-untouched repository integration
- Automatic version resolution
- Remote and local fallback

---

## Task Groups

### Build
- `release` - Build a single version
- `releaseAll` - Build all versions
- `clean` - Clean build artifacts
- `cleanAll` - Clean everything

### Verification
- `verify` - Verify environment
- `validateProperties` - Validate configuration
- `checkModulesUntouched` - Check integration

### Help
- `info` - Display build information
- `listVersions` - List local versions
- `listReleases` - List remote releases
- `tasks` - List all tasks

---

## External Resources

- [Gradle Documentation](https://docs.gradle.org/)
- [Bearsampp Project](https://github.com/bearsampp/bearsampp)
- [phpPgAdmin Project](http://phppgadmin.sourceforge.net/)
- [Bearsampp Module Documentation](https://bearsampp.com/module/phppgadmin)

---

## Support

For issues and questions:
- **GitHub Issues**: https://github.com/bearsampp/module-phppgadmin/issues
- **Bearsampp Issues**: https://github.com/bearsampp/bearsampp/issues

---

**Last Updated**: 2025-01-31  
**Version**: 2025.10.31  
**Build System**: Pure Gradle (no wrapper, no Ant)
