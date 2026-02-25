# TRUSTED_SOURCES.yaml Maintenance Implementation Summary

## Overview

This implementation provides a comprehensive, automated maintenance system for `examples/TRUSTED_SOURCES.yaml`, ensuring the list of trusted accessibility resources remains high-quality, accurate, and up-to-date.

## Problem Statement Requirements Met

✅ **GitHub Action runs monthly** - Implemented via cron schedule  
✅ **404 detection** - First occurrence marks as "not active", second removes entry  
✅ **Error history tracking** - Stored in `.github/data/url_error_history.json`  
✅ **Metadata enrichment**:
- `owner` field filled from domain/description analysis
- `license` field set to "unknown" if not found (never null)
- `topic_tags` populated during annual scans
- `last_reviewed` timestamp tracks when metadata was updated
✅ **Content freshness detection** - Sources marked "not active" if no updates >1 year  
✅ **Quality validation** - Structure checks, duplicate detection, required fields  

## Architecture

### Components

1. **GitHub Action Workflow** (`.github/workflows/maintain-trusted-sources.yml`)
   - Runs monthly on the 1st at 00:00 UTC
   - Manual trigger with options (full_scan, skip_validation)
   - Creates PRs for review (never auto-merges)
   - 110 lines

2. **Python Maintenance Script** (`.github/scripts/maintain_trusted_sources.py`)
   - URL validation with retry logic
   - Metadata detection algorithms
   - Content freshness checking
   - Topic tag suggestions
   - Duplicate detection
   - 476 lines

3. **Unit Tests** (`.github/scripts/test_maintenance.py`)
   - Tests for owner detection
   - Tests for topic tag suggestions
   - Tests for validation logic
   - 100% pass rate
   - 219 lines

4. **Documentation**
   - Main guide: `.github/TRUSTED_SOURCES_MAINTENANCE.md` (132 lines)
   - Quick reference: `.github/scripts/README.md` (80 lines)
   - Updated: `README.md`, `index.md`, `examples/README.md`

## Features

### URL Validation
- Checks all URLs for 404 errors
- Tracks error history per source
- Two-strike removal policy
- Rate limiting and respectful delays

### Metadata Enrichment
- **Owner detection**: Extracts from domain name or description patterns
- **License detection**: Scans content for common licenses (CC, MIT, Apache, GPL)
- **Topic tags**: Intelligent suggestions based on keywords (17 topic categories)
- **Last reviewed**: Timestamps when metadata was last updated
- **Content freshness**: Checks Last-Modified headers

### Quality Assurance
- YAML structure validation
- Required field checking
- Duplicate ID detection
- Duplicate URL detection
- Status value validation

## Usage

### Command Line

```bash
# Validate only (no changes)
python .github/scripts/maintain_trusted_sources.py --validate-only

# Monthly maintenance (default)
python .github/scripts/maintain_trusted_sources.py

# Annual full scan (includes topic tags)
python .github/scripts/maintain_trusted_sources.py --full-scan

# Metadata only (skip URL validation)
python .github/scripts/maintain_trusted_sources.py --skip-validation
```

### GitHub Actions

- **Automatic**: Monthly on the 1st
- **Manual**: Via Actions tab → "Maintain TRUSTED_SOURCES.yaml" → Run workflow
  - Option: Full scan (annual topic update)
  - Option: Skip validation (metadata only)

## Safety Features

- ✅ All changes create PRs (human review required)
- ✅ Two-strike policy before removing entries
- ✅ Error history tracked for auditing
- ✅ Respects `ai_scraping` preferences
- ✅ Rate limiting between requests
- ✅ Comprehensive validation before changes
- ✅ Dry-run capability (--validate-only)

## Testing

```bash
# Run unit tests
python .github/scripts/test_maintenance.py

# Results: 3/3 test suites passed
# - detect_owner: 3/3 tests ✓
# - suggest_topic_tags: 3/3 tests ✓
# - validate_yaml_structure: 3/3 tests ✓
```

## File Structure

```
.github/
├── workflows/
│   └── maintain-trusted-sources.yml    # GitHub Action (monthly)
├── scripts/
│   ├── maintain_trusted_sources.py     # Main script (476 lines)
│   ├── test_maintenance.py             # Unit tests (219 lines)
│   ├── requirements.txt                # Python deps (PyYAML, requests)
│   └── README.md                       # Quick reference (80 lines)
├── data/
│   ├── .gitkeep                        # Directory marker
│   └── url_error_history.json          # Created by script (tracked in git)
└── TRUSTED_SOURCES_MAINTENANCE.md      # Full documentation (132 lines)
```

## Topic Tag Categories

The script can suggest tags from 17 categories:
- wcag, aria, forms, color-contrast, keyboard
- screen-readers, testing, design, development
- inclusive-design, captions, audio-description
- pdf, mobile, web-standards, policy, training

## Data Quality Improvements

### Automatic
- Broken link detection and removal
- Missing metadata filled in
- Stale content marked inactive
- Duplicates prevented

### Manual (PR Review)
- Verify suggested topic tags
- Confirm owner/license accuracy
- Review status changes
- Approve removals

## Future Enhancements

Documented in `.github/TRUSTED_SOURCES_MAINTENANCE.md`:
1. ML/NLP for sophisticated topic analysis
2. RSS/Atom feed checking for content freshness
3. Archive.org integration for removed content
4. WCAG version detection from content
5. Authority scoring (citations, backlinks, credentials)

## Configuration

### Workflow Schedule
- Monthly: `cron: '0 0 1 * *'` (1st of month, 00:00 UTC)
- Can be changed in workflow file

### Script Settings
```python
REQUEST_TIMEOUT = 10        # seconds
REQUEST_DELAY = 1           # seconds between requests
USER_AGENT = "..."          # identifies bot to websites
```

## Monitoring

- GitHub Actions tab shows workflow runs
- PR descriptions include summary of changes
- Error history file shows 404 tracking
- Validation errors reported in workflow logs

## Integration Points

- ✅ Respects existing `ai_scraping` field
- ✅ Preserves manual edits (authority_level, jurisdiction, etc.)
- ✅ Works with wai-yaml-ld references
- ✅ Compatible with link-check workflow

## Success Metrics

- **Automated**: 7 files added, 1017 lines of code
- **Testing**: 100% unit test pass rate
- **Documentation**: 4 files updated with comprehensive guides
- **Quality**: Validates 143 sources, detects duplicates, tracks errors
- **Safety**: Two-strike policy, human review, audit trail

## Implementation Complete

All requirements from the problem statement have been addressed:
1. ✅ Monthly GitHub Action
2. ✅ 404 detection with two-strike removal
3. ✅ Metadata enrichment (owner, license, topic_tags, last_reviewed)
4. ✅ Content freshness detection
5. ✅ Quality validation and maintenance
6. ✅ Comprehensive documentation and testing

The system is production-ready and will run on the 1st of next month.
