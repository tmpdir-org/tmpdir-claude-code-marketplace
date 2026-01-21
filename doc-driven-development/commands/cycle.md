Run the complete TDD implementation cycle for the **IN PROGRESS** plan.

This command chains together the full development workflow:

1. Implement tests first (TDD)
2. Implement the code to pass the tests
3. Update documentation to reflect changes
4. Perform comprehensive code review

---

## Phase 1: Verify Active Plan

1. Read `plans/plans.md` to find the plan marked **IN PROGRESS**.
2. If no plan is in progress, inform the user and suggest running `/plan` first.
3. Read the plan file to understand the scope of work.

---

## Phase 2: Implement Tests

Implement tests for the **IN PROGRESS** plan, creating logical git commits for
each unit of work.

### Step 2.1: Plan Test Units

Analyze the plan and break the implementation into logical units. Each unit
should be:

- A single function, method, or component
- A cohesive set of related changes
- Small enough to understand in one commit

Create or edit a commit table in the **IN PROGRESS** plan file that tracks these
logical units, what has or has not been implemented, and the hash if it has been
implemented, so that you can see what needs to be done.

### Step 2.2: Implement and Commit Each Test Unit

For each test unit:

1. Write the tests for that unit.
2. Stage the changes with `git add`.
3. Create a commit using conventional commit format:
   - `test:` for new tests
   - `test: fix` for fixing broken tests
   - Commits should be done in the developer's name, never Claude's.
4. Capture the commit hash using `git rev-parse --short HEAD`.
5. Add the commit to the plan file's `## Commits` section.

The `## Commits` section in the plan file uses this format:

```markdown
## Commits

| Hash | Description | Status |
|------|-------------|--------|
| abc1234 | test: user authentication module | Implemented |
| def5678 | test: login form component | Not Implemented |
```

### Step 2.3: Finalize Tests

Update the plan file with any test implementation details. Keep the plan status
as **IN PROGRESS** (tests alone don't complete a plan).

---

## Phase 3: Implement Code

Implement code changes for the **IN PROGRESS** plan, creating logical git
commits for each unit of work.

### Step 3.1: Plan Implementation Units

Analyze the plan and break the implementation into logical units. Each unit
should be:

- A single function, method, or component
- A cohesive set of related changes
- Small enough to understand in one commit

Update the commit table in the **IN PROGRESS** plan file to track implementation
units.

### Step 3.2: Implement and Commit Each Unit

For each implementation unit:

1. Make the code changes for that unit.
2. Stage the changes with `git add`.
3. Create a commit using conventional commit format:
   - `feat:` for new features
   - `fix:` for bug fixes
   - `refactor:` for code restructuring
   - `docs:` for documentation changes
   - Commits should be done in the developer's name, never Claude's.
4. Capture the commit hash using `git rev-parse --short HEAD`.
5. Add the commit to the plan file's `## Commits` section.

### Step 3.3: Finalize Implementation

1. Update the plan file with any implementation details or deviations.
2. Change the status in the plan file to `Implemented`.

---

## Phase 4: Update Documentation

Update any relevant documentation to accurately reflect the code changes since
the starting hash for this plan recorded in the `plans/plans.md` file.

### Documentation Guidelines

- Be concise. AI tends to generate very verbose/wordy documentation. Don't do
  that.
- Don't add trivial examples that users can easily figure out.
- Avoid duplicated information when possible.
- The first word in a list item should always be capitalized.
- Sentences (or close to it) in a list should always end with a period.

### Step 4.1: Update Project Documentation

- Update `CLAUDE.md` with any architectural or technical details.
- Update `README.md` if user-facing behavior changed.

### Step 4.2: Update Changelog

Update the `CHANGELOG.md` to describe changes as benefits to the user. It should
not include technical details of the change - that is what `git log` is for.

### Step 4.3: Update Plan File

Review all changes since the starting hash for this plan and make sure the plan
accurately reflects these changes. The plan should document:

- What was implemented.
- Technical decisions and their rationale.
- Any deviations from the original plan.

Do not add excessive examples to the plan file.

---

## Phase 5: Code Review

Perform a comprehensive code review on changes using specialized review agents.

### Step 5.1: Gather Context

First, verify the current directory is a git repository. If not, inform the user
and exit.

Check `plans/plans.md` to find the **IN PROGRESS** file, and get its starting
commit hash. Run git diff on this hash to see what has changed. Also check
whether a `FORMAT.md` file exists in the repository root.

### Step 5.2: Launch Review Agents

Use the Task tool to spawn the following review agents **in parallel**. Each
agent analyzes the git diff output and provides findings.

#### Agent 1: Formatting Review

**Focus**: Code formatting consistency

Instructions:

1. Check if `FORMAT.md` exists in the repository root.
2. If missing, analyze the codebase to identify formatting patterns
   (indentation, naming conventions, bracket styles, line lengths). Before
   creating `FORMAT.md`, check again that it doesn't exist to avoid race
   conditions with concurrent reviews.
3. Review all changed code against the formatting standards.
4. Report any formatting inconsistencies.

Output format:

```
## Formatting Review
- [List of formatting issues found, or "No issues found"]
- [If FORMAT.md was created, note this]
```

#### Agent 2: Architectural Consistency Review

**Focus**: Adherence to documented architecture

Instructions:

1. Read README.md and any architecture documentation (ARCHITECTURE.md,
   DESIGN.md, CLAUDE.md).
2. Identify documented architectural decisions (module boundaries, patterns,
   conventions).
3. Review changed code for violations of these patterns.
4. Report any architectural inconsistencies.

Output format:

```
## Architectural Consistency Review
- [List of architectural concerns, or "Code follows documented architecture"]
```

#### Agent 3: Documentation Accuracy Review

**Focus**: Ensuring documentation matches code

Instructions:

1. Compare code changes to README.md, CHANGELOG.md, and the **IN PROGRESS** plan
   file.
2. Identify any code functionality not reflected in documentation.
3. Identify any documentation claims not supported by the code.
4. Identify any features marked as implemented that are not implemented.
5. Identify any inconsistent, contradictory, or outdated documentation.

Output format:

```
## Documentation Accuracy Review
- [List of documentation gaps or inconsistencies, or "Documentation is accurate"]
```

#### Agent 4: Potential Bugs Review

**Focus**: Identifying potential bugs and edge cases

Instructions:

1. Analyze changed code for common bug patterns.
2. Look for: null/undefined handling, boundary conditions, race conditions,
   resource leaks, error handling gaps.
3. Consider edge cases that may not be handled.
4. Note any security concerns.

Output format:

```
## Potential Bugs Review
- [List of potential bugs with severity and location]
- [Each bug should include: description, location, suggested fix]
```

#### Agent 5: Code Clarity Review

**Focus**: Code clarity and design decisions

Instructions:

1. For each significant code change, first justify why the code is structured
   this way. Significant changes include: new functions/methods, modified logic
   flow, new classes/modules, or changes exceeding 10 lines. Exclude trivial
   changes like import reordering, whitespace, or simple renames.
2. Then critically evaluate: validate the reasoning or suggest improvements with
   rationale.
3. Alternate between advocate and critic perspectives until all significant code
   is reviewed.

Output format:

```
## Code Clarity Review

### [File/Function Name]
**Advocate**: [Justification for code design]
**Critic**: [Validation or improvement suggestion]
```

#### Agent 6: Comments Review

**Focus**: Quality and usefulness of code comments

Instructions:

1. Review all comments in changed code (function docs, inline comments).
2. Assess whether comments add value beyond what code already expresses.
3. Identify missing comments where complexity warrants explanation.
4. Flag redundant or misleading comments.

Output format:

```
## Comments Review
- [List of comment improvements needed, or "Comments are appropriate"]
```

### Step 5.3: Consolidate Results

After all agents complete, append all review output to the **IN PROGRESS** plan
file under a `## Review (<current-date>)` heading. Include:

1. A summary of issues found per category.
2. All agent outputs in full.
3. Highlight any critical issues requiring immediate attention.
4. Note if FORMAT.md was auto-generated.

### Step 5.4: Commit Decision

Create a commit and auto-commit with a message summarizing what was implemented
and key review findings.

### Step 5.5: Completion Decision

After handling the commit decision:

1. Display a summary of what was implemented vs what remains in the plan.
2. Ask the user if the plan should be:
   - **Completed** - Mark the plan as Completed in `plans/plans.md`
     (implementation is done)
   - **Continue** - Keep the plan as **IN PROGRESS** for further iteration

If Completed is selected, update `plans/plans.md` to change the plan status from
**IN PROGRESS** to Completed.

---

## Notes

- Each phase builds on the previous; if any phase fails, stop and address the
  issue.
- Commits are created incrementally throughout the process.
- The plan file tracks all commits and serves as the source of truth.
- Documentation drives code changes in this workflow, not the other way around.
- Commits should be done in the developer's name, never Claude's.
