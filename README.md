# .agents

**Automated workflows and instructions for Knowledge Nexus quality assurance.**

This repository contains GitHub Actions workflows that validate external links, check file integrity, and audit documentation completeness. Use these as templates to automate your Knowledge Nexus maintenance.

---

## Quick Start

### Copy these workflows into Knowledge Nexus

1. In your `Knowledge-Nexus` repository, create this directory structure:
   ```
   Knowledge-Nexus/
   └─ .github/
      └─ workflows/
   ```

2. Copy the three YAML files from this repo into `.github/workflows/`:
   - `validate-links.yml`
   - `validate-files.yml`
   - `audit-documentation.yml`

3. Push to `main` → workflows run automatically

4. View results in GitHub → **Actions** tab

---

## Workflow 1: Validate External Links

**File:** `validate-links.yml`

**What it does:**
- Checks all source links are still accessible
- Tests: CDC, APA, Solventum, DoH, ORCID
- Reports broken or redirected links

**When it runs:**
- On every push to `main`
- On every pull request

**How to use:**
1. Copy `validate-links.yml` to `.github/workflows/`
2. Customize the `sources` array with your links
3. Push and watch it run in Actions tab

---

## Workflow 2: Check File Integrity

**File:** `validate-files.yml`

**What it does:**
- Scans all HTML files for truncation or corruption
- Checks for: doctype, closing `</html>`, charset, file size
- Alerts if files are incomplete or unusually small

**When it runs:**
- On every push to `main`
- On every pull request

**How to use:**
1. Copy `validate-files.yml` to `.github/workflows/`
2. Customize the file path patterns if needed (currently scans `*.html` in root)
3. Push and check results in Actions

---

## Workflow 3: Audit Documentation Completeness

**File:** `audit-documentation.yml`

**What it does:**
- Verifies all modules in README.md have corresponding `.html` files
- Checks every documented page exists
- Ensures consistency between docs and code

**When it runs:**
- On every push to `main`
- On every pull request
- Scheduled: Weekly on Mondays at 09:00 UTC

**How to use:**
1. Copy `audit-documentation.yml` to `.github/workflows/`
2. Update the `modules` array to match your pages
3. Push and let it audit automatically

---

## Customization Guide

### Add more link validations

In `validate-links.yml`, add URLs to the `sources` array:

```bash
declare -a sources=(
  "https://your-link-here.com|Description"
  "https://another-source.com|Another Description"
)
```

### Monitor specific HTML files

In `validate-files.yml`, modify the file search:

```bash
for file in *.html; do  # Change this pattern
```

Examples:
- `*.html` — all HTML files in root
- `modules/*.html` — only files in modules/ folder
- `drgs-compendium.html psychiatry-intro.html` — specific files

### Update module list

In `audit-documentation.yml`, update the modules array:

```bash
declare -a modules=(
  "filename.html|Module Display Name"
)
```

---

## Manual Workflow Execution

You don't have to wait for a push to trigger workflows. You can run them manually:

1. Go to your Knowledge Nexus repository
2. Click **Actions** tab
3. Select the workflow (e.g., "Validate External Links")
4. Click **Run workflow** button
5. Watch logs in real-time

---

## Reading Workflow Results

### In GitHub Actions Tab:
1. Click **Actions**
2. Select the workflow run
3. Click the job name
4. Scroll through logs to see:
   - ✓ = Passed
   - ✗ = Failed
   - ⚠ = Warning

### If a workflow fails:
- Scroll up in logs to find the issue
- Fix the problem (broken link, missing file, etc.)
- Push again → workflow runs automatically

---

## If Claude Needs to Execute These Tasks

If you run out of tokens during automation setup, provide Claude with:

1. **Your Knowledge Nexus repository link:** `https://github.com/sherifalattar/knowledge-nexus`
2. **Which workflow you need:** (links, files, or documentation)
3. **Any customizations:** (specific URLs, file paths, module names)

Then ask Claude to:
- Copy the workflow YAML from this repo into Knowledge Nexus `.github/workflows/`
- Customize the arrays (sources, files, modules) with your specific data
- Push to main branch

Claude can then execute the workflow by creating the files directly in your repo.

---

## Troubleshooting

### Workflow doesn't run after push
- Check that files are in `.github/workflows/` (not `.workflows/`)
- Verify YAML syntax is correct (no tabs, proper indentation)
- Push to `main` branch specifically

### Link validation fails but links work
- Some sites block automated requests
- Add `--max-time 10` to curl to increase timeout
- Use `curl -I` (header-only) instead of full request

### File integrity check reports false positives
- Small files (< 2000 bytes) trigger warnings
- Adjust the size threshold in the script: `if [ "$size" -lt 2000 ]`
- Change to your preferred minimum size

### Documentation audit can't find files
- Verify file names match exactly (case-sensitive)
- Check paths are relative to repo root
- Update the `modules` array if you renamed files

---

## Next Steps

1. **Copy these workflows into Knowledge Nexus** (as described above)
2. **Customize them** for your specific links, files, and modules
3. **Push to main** and watch them run
4. **View results** in the Actions tab
5. **Iterate** — add more checks as needed

You now have **fully automated quality assurance for Knowledge Nexus.**

Every push will validate your sources, check file integrity, and audit documentation — without any manual effort.
