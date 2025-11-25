# Quick Start Guide - phpPgAdmin CI/CD

## 🚀 Quick Setup (5 minutes)

### 1. Configure GitHub Token

Create a Personal Access Token (PAT) with these permissions:
- ✅ `repo` - Full control of private repositories
- ✅ `workflow` - Update GitHub Action workflows

Add it to repository secrets as `GH_PAT`:
```
Settings → Secrets and variables → Actions → New repository secret
Name: GH_PAT
Value: ghp_your_token_here
```

### 2. Enable Auto-merge

```
Settings → General → Pull Requests → ✅ Allow auto-merge
```

### 3. Set Workflow Permissions

```
Settings → Actions → General → Workflow permissions
→ ✅ Read and write permissions
→ ✅ Allow GitHub Actions to create and approve pull requests
```

---

## 📦 Creating a New Release

### Step 1: Prepare Release Files
```bash
# Your .7z file should follow this naming pattern:
bearsampp-phppgadmin-{VERSION}-{DATE}.7z

# Example:
bearsampp-phppgadmin-7.14.7-2025.01.15.7z
```

### Step 2: Create Pre-release
1. Go to **Releases** → **Draft a new release**
2. Tag: `2025.01.15` (use date format)
3. Title: `phpPgAdmin Release 2025.01.15`
4. ✅ Check "Set as a pre-release"
5. Upload your `.7z` files
6. Click **Publish release**

### Step 3: Automatic Processing
The system will automatically:
- ✅ Extract version numbers from filenames
- ✅ Update `releases.properties`
- ✅ Create a PR
- ✅ Validate download links
- ✅ Auto-merge if validation passes

**Expected time:** 2-3 minutes

---

## 🔄 Creating a Pull Request

### Method 1: With /bin Changes (Recommended)

```bash
# 1. Create a branch
git checkout -b january-release

# 2. Add new version directories
mkdir -p bin/phppgadmin7.14.7
# Add your files to bin/phppgadmin7.14.7/

# 3. Update releases.properties
echo "7.14.7 = https://github.com/Bearsampp/module-phppgadmin/releases/download/2025.01.15/bearsampp-phppgadmin-7.14.7-2025.01.15.7z" >> releases.properties

# 4. Commit and push
git add .
git commit -m "Add phpPgAdmin 7.14.7"
git push origin january-release

# 5. Create PR on GitHub
```

**Result:** Tests will automatically run for version `7.14.7` only ✅

### Method 2: Using PR Title

If you can't modify `/bin` files:

**PR Title:** `Add phpPgAdmin 7.14.7 and 7.14.8`

**Result:** Tests will run for versions `7.14.7` and `7.14.8` ✅

### Method 3: Fallback (No Version Info)

If no versions detected:
- Tests will run for the **latest 5 versions** from `releases.properties`
- Takes longer but ensures compatibility

---

## 🧪 Manual Testing

### Test Specific Versions

1. Go to **Actions** → **phpPgAdmin Test**
2. Click **Run workflow**
3. Enter versions: `7.14.4,7.14.7`
4. Click **Run workflow**

### Test Latest N Versions

1. Go to **Actions** → **phpPgAdmin Test**
2. Click **Run workflow**
3. Leave versions empty
4. Set test_latest: `3`
5. Click **Run workflow**

---

## 📊 Understanding Test Results

### ✅ Success
```
Test Report: phpPgAdmin 7.14.7
==================================
Status: ✅ PASSED
Runner: Windows
Timestamp: 2025-01-15 10:30:00
==================================
```

### ❌ Failure
Check the logs for:
- Download errors → Verify URL in `releases.properties`
- Extraction errors → Check `.7z` file integrity
- Structure errors → Ensure required files exist
- Configuration errors → Validate config template

---

## 🔍 Version Detection Examples

### Example 1: /bin Changes
```
Changed files:
  bin/phppgadmin7.14.7/index.php
  bin/phppgadmin7.14.7/conf/config.inc.php-dist

Result: Tests version 7.14.7 only
```

### Example 2: PR Title
```
PR Title: "Update phpPgAdmin 7.14.4 and 7.14.7 configurations"

Result: Tests versions 7.14.4 and 7.14.7
```

### Example 3: Fallback
```
No versions detected in /bin or PR title

Result: Tests latest 5 versions from releases.properties
```

---

## 🛠️ Common Tasks

### Add a New Version

```bash
# 1. Create version directory
mkdir -p bin/phppgadmin7.14.8

# 2. Add files
cp -r source/* bin/phppgadmin7.14.8/

# 3. Update releases.properties
echo "7.14.8 = https://github.com/.../phppgadmin-7.14.8.7z" >> releases.properties

# 4. Commit and create PR
git add .
git commit -m "Add phpPgAdmin 7.14.8"
git push
```

### Update Existing Version

```bash
# 1. Modify files in version directory
vim bin/phppgadmin7.14.7/conf/config.inc.php-dist

# 2. Commit and create PR
git add .
git commit -m "Update phpPgAdmin 7.14.7 configuration"
git push
```

### Archive Old Version

```bash
# 1. Move to archived directory
mv bin/phppgadmin7.13.0 bin/archived/

# 2. Keep entry in releases.properties (for backward compatibility)

# 3. Commit
git add .
git commit -m "Archive phpPgAdmin 7.13.0"
git push
```

---

## 📋 Checklist for New Releases

- [ ] Create pre-release with date tag (e.g., `2025.01.15`)
- [ ] Upload `.7z` files with correct naming pattern
- [ ] Publish release
- [ ] Wait for auto-PR creation (2-3 minutes)
- [ ] Verify link validation passes
- [ ] Confirm auto-merge completes
- [ ] Check `releases.properties` updated on main branch

---

## 📋 Checklist for Pull Requests

- [ ] Add version directories to `/bin` (if applicable)
- [ ] Update `releases.properties` with correct URLs
- [ ] Include version numbers in PR title (if not using /bin)
- [ ] Wait for automated tests to complete
- [ ] Review test results
- [ ] Fix any failures
- [ ] Merge when all checks pass

---

## 🚨 Troubleshooting Quick Fixes

### "No versions detected"
→ Add version to PR title or `/bin` directory

### "Version not found in releases.properties"
→ Ensure version exists in `releases.properties` on your branch

### "Download failed - 404"
→ Check `GH_PAT` secret is configured and release exists

### "Link validation failed"
→ Verify URLs are accessible and assets are uploaded

### "Tests failed"
→ Check logs for specific error and fix the issue

---

## 📚 More Information

- **Full Documentation:** [README.md](README.md)
- **Workflow Files:** `.github/workflows/`
- **Issues:** GitHub Issues tab
- **Support:** Contact maintainers

---

## 💡 Pro Tips

1. **Use pre-releases** for testing before full release
2. **Include version numbers** in PR titles for faster testing
3. **Test locally** before pushing to save CI time
4. **Check logs** immediately if tests fail
5. **Keep releases.properties sorted** (newest first)

---

**Need Help?** Check the [full documentation](README.md) or open an issue!
