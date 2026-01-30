Automate the creation of git tags for releases.

Steps:

1. Save the current branch name:
   - Run `git branch --show-current` to get the current branch
   - Store this for switching back later

2. Check for uncommitted changes:
   - If there are uncommitted changes, warn the user and stop

3. Switch to main/master branch:
   - Run `git checkout main` (or `git checkout master` if main doesn't exist)

4. Pull the latest changes:
   - Run `git pull` to ensure we have the latest commits

5. Read the CHANGELOG.md file to extract the latest version:
   - Look for the most recent `## [X.Y.Z]` entry (not `## [Unreleased]`)
   - Extract the version number (e.g., "1.0.7" from `## [1.0.7] - 2026-01-30`)

6. Check if the tag already exists:
   - Run `git tag -l "vX.Y.Z"` to check if the tag exists
   - If it exists, inform the user and stop

7. Create the git tag:
   - Run `git tag vX.Y.Z` (prefix version with "v")

8. Push the tag to the remote:
   - Run `git push origin vX.Y.Z`

9. Switch back to the original branch:
   - Run `git checkout <original-branch>` using the branch name saved in step 1

10. Provide a summary showing:
    - The tag that was created
    - The version it corresponds to
    - Confirmation that the tag was pushed
    - The branch you switched back to

Important: This command assumes the release has already been committed. Run
`/release` first to update version numbers and changelog, then commit those
changes before running `/tag`.
