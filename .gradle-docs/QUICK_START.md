# Quick Start Guide

Get started with building phpPgAdmin module releases using Gradle.

## Prerequisites

1. **Java 8 or higher** - Check with `java -version`
2. **Gradle 9.2+** - Check with `gradle --version`
3. **7-Zip** - Required for creating .7z archives
4. **Internet connection** - For downloading phpPgAdmin sources

## First Time Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Bearsampp/module-phppgadmin.git
   cd module-phppgadmin
   ```

2. **Verify your environment**
   ```bash
   gradle verify
   ```

3. **View build information**
   ```bash
   gradle info
   ```

## Building Your First Release

### Option 1: Interactive Mode (Recommended for beginners)

```bash
gradle release
```

You'll be prompted to select a version from the available versions in the `bin/` directory.

### Option 2: Non-Interactive Mode

```bash
gradle release -PbundleVersion=7.14.7
```

Replace `7.14.7` with the version you want to build.

## What Happens During a Build?

1. **Version Check** - Checks if version exists in `bin/` directory
2. **Download** - If not found locally, downloads from modules-untouched or GitHub
3. **Extract** - Extracts the downloaded archive
4. **Prepare** - Copies configuration files from `bin/` directory
5. **Package** - Creates the release archive
6. **Hash Generation** - Generates MD5, SHA1, SHA256, SHA512 hash files

## Output Location

Built releases are placed in:
```
E:/Bearsampp-development/bearsampp-build/apps/phppgadmin/{release-version}/
```

Example:
```
bearsampp-build/
└── apps/
    └── phppgadmin/
        └── 2024.4.14/
            ├── bearsampp-phppgadmin-7.14.7-2024.4.14.7z
            ├── bearsampp-phppgadmin-7.14.7-2024.4.14.7z.md5
            ├── bearsampp-phppgadmin-7.14.7-2024.4.14.7z.sha1
            ├── bearsampp-phppgadmin-7.14.7-2024.4.14.7z.sha256
            └── bearsampp-phppgadmin-7.14.7-2024.4.14.7z.sha512
```

## Common Commands

```bash
# List all available tasks
gradle tasks

# List versions in bin/ directory
gradle listVersions

# List releases from modules-untouched
gradle listReleases

# Build all versions
gradle releaseAll

# Clean build artifacts
gradle clean

# Clean everything including downloads
gradle cleanAll

# Validate build.properties
gradle validateProperties

# Check modules-untouched integration
gradle checkModulesUntouched
```

## Next Steps

- Read the [Build System Overview](BUILD_SYSTEM_OVERVIEW.md) to understand the architecture
- Check [Available Tasks](TASKS.md) for a complete list of tasks
- Learn about [Build Properties](BUILD_PROPERTIES.md) for customization options

## Troubleshooting

If you encounter issues:
1. Run `gradle verify` to check your environment
2. Check the [Troubleshooting Guide](TROUBLESHOOTING.md)
3. Ensure you have internet connectivity for downloads
4. Verify 7-Zip is installed and accessible
