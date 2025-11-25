# phpPgAdmin CI/CD Documentation

## Overview

Automated testing workflow for phpPgAdmin module with smart version detection that tests only relevant versions from pull requests.

---

## 🚀 Quick Setup

### 1. Configure GitHub Token

Create a Personal Access Token with these permissions:
- `repo` - Full control of private repositories
- `workflow` - Update GitHub Action workflows

Add to repository secrets:
```
Settings → Secrets and variables → Actions → New repository secret
Name: GH_PAT
Value: ghp_your_token_here
```

### 2. Enable Repository Settings

```
Settings → Actions → General
→ ✅ Read and write permissions
→ ✅ Allow GitHub Actions to create and approve pull requests

Settings → General → Pull Requests
→ ✅ Allow auto-merge
```

---

## 📋 How It Works

### Smart Version Detection (2-Tier Strategy)

The workflow automatically detects which versions to test from pull requests:

#### 1️⃣ Primary: /bin Directory Changes
Detects versions from changed files in `/bin` directory.

**Example:**
```bash
# Changed files:
bin/phppgadmin7.14.7/index.php
bin/phppgadmin7.14.7/conf/config.inc.php-dist

# Result: Tests only version 7.14.7
```

#### 2️⃣ Fallback: PR Title Parsing
Extracts version numbers from PR title.

**Example:**
```
PR Title: "Add phpPgAdmin 7.14.4 and 7.14.7"
Result: Tests versions 7.14.4 and 7.14.7
```

#### ⚠️ No Automatic Fallback for PRs
If no versions are detected by either method, **the workflow will fail**. This ensures intentional testing.

**To test latest versions, use manual workflow dispatch** (see Manual Testing section below).

### Important: Always Uses Main Branch

**The workflow always reads `releases.properties` from the main branch**, not from the PR branch. This ensures:
- ✅ Consistent version URLs
- ✅ Stable download links
- ✅ No broken references from PR changes

---

## 🔧 Workflow Files

### phppgadmin-test.yml

**Triggers:**
- Pull requests to `main` branch
- Manual workflow dispatch

**What it does:**
1. Detects versions to test (3-tier strategy)
2. Downloads each version from releases.properties (main branch)
3. Extracts and verifies structure
4. Tests configuration (if present)
5. Generates test report

**File Checks:**

Required files (must exist):
- ✅ `index.php`

Optional files (warning only):
- ⚠️ `conf/config.inc.php-dist`

### update-releases-properties.yml

Automatically updates `releases.properties` when releases are published.

### validate-properties-links.yml

Validates all download URLs in properties files before merge.

---

## 💡 Usage Examples

### Example 1: Creating a Release

```bash
# 1. Create pre-release
Tag: 2025.01.15
Files: bearsampp-phppgadmin-7.14.7-2025.01.15.7z

# 2. Publish release

# 3. Automatic actions:
# ✅ Properties updated on main branch
# ✅ PR created
# ✅ Links validated
# ✅ Auto-merged
```

### Example 2: Creating a PR (Method 1 - Fastest)

```bash
# Add version to /bin directory
git checkout -b january-release
mkdir -p bin/phppgadmin7.14.7
# Add files...
git commit -m "Add phpPgAdmin 7.14.7"
git push

# Result: Tests only version 7.14.7 ✅
# Time: ~3 minutes
```

### Example 3: Creating a PR (Method 2)

```bash
# Include version in PR title
PR Title: "Update phpPgAdmin 7.14.7 configuration"

# Result: Tests version 7.14.7 ✅
```

### Example 4: Manual Testing

```bash
# Go to Actions → phpPgAdmin Test → Run workflow
# Input versions: 7.14.4,7.14.7
# Or leave empty to test latest 5

# Result: Tests specified versions ✅
```

---

## 📊 Performance Benefits

### Traditional Approach
```
Test all versions: 5 versions × 3 min = 15 minutes
```

### Smart Detection Approach (PR)
```
Test changed versions: 1 version × 3 min = 3 minutes
Savings: 80% faster ⚡
```

### Manual Mode
```
Test latest N versions: Configurable (default 5)
Test specific versions: As many as needed
```

---

## 🔍 Version Detection Flow

### For Pull Requests
```
PR Created
    │
    ▼
Check /bin changes
    │
    ├─ Found? → Test those versions ✅
    │
    ▼ No
Check PR title
    │
    ├─ Found? → Test those versions ✅
    │
    ▼ No
Workflow fails ❌
(Must specify versions in /bin or PR title)
```

### For Manual Workflow Dispatch
```
Manual Trigger
    │
    ├─ Versions specified? → Test those versions ✅
    │
    ▼ No
Test latest N from main branch ✅
(Default: 5 versions)
```

---

## 🛠️ Troubleshooting

### "No versions detected"
**For PRs:** Add version to PR title or `/bin` directory  
**For Manual Runs:** Specify versions or leave empty to test latest N

### "Version X.X.X not found in releases.properties"
**Solution:** Ensure version exists in `releases.properties` on **main branch**

### "Download failed - 404"
**Solution:** 
1. Check `GH_PAT` secret is configured
2. Verify release exists and is published
3. Ensure URL in `releases.properties` (main branch) is correct

### "Missing required file: index.php"
**Solution:** Ensure `index.php` exists in the phpPgAdmin directory

### "Optional file not found: conf/config.inc.php-dist"
**Note:** This is just a warning, not an error. Tests will continue.

---

## 📝 Important Notes

### fetch-depth: 0
```yaml
- uses: actions/checkout@v4
  with:
    fetch-depth: 0  # Required for git diff with main branch
```
This is **critical** for version detection to work.

### GH_PAT Token
```yaml
token: ${{ secrets.GH_PAT || secrets.GITHUB_TOKEN }}
```
Required for:
- Downloading pre-release assets
- Creating PRs with auto-merge
- Accessing private releases

### Main Branch Reference
```bash
# Always uses main branch for releases.properties
git checkout main -- releases.properties
```
Ensures stable and consistent version URLs.

---

## 🎯 Best Practices

### For Releases
1. Use pre-releases for testing
2. Follow naming: `bearsampp-phppgadmin-{version}-{date}.7z`
3. Use semantic versioning (e.g., `7.14.7`)
4. Use date format for tags (e.g., `2025.01.15`)

### For Pull Requests
1. Add versions to `/bin/phppgadmin{version}/` for fastest testing
2. Include version numbers in PR titles as backup
3. Wait for automated tests before merging
4. Review test logs if failures occur

### For Testing
1. Test locally before pushing
2. Use manual workflow for specific version testing
3. Check logs immediately if tests fail
4. Test one or two versions at a time

---

## 📚 Additional Resources

- **Quick Start:** `docs/QUICK_START.md`
- **Reference Workflow:** [module-phpmyadmin](https://github.com/Bearsampp/module-phpmyadmin/blob/main/.github/workflows/phpmyadmin-test.yml)
- **GitHub Actions Docs:** [docs.github.com/actions](https://docs.github.com/en/actions)

---

## 🤝 Support

For issues or questions:
1. Check troubleshooting section above
2. Review workflow logs in Actions tab
3. Open an issue in the repository
4. Contact the maintainers

---

**Version:** 1.1.0  
**Last Updated:** 2025-01-15  
**Changes:**
- Always uses main branch for releases.properties
- Made conf/config.inc.php-dist optional
