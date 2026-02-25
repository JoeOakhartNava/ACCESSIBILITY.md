# TRUSTED_SOURCES.yaml Maintenance

## Overview

This repository includes an automated monthly maintenance workflow for `examples/TRUSTED_SOURCES.yaml` to ensure the list of trusted accessibility resources remains high-quality and up-to-date.

## Features

The maintenance script (`.github/scripts/maintain_trusted_sources.py`) performs:

### 1. URL Validation
- Checks all URLs for accessibility (404 detection)
- **First 404**: Marks source as "not active"
- **Second 404**: Removes the entry entirely
- Tracks error history in `.github/data/url_error_history.json`

### 2. Metadata Enrichment
- **`owner`**: Detects from domain name or description patterns
- **`license`**: Attempts to detect common licenses (Creative Commons, MIT, Apache, GPL, etc.)
  - Sets to "unknown" if not found (never leaves as `null`)
- **`last_reviewed`**: Updates timestamp when metadata is reviewed

### 3. Content Freshness Check
- Checks Last-Modified headers to detect site updates
- Marks as "not active" if no content updates in over 1 year

### 4. Quality Validation
- Validates YAML structure
- Ensures required fields are present
- Checks for valid status values

## Workflow Schedule

The workflow runs:
- **Automatically**: Monthly on the 1st at 00:00 UTC
- **Manually**: Via workflow_dispatch with options:
  - `full_scan`: Perform annual topic_tags update
  - `skip_validation`: Skip URL checks (metadata only)

## How It Works

1. **Automated Execution**: GitHub Action runs monthly
2. **Changes Detection**: Script validates URLs and enriches metadata
3. **Pull Request Creation**: If changes are found, creates a PR for review
4. **Human Review**: Maintainer reviews and merges the PR

## Manual Execution

### Run Full Scan (Annual Topic Update)
```bash
python .github/scripts/maintain_trusted_sources.py --full-scan
```

### Validate Only (No Changes)
```bash
python .github/scripts/maintain_trusted_sources.py --validate-only
```

### Skip URL Validation (Metadata Only)
```bash
python .github/scripts/maintain_trusted_sources.py --skip-validation
```

## Requirements

- Python 3.11+
- Dependencies in `.github/scripts/requirements.txt`:
  - PyYAML
  - requests

## Error Tracking

The script maintains a history file at `.github/data/url_error_history.json`:

```json
{
  "errors": {
    "source-id": [
      {
        "date": "2026-02-25T12:00:00",
        "status_code": 404,
        "error": "404 Not Found"
      }
    ]
  }
}
```

## Respecting Content Creator Preferences

The script respects the `ai_scraping` field in TRUSTED_SOURCES.yaml:
- `allowed` (default): Content can be used
- `prohibited`: Do not scrape/train on content
- `restricted`: Use only for reference/citation

## Quality Assurance

### What the Script Does
- ✅ Validates all URLs monthly
- ✅ Tracks 404 errors with two-strike removal
- ✅ Enriches missing metadata
- ✅ Detects content staleness
- ✅ Creates PRs for human review

### What Humans Should Review
- ⚠️ Verify sources marked "not active" are truly inactive
- ⚠️ Confirm removed sources should be deleted
- ⚠️ Check accuracy of detected metadata (owner, license)
- ⚠️ Add context for topic_tags during annual scans

## Future Enhancements

Potential improvements:
1. **Topic Tags**: Implement ML/NLP analysis for automatic tag suggestions
2. **RSS/Atom**: Check blog feeds for recent posts
3. **Archive.org**: Suggest archived versions for removed content
4. **WCAG Version**: Parse content for WCAG 2.1/2.2/3.0 references
5. **Authority Scoring**: Analyze citations, backlinks, author credentials

## Related Files

- Workflow: `.github/workflows/maintain-trusted-sources.yml`
- Script: `.github/scripts/maintain_trusted_sources.py`
- Data: `examples/TRUSTED_SOURCES.yaml`
- History: `.github/data/url_error_history.json`

## Support

For issues or questions about the maintenance workflow, please:
1. Check existing GitHub Issues
2. Review the workflow logs in GitHub Actions
3. Open a new issue with the `maintenance` label
