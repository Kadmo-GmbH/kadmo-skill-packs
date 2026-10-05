# Attribution

This module merges and adapts content from two MIT-licensed open-source projects:

## humanizer

- **Repository:** [blader/humanizer](https://github.com/blader/humanizer)
- **Author:** Siqi Chen
- **License:** MIT (Copyright 2025 Siqi Chen)
- **Contribution:** 29 AI writing pattern definitions, two-pass audit process, voice calibration methodology
- **Source:** Based on Wikipedia's "Signs of AI writing" page maintained by WikiProject AI Cleanup

## stop-slop

- **Repository:** [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop)
- **Author:** Hardik Pandya
- **License:** MIT (Copyright 2025 Hardik Pandya)
- **Contribution:** 5-dimension scoring system (Directness, Rhythm, Trust, Authenticity, Density), 35/50 threshold, banned phrases, banned structural patterns, 8 core editing rules, 12-item quick checks

## Upstream Sync

This module is structured to allow tracking of upstream changes. To sync:

1. Compare `writing-quality/_skill.md` against the latest SKILL.md from each source repo
2. Compare `references/banned-patterns.md` against humanizer's pattern list
3. Compare `references/banned-phrases.md` and `references/banned-structures.md` against stop-slop's reference files
4. Merge new patterns or rules, preserving this module's SideButton formatting

Both source repos are actively maintained. Check for new versions periodically.

## Relationship to plugin-writing-quality

The `sidebutton/plugin-writing-quality` repo provides a standalone MCP plugin with deterministic JS pattern checking + LLM scoring. It uses the upstream repos as git submodules directly.

This skill pack module adapts the same upstream content into LLM-consumable reference files with additional calibration:
- Landing-page content-type exceptions (Pattern 10, 29, S3, Metronomic)
- Content-type-aware scoring adjustments (Rhythm, Trust, Authenticity for landing pages)
- Calibrated thresholds (28/50 for landing pages, 35/50 for prose)

When updating either, sync the calibration changes to the other:
- Skill pack references → plugin's `scoring-prompt.js` content-type logic
- Plugin's upstream submodule updates → skill pack reference files
