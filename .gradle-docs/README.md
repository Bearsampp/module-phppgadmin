# Gradle Build Documentation - phpPgAdmin Module

This directory contains comprehensive documentation for the Gradle build system used in the Bearsampp phpPgAdmin module.

## Documentation Index

### Getting Started
- [Quick Start Guide](QUICK_START.md) - Get up and running quickly
- [Build System Overview](BUILD_SYSTEM_OVERVIEW.md) - Understanding the build architecture

### Build Tasks
- [Available Tasks](TASKS.md) - Complete list of all Gradle tasks
- [Release Process](RELEASE_PROCESS.md) - How to build releases

### Configuration
- [Build Properties](BUILD_PROPERTIES.md) - Configuration options
- [Path Configuration](PATH_CONFIGURATION.md) - Customizing build paths

### Integration
- [Modules Untouched Integration](MODULES_UNTOUCHED_INTEGRATION.md) - Remote version management

### Migration
- [Ant to Gradle Migration](ANT_TO_GRADLE_MIGRATION.md) - Migration guide from legacy Ant build

### Troubleshooting
- [Common Issues](TROUBLESHOOTING.md) - Solutions to common problems

## Quick Reference

### Build a Release
```bash
# Interactive mode
gradle release

# Non-interactive mode
gradle release -PbundleVersion=7.14.7
```

### List Available Versions
```bash
gradle listVersions
```

### Clean Build Artifacts
```bash
gradle clean      # Clean build artifacts only
gradle cleanAll   # Clean everything including downloads
```

### Verify Environment
```bash
gradle verify
```

## Key Features

- **Pure Gradle Implementation** - No Ant dependencies
- **Automatic Downloads** - Downloads phpPgAdmin sources automatically
- **Hash Generation** - Creates MD5, SHA1, SHA256, SHA512 hash files
- **Interactive Mode** - User-friendly prompts for version selection
- **Modules-Untouched Integration** - Automatic version resolution
- **Incremental Builds** - Fast rebuilds with Gradle caching

## Support

For issues or questions:
1. Check the [Troubleshooting Guide](TROUBLESHOOTING.md)
2. Review the [Build System Overview](BUILD_SYSTEM_OVERVIEW.md)
3. Open an issue on GitHub

## Version

Documentation Version: 1.0.0  
Last Updated: 2024-11-16  
Build System: Gradle 9.2+
