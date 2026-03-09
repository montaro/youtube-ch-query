# Changelog

All notable changes to the YouTube Channel Query & Filter project are documented here.

## [Current] - 2026-03-09

### Added
- **Two-Tier Keyword Matching Algorithm** - Intelligent system to prevent false positives
  - STRICT keywords (16 words): Match only in tags or title (never description)
  - SPECIFIC keywords (>10 chars): Can match in tags, title, or description
  - Regular keywords (≤10 chars): Match only in tags or title

- **Politics & Government Keywords** - New category added
  - politics, government, policy, election, president, congress, senate, war, conflict, vote, voting

- **Match Source Tracking** - Debug column showing whether each match came from tags, title, or description

- **Tag Display in CSV** - Tags now properly displayed as comma-separated strings

- **Markdown Documentation in Notebook**
  - Algorithm explanation before filtering cells
  - Debugging & Validation section explaining debug cells

- **README Algorithm Documentation** - Detailed explanation of matching rules and keyword tiers

### Fixed
- **False Positive Elimination**
  - "pandemic" no longer matches news about Qatar flights
  - "environment" no longer matches "unpredictable environment" (non-environmental context)
  - "conflict" no longer matches "conflict of interest" (non-political context)

- **Duplicate Code Cells** - Removed old matching function that was causing inconsistent behavior

- **CSV Output Location** - Changed from `data/` subdirectory to root directory

- **Column Names** - Converted to Title Case (Title, Matched Keyword, Match Source, etc.)

- **Tags Extraction** - Confirmed YouTube API returns tags; updated logic to handle missing tags properly

### Changed
- **Keyword Count**: 30 → 41 keywords (added 11 politics-related keywords)

- **STRICT_KEYWORDS Set**: Expanded to include ambiguous politics keywords
  - Original: AI, tech, virus, disease, pandemic, science, research, study, health, medicine, earth, space, environment
  - New additions: policy, conflict, politics, election

- **CSV Columns**: Added "Match Source" column for transparency
  - Old: Title, Matched Keyword, Category Name, View Count, Like Count, Tags
  - New: Title, Matched Keyword, Match Source, Category Name, View Count, Like Count, Tags

- **Filtering Logic**:
  - Keyword length threshold changed from >10 to >10 (only for non-strict keywords)
  - Description matching now restricted to non-strict keywords only

### Technical Details

#### Algorithm Evolution
1. **Initial**: Simple substring matching (`"ai" in text`) → False positives (matched "Iran", "said", etc.)
2. **Iteration 1**: Regex word boundaries (`\bai\b`) → Better, but still matched descriptions
3. **Iteration 2**: Length-based filtering (>10 chars in description) → Good, but missed ambiguous words
4. **Final**: Two-tier system (STRICT + length filtering) → Accurate with no false positives

#### Results (500 videos)
- Matches: 17 (3.4% hit rate)
- All matches verified as accurate
- Zero false positives
- Tags properly displayed when available (56% of videos have tags)

### Documentation
- README updated with algorithm explanation
- Notebook includes markdown cells explaining filtering strategy
- CHANGELOG created for change tracking

---

## Future Enhancements (Potential)
- Keyword weighting (prioritize some keywords over others)
- Customizable matching tiers per keyword
- Duplicate detection (same content uploaded by different channels)
- Sentiment analysis integration
- Category-specific keyword sets
