Automate the release process by updating the changelog, manifest files, and
creating a git tag.

Steps:

1. Read @CLAUDE.md to find the list of manifest files under the "Release
   Management" section. These files contain version numbers that need to be
   updated.

2. Read the CHANGELOG.md file to find:
   - The latest version number (look for `## [X.Y.Z]` format)
   - Any `## [Unreleased]` section that needs to be converted

3. Determine the release version:
   - If "$ARGUMENTS" are provided, use that as the version number (e.g.,
     `/release 2.0.0`)
   - If an `## [Unreleased]` section exists, calculate the next version by
     incrementing the patch version (e.g., 1.0.6 → 1.0.7)
   - If no `## [Unreleased]` section exists, use the latest version found in the
     changelog

4. Update CHANGELOG.md:
   - Replace `## [Unreleased]` with
     `## [X.Y.Z] - <current date in YYYY-MM-DD format>`
   - Keep all content under that section unchanged
   - Add a `## [Unreleased]` section at the top for new changes.

5. Update version in all manifest files listed in CLAUDE.md:
   - Search for version fields in each manifest file (patterns like
     `"version":`, `version =`, `version:`, etc.)
   - Update the version value to match the release version
   - Preserve all other fields and formatting

6. Provide a summary showing:
   - The version that was released
   - The date applied to the changelog
   - Which manifest files were updated
   - Remind the user to commit the changes, tag and push with `git push --tags`

Important: Do not push tags or commit changes automatically. Let the user review
and push when ready.
