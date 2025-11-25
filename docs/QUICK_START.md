# Quick Start Guide - phpPgAdmin CI/CD

## 🚀 5-Minute Setup

### 1. Configure GitHub Token (2 minutes)

Create a Personal Access Token:
```
GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
→ Generate new token
→ Select: repo, workflow
→ Generate token
```

Add to repository:
```
Repository Settings → Secrets and variables → Actions
→ New repository secret
→ Name: GH_PAT
→ Value: [paste token]
→ Add secret
```

### 2. Enable Settings (1 minute)

```
Settings → Actions → General
→ Workflow permissions
→ ✅ Read and write permissions
→ ✅ Allow GitHub Actions to create and approve pull requests
→ Save

Settings → General → Pull Requests
→ ✅ Allow auto-merge
→ Save
```

### 3. Done! (2 minutes to verify)

Create a test PR or trigger the workflow manually to verify it works.

---

## 📦 Creating a Release

### Step-by-Step

```bash
# 1. Create pre-release on GitHub
Tag: 2025.01.15
Title: phpPgAdmin Release 2025.01.15
✅ Set as a pre-release

# 2. Upload .7z file
bearsampp-phppgadmin-7.14.7-2025.01.15.7z

# 3. Publish release

# 4. Wait 2-3 minutes
# ✅ releases.properties updated automatically
# ✅ PR created and auto-merged
```

---

## 🔄 Creating a Pull Request

### Method 1: /bin Changes (Fastest - Recommended)

```bash
git checkout -b january-release
mkdir -p bin/phppgadmin7.14.7
# Add your files to bin/phppgadmin7.14.7/
git add .
git commit -m "Add phpPgAdmin 7.14.7"
git push origin january-release

# Create PR on GitHub
# Result: Tests only version 7.14.7 (3 minutes)
```

### Method 2: PR Title

```bash
# Just include version in PR title
PR Title: "Add phpPgAdmin 7.14.7"

# Result: Tests version 7.14.7 (3 minutes)
```

### Method 3: No Automatic Fallback

```bash
# No version info provided in PR
# Result: Workflow fails ❌

# To test latest versions, use manual workflow dispatch
```

---

## 🧪 Manual Testing

### Test Specific Versions

```
1. Go to Actions tab
2. Click "phpPgAdmin Test"
3. Click "Run workflow"
4. Enter versions: 7.14.4,7.14.7
5. Click "Run workflow"
```

### Test Latest N Versions

```
1. Go to Actions tab
2. Click "phpPgAdmin Test"
3. Click "Run workflow"
4. Leave versions empty
5. Set test_latest: 3
6. Click "Run workflow"
```

---

## 📋 Checklists

### Release Checklist

- [ ] Create pre-release with date tag (e.g., `2025.01.15`)
- [ ] Upload `.7z` file with correct naming
- [ ] Publish release
- [ ] Wait for auto-PR (2-3 minutes)
- [ ] Verify link validation passes
- [ ] Confirm auto-merge completes

### PR Checklist

- [ ] Add version to `/bin/phppgadmin{version}/` (recommended)
- [ ] OR include version in PR title
- [ ] Wait for automated tests
- [ ] Review test results
- [ ] Fix any failures
- [ ] Merge when all checks pass

---

## 🚨 Quick Troubleshooting

| Problem | Quick Fix |
|---------|-----------|
| No versions detected | Add version to PR title or `/bin` |
| Version not found | Check `releases.properties` on **main branch** |
| Download failed | Verify `GH_PAT` secret is configured |
| Tests failed | Check logs in Actions tab |

---

## 💡 Pro Tips

1. **Use /bin method** - Fastest testing (80% time saved)
2. **Include version in PR title** - Backup detection method
3. **Test locally first** - Save CI time
4. **Use manual trigger** - Test latest N versions or specific versions
5. **Check main branch** - releases.properties always from main
6. **PRs must specify versions** - No automatic fallback to latest versions

---

## 📊 What Gets Tested

### Required Files (must exist)
- ✅ `index.php`

### Optional Files (warning only)
- ⚠️ `conf/config.inc.php-dist`

### Tests Performed
1. Download from releases.properties (main branch)
2. Extract .7z archive
3. Verify directory structure
4. Check required files exist
5. Validate PHP syntax
6. Test configuration (if present)

---

## 🎯 Common Tasks

### Add New Version

```bash
mkdir -p bin/phppgadmin7.14.8
# Copy files to bin/phppgadmin7.14.8/
git add .
git commit -m "Add phpPgAdmin 7.14.8"
git push
# Create PR - tests only 7.14.8
```

### Update Existing Version

```bash
# Modify files in bin/phppgadmin7.14.7/
git add .
git commit -m "Update phpPgAdmin 7.14.7"
git push
# Create PR - tests only 7.14.7
```

### Test Multiple Versions

```bash
# Method 1: Add multiple to /bin
mkdir -p bin/phppgadmin7.14.7 bin/phppgadmin7.14.8
# Add files...
git commit -m "Add phpPgAdmin 7.14.7 and 7.14.8"
# Tests both versions

# Method 2: Manual trigger
# Actions → Run workflow → versions: 7.14.7,7.14.8
```

---

## 📖 Key Concepts

### Smart Detection
- Automatically finds which versions to test from PRs
- Reduces CI time by 80%
- Two detection methods: /bin changes and PR title
- No automatic fallback (prevents accidental testing)

### Main Branch Reference
- **Always uses releases.properties from main branch**
- Ensures stable URLs
- No broken links from PR changes

### Parallel Testing
- Multiple versions tested simultaneously
- Faster results
- Independent test runs

---

## 🔗 Quick Links

- **Full Documentation:** `docs/README.md`
- **Workflow File:** `.github/workflows/phppgadmin-test.yml`
- **Actions Tab:** Repository → Actions
- **Releases:** Repository → Releases

---

**Need Help?** Check `docs/README.md` for detailed documentation.

**Version:** 1.1.0  
**Last Updated:** 2025-01-15
