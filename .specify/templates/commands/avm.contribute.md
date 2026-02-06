---
description: "Prepare for an AVM contribution by reviewing issues, owners, and guidelines"
tools: ["fetch", "github-pull-request_issue_fetch", "github-pull-request_doSearch", "github-pull-request_formSearchQuery", "todos"]
---

# AVM Contribution Preparation

You are helping an AVM contributor prepare to work on an issue or feature. This command ensures they don't duplicate work, follow guidelines, and coordinate appropriately.

## Input Required

Ask the user for:
1. **Issue link** (GitHub issue URL) OR
2. **Description** of what they want to contribute

## Step 1: Issue Analysis

If an issue link is provided, analyze it:

```
**Issue:** [ISSUE_TITLE]
**Status:** Open | Closed | In Progress
**Labels:** [list labels]
**Assignees:** [list or "None"]
**Created:** [date]
**Last Activity:** [date]
```

### Check for Existing Work

Search for related PRs:
- Use `github-pull-request_formSearchQuery` with: "is:pr [issue keywords]"
- Check if PRs reference this issue number
- Look for "WIP", "Draft", or recent activity

Report:
```
## Existing Work Check

| Finding | Status |
|---------|--------|
| Related PRs | [list any, or "None found"] |
| Someone assigned? | Yes/No |
| Recent comments indicating work? | Yes/No |
| **Safe to proceed?** | ✅ Yes / ⚠️ Coordinate first |
```

## Step 2: Identify Module Owners

Determine who owns the affected code:

1. **Check CODEOWNERS file** in the repo
2. **Look at recent commits** to the affected files
3. **Check issue labels** for team assignments

Report:
```
## Module Owners

| Role | Person | How to Contact |
|------|--------|----------------|
| Primary Owner | @username | Tag in PR |
| Secondary Owner | @username | CC on PR |
| Recent Contributors | @user1, @user2 | Consider CC |

**Coordination needed?** [Yes - reach out first / No - proceed with PR]
```

## Step 3: Contribution Guidelines

Gather relevant guidelines:

1. **CONTRIBUTING.md** in the repo
2. **AVM Specifications** relevant to the module type
3. **Module-specific requirements** (README, ADR)

Report:
```
## Guidelines to Follow

### Repository Rules
- [ ] [Key requirement from CONTRIBUTING.md]
- [ ] [Another requirement]

### AVM Specifications
- [ ] Module type: [RES/PTN/UTL]
- [ ] Relevant specs: [BCPNFR###, SNFR###]
- [ ] Required tests: [defaults, waf-aligned, etc.]

### Module-Specific
- [ ] [Any ADR decisions to honor]
- [ ] [Existing patterns to follow]
```

## Step 4: Scope Analysis

Help prevent scope creep:

```
## Scope Analysis

### Issue Requests
[What the issue actually asks for]

### Minimum Viable Change
[The smallest change that addresses the issue]

### Potential Scope Creep
⚠️ Avoid these unless explicitly requested:
- [Related but out-of-scope change 1]
- [Related but out-of-scope change 2]

### Recommended Approach
[Suggested implementation approach]
```

## Step 5: Pre-PR Checklist

Create a checklist for the contributor:

```
## Before Submitting PR

### Coordination
- [ ] No duplicate work in progress
- [ ] Owners notified (if needed)
- [ ] Issue linked in PR description

### Code Quality
- [ ] Follows naming conventions (constitution)
- [ ] Has required decorators/descriptions
- [ ] No hardcoded values

### Testing
- [ ] Existing tests still pass
- [ ] New tests added (if needed)
- [ ] `bicep build` passes
- [ ] `bicep lint` passes

### Documentation
- [ ] CHANGELOG.md updated
- [ ] version.json updated (if version bump)
- [ ] PR description explains the change

### Ready for Review
- [ ] Self-reviewed the diff
- [ ] No unrelated changes included
- [ ] Scope matches issue request
```

## Step 6: Track with Todos

Use `#todos` to create the action list:
- [ ] Issue analyzed
- [ ] Checked for existing work
- [ ] Owners identified
- [ ] Guidelines gathered
- [ ] Scope defined
- [ ] Ready to implement

---

## When User Says "Review my PR"

Switch to PR review mode:

1. **Ask for PR link or diff**
2. **Compare against the issue requirements**
3. **Check against constitution rules**
4. **Flag any scope creep**
5. **Verify checklist items**

Provide feedback:
```
## PR Review

### ✅ Looks Good
- [What's correct]

### ⚠️ Suggestions
- [Improvements to consider]

### ❌ Issues to Fix
- [Things that should be changed before merge]

### Scope Check
- Matches issue: ✅/❌
- Scope creep detected: Yes/No
```
