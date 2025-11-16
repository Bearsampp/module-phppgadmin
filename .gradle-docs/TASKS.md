# Available Gradle Tasks

Complete reference of all available Gradle tasks for the phpPgAdmin module.

## Build Tasks

### `release`
Build a release package for a specific version.

**Usage:**
```bash
# Interactive mode - prompts for version selection
gradle release

# Non-interactive mode - specify version
gradle release -PbundleVersion=7.14.7
```

**What it does:**
1. Checks if version exists in `bin/` or `bin/archived/`
2. If not found, downloads from modules-untouched or GitHub
3. Extracts the source files
4. Copies configuration files from `bin/` directory
5. Creates release archive (.7z format)
6. Generates hash files (MD5, SHA1, SHA256, SHA512)

**Output:** `bearsampp-build/apps/phppgadmin/{release}/bearsampp-phppgadmin-{version}-{release}.7z`

---

### `releaseAll`
Build release packages for all available versions in the `bin/` directory.

**Usage:**
```bash
gradle releaseAll
```

**What it does:**
- Iterates through all versions found in `bin/` and `bin/archived/`
- Builds each version sequentially
- Provides a summary of successful and failed builds

**Output:** Multiple release archives, one for each version

---

### `clean`
Clean build artifacts and temporary files.

**Usage:**
```bash
gradle clean
```

**What it does:**
- Removes the `build/` directory
- Cleans Gradle-specific build artifacts

---

### `cleanAll`
Clean all build artifacts, downloads, and temporary files.

**Usage:**
```bash
gradle cleanAll
```

**What it does:**
- Removes the `build/` directory
- Removes `bearsampp-build/tmp/` directory (downloads, extracts, prep, build)
- Removes `bearsampp-build/apps/phppgadmin/` output directory

---

## Verification Tasks

### `verify`
Verify the build environment and dependencies.

**Usage:**
```bash
gradle verify
```

**What it checks:**
- Java 8+ is installed
- `build.properties` file exists
- `bin/` directory exists
- 7-Zip is installed and accessible

**Output:** Pass/Fail status for each check

---

### `validateProperties`
Validate `build.properties` configuration.

**Usage:**
```bash
gradle validateProperties
```

**What it checks:**
- `bundle.name` is set
- `bundle.release` is set
- `bundle.type` is set
- `bundle.format` is set

---

### `checkModulesUntouched`
Check modules-untouched repository integration.

**Usage:**
```bash
gradle checkModulesUntouched
```

**What it does:**
- Fetches `phppgadmin.properties` from modules-untouched repository
- Lists all available versions
- Verifies connectivity and integration

---

## Help Tasks

### `info`
Display build configuration information.

**Usage:**
```bash
gradle info
```

**What it shows:**
- Project information
- Bundle properties
- Build paths
- Java and Gradle versions
- Quick start commands

---

### `tasks`
List all available Gradle tasks.

**Usage:**
```bash
gradle tasks

# Show all tasks including internal ones
gradle tasks --all
```

---

### `listVersions`
List all available bundle versions in `bin/` and `bin/archived/` directories.

**Usage:**
```bash
gradle listVersions
```

**Output:**
```
Available phppgadmin versions:
------------------------------------------------------------
  7.13.0          [bin/archived]
  7.14.4          [bin]
  7.14.7          [bin]
------------------------------------------------------------
Total versions: 3
```

---

### `listReleases`
List all available releases from modules-untouched repository.

**Usage:**
```bash
gradle listReleases
```

**What it does:**
- Fetches `phppgadmin.properties` from modules-untouched
- Falls back to local `releases.properties` if unavailable
- Lists all versions with their download URLs

---

## Task Groups

Tasks are organized into the following groups:

- **build** - Build and package tasks
- **verification** - Verification and validation tasks
- **help** - Help and information tasks

View tasks by group:
```bash
gradle tasks --group=build
gradle tasks --group=verification
gradle tasks --group=help
```

## Default Task

Running `gradle` without any task name executes the default task:

```bash
gradle
```

This is equivalent to `gradle info` and displays build information.

## Task Dependencies

Some tasks have dependencies that run automatically:

- `release` → Checks environment and validates properties
- `releaseAll` → Runs `release` for each version
- `cleanAll` → Includes everything `clean` does plus more

## Advanced Usage

### Running Multiple Tasks

```bash
gradle clean release -PbundleVersion=7.14.7
```

### Parallel Execution

```bash
gradle releaseAll --parallel
```

### Verbose Output

```bash
gradle release -PbundleVersion=7.14.7 --info
gradle release -PbundleVersion=7.14.7 --debug
```

### Dry Run

```bash
gradle release -PbundleVersion=7.14.7 --dry-run
```

## See Also

- [Release Process](RELEASE_PROCESS.md) - Detailed release workflow
- [Build Properties](BUILD_PROPERTIES.md) - Configuration options
- [Troubleshooting](TROUBLESHOOTING.md) - Common issues and solutions
