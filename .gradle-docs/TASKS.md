# Gradle Tasks Reference - phpPgAdmin Module

Complete reference for all available Gradle tasks in the Bearsampp phpPgAdmin module.

## Task Groups

- [Build Tasks](#build-tasks)
- [Verification Tasks](#verification-tasks)
- [Information Tasks](#information-tasks)
- [Cleanup Tasks](#cleanup-tasks)

---

## Build Tasks

### `release`

Build and package a release for a specific phpPgAdmin version.

**Group**: `build`

**Usage**:
```bash
# Interactive mode (prompts for version selection)
gradle release

# Non-interactive mode (specify version)
gradle release -PbundleVersion=7.14.7
```

**Parameters**:
- `-PbundleVersion=X.X.X` - Specify the phpPgAdmin version to build

**Process**:
1. Validates the specified version exists in `bin/` or `bin/archived/`
2. Downloads phpPgAdmin source from modules-untouched
3. Extracts the source to temporary directory
4. Copies configuration files from `bin/phpPgAdmin{version}/`
5. Prepares the bundle in `tmp/bundles_prep/`
6. Creates a non-zip copy in `tmp/bundles_build/`
7. Compresses to archive in `bearsampp-build/apps/phpPgAdmin/{bundle.release}/`
8. Generates hash files (MD5, SHA1, SHA256, SHA512)

**Output**:
- Archive: `bearsampp-build/apps/phpPgAdmin/{bundle.release}/bearsampp-phpPgAdmin-{version}-{bundle.release}.7z`
- Hash files: `.md5`, `.sha1`, `.sha256`, `.sha512`
- Non-zip copy: `tmp/bundles_build/apps/phpPgAdmin/phpPgAdmin{version}/`

**Example**:
```bash
gradle release -PbundleVersion=7.14.7
```

---

### `releaseAll`

Build releases for all available phpPgAdmin versions found in the `bin/` directory.

**Group**: `build`

**Usage**:
```bash
gradle releaseAll
```

**Process**:
1. Scans `bin/` and `bin/archived/` for all phpPgAdmin versions
2. Builds each version sequentially
3. Reports success/failure for each version
4. Provides a summary at the end

**Output**:
- Multiple archives in `bearsampp-build/apps/phpPgAdmin/{bundle.release}/`
- Build summary with success/failure counts

**Example**:
```bash
gradle releaseAll
```

---

## Verification Tasks

### `verify`

Verify the build environment and dependencies.

**Group**: `verification`

**Usage**:
```bash
gradle verify
```

**Checks**:
- Java version (8+)
- `build.properties` file exists
- `bin/` directory exists
- 7-Zip installation (if format is 7z)

**Output**:
```
Environment Check Results:
------------------------------------------------------------
  [PASS]     Java 8+
  [PASS]     build.properties
  [PASS]     bin directory
  [PASS]     7-Zip
------------------------------------------------------------

[SUCCESS] All checks passed! Build environment is ready.
```

**Example**:
```bash
gradle verify
```

---

### `validateProperties`

Validate the `build.properties` configuration file.

**Group**: `verification`

**Usage**:
```bash
gradle validateProperties
```

**Validates**:
- `bundle.name` - Must be present and non-empty
- `bundle.release` - Must be present and non-empty
- `bundle.type` - Must be present and non-empty
- `bundle.format` - Must be present and non-empty

**Output**:
```
[SUCCESS] All required properties are present:
    bundle.name = phpPgAdmin
    bundle.release = 2025.10.31
    bundle.type = apps
    bundle.format = 7z
```

**Example**:
```bash
gradle validateProperties
```

---

### `checkModulesUntouched`

Check integration with the modules-untouched repository.

**Group**: `verification`

**Usage**:
```bash
gradle checkModulesUntouched
```

**Process**:
1. Fetches `phpPgAdmin.properties` from modules-untouched GitHub repository
2. Lists all available versions
3. Displays version resolution strategy

**Output**:
```
==================================================================
Modules-Untouched Integration Check
==================================================================

Repository URL:
  https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/phpPgAdmin.properties

Available Versions in modules-untouched
==================================================================
  7.14.7
  ...
==================================================================
Total versions: X

[SUCCESS] modules-untouched integration is working

Version Resolution Strategy:
  1. Check modules-untouched phpPgAdmin.properties (remote)
  2. Check releases.properties (local)
  3. Construct standard URL format (fallback)
```

**Example**:
```bash
gradle checkModulesUntouched
```

---

## Information Tasks

### `info`

Display comprehensive build configuration information.

**Group**: `help`

**Usage**:
```bash
gradle info
```

**Output**:
```
================================================================
          Bearsampp Module phpPgAdmin - Build Info
================================================================

Project:        module-phppgadmin
Version:        2025.10.31
Description:    Bearsampp Module - phpPgAdmin

Bundle Properties:
  Name:         phpPgAdmin
  Release:      2025.10.31
  Type:         apps
  Format:       7z

Paths:
  Project Dir:  E:/Bearsampp-development/module-phppgadmin
  Root Dir:     E:/Bearsampp-development
  Dev Path:     E:/Bearsampp-development/dev
  Build Base:   E:/Bearsampp-development/bearsampp-build

Build Paths:
  Tmp:          E:/Bearsampp-development/bearsampp-build/tmp
  Build:        E:/Bearsampp-development/bearsampp-build/tmp/bundles_build/apps/phpPgAdmin
  Prep:         E:/Bearsampp-development/bearsampp-build/tmp/bundles_prep/apps/phpPgAdmin
  Src:          E:/Bearsampp-development/bearsampp-build/tmp/bundles_src
  Download:     E:/Bearsampp-development/bearsampp-build/tmp/downloads/phpPgAdmin
  Extract:      E:/Bearsampp-development/bearsampp-build/tmp/extract/phpPgAdmin

Java:
  Version:      17
  Home:         C:\Program Files\Java\jdk-17

Gradle:
  Version:      8.12
  Home:         C:\Gradle\gradle-8.12

Quick Start:
  gradle tasks                              - List all available tasks
  gradle info                               - Show this information
  gradle release -PbundleVersion=7.14.7     - Build release for version
  gradle releaseAll                         - Build all versions
  gradle clean                              - Clean build artifacts
  gradle verify                             - Verify build environment
```

**Example**:
```bash
gradle info
```

---

### `listVersions`

List all available phpPgAdmin versions in the `bin/` and `bin/archived/` directories.

**Group**: `help`

**Usage**:
```bash
gradle listVersions
```

**Output**:
```
Available phpPgAdmin versions:
------------------------------------------------------------
  7.14.7          [bin]
  ...
------------------------------------------------------------
Total versions: X

To build a specific version:
  gradle release -PbundleVersion=7.14.7
```

**Example**:
```bash
gradle listVersions
```

---

### `listReleases`

List all available phpPgAdmin releases from the modules-untouched repository.

**Group**: `help`

**Usage**:
```bash
gradle listReleases
```

**Output**:
```
Available phpPgAdmin Releases:
--------------------------------------------------------------------------------
  7.14.7     -> https://github.com/phppgadmin/phppgadmin/archive/refs/tags/REL_7-14-7.zip
  ...
--------------------------------------------------------------------------------
Total releases: X
```

**Example**:
```bash
gradle listReleases
```

---

## Cleanup Tasks

### `clean`

Clean build artifacts and temporary files.

**Group**: `build`

**Usage**:
```bash
gradle clean
```

**Removes**:
- `build/` directory (Gradle build directory)

**Output**:
```
[SUCCESS] Build artifacts cleaned
```

**Example**:
```bash
gradle clean
```

---

### `cleanAll`

Clean all build artifacts, downloads, and temporary files.

**Group**: `build`

**Usage**:
```bash
gradle cleanAll
```

**Removes**:
- `build/` directory (Gradle build directory)
- `tmp/` directory (downloads, extracts, prep, build)
- Final output directory (`bearsampp-build/apps/phpPgAdmin/`)

**Output**:
```
Cleaned:
  - Gradle build directory
  - Tmp directory (E:/Bearsampp-development/bearsampp-build/tmp)
  - Output directory (E:/Bearsampp-development/bearsampp-build/apps/phpPgAdmin)

[SUCCESS] All build artifacts and temporary files cleaned
```

**Example**:
```bash
gradle cleanAll
```

---

## Task Execution Examples

### Build a Single Version

```bash
# Interactive mode
gradle release

# Non-interactive mode
gradle release -PbundleVersion=7.14.7
```

### Build All Versions

```bash
gradle releaseAll
```

### Verify Environment Before Building

```bash
gradle verify
gradle release -PbundleVersion=7.14.7
```

### Clean and Rebuild

```bash
gradle cleanAll
gradle release -PbundleVersion=7.14.7
```

### Check Available Versions

```bash
# Local versions (in bin/)
gradle listVersions

# Remote versions (modules-untouched)
gradle listReleases
```

### Debug Build Issues

```bash
# Run with info logging
gradle release -PbundleVersion=7.14.7 --info

# Run with debug logging
gradle release -PbundleVersion=7.14.7 --debug
```

---

## Task Dependencies

```
release
  └── (no dependencies)

releaseAll
  └── (calls release internally for each version)

clean
  └── (no dependencies)

cleanAll
  └── (no dependencies)

verify
  └── (no dependencies)

validateProperties
  └── (no dependencies)

checkModulesUntouched
  └── (no dependencies)

info
  └── (no dependencies)

listVersions
  └── (no dependencies)

listReleases
  └── (no dependencies)
```

---

## Common Task Combinations

### Initial Setup

```bash
gradle verify
gradle listVersions
```

### Development Workflow

```bash
# Build and test a single version
gradle release -PbundleVersion=7.14.7

# Clean and rebuild
gradle clean
gradle release -PbundleVersion=7.14.7
```

### Release Workflow

```bash
# Verify environment
gradle verify

# Build all versions
gradle releaseAll

# Verify output
ls bearsampp-build/apps/phpPgAdmin/2025.10.31/
```

### Troubleshooting

```bash
# Check configuration
gradle info
gradle validateProperties

# Check modules-untouched integration
gradle checkModulesUntouched

# Clean everything and start fresh
gradle cleanAll
gradle verify
gradle release -PbundleVersion=7.14.7 --info
```

---

**Last Updated**: 2025-01-31  
**Version**: 2025.10.31  
**Build System**: Pure Gradle (no wrapper, no Ant)
