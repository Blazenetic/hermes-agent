# Long-Term TODO for Hermes Agent

This document tracks future work, improvements, and pending tasks that should be addressed in future iterations.

---

## 🔄 Pending Pull Request

### skills-discovery-improvements (Branch)
- **Status**: Pushed to fork, awaiting PR creation
- **Branch**: `skills-discovery-improvements`
- **Fork**: `Blazenetic/hermes-agent`
- **Upstream**: `nousresearch/hermes-agent`
- **Changes**:
  - Enhanced `skills_list` with `search` and `tags` parameters
  - Added `tags` and `related_skills` to skills_list output
  - Added `memory_search` tool to prevent duplicate memory entries
  - Updated memory toolset to include new tool

**Action Required**:
1. Create PR: https://github.com/Blazenetic/hermes-agent/pull/new/skills-discovery-improvements
2. Add upstream as remote: `git remote add upstream https://github.com/nousresearch/hermes-agent.git`
3. Rebase onto upstream main: `git rebase upstream/main`
4. Open PR against nousresearch/hermes-agent

---

## 📋 Skills System Improvements

### 1. Add `when_to_use` Field to Skills

**Description**: Add a `when_to_use` field to skill frontmatter to provide explicit guidance on when to load a skill.

**Implementation**:
- Add `when_to_use` field to SKILL.md frontmatter template
- Update existing skills with relevant `when_to_use` entries
- Optionally expose `when_to_use` in `skill_view` output

**Example Frontmatter**:
```yaml
---
name: github-pr-workflow
description: Full pull request lifecycle management
when_to_use:
  - Opening a new PR
  - Checking CI status
  - Merging a branch
  - Rebasing PR
---
```

**Priority**: Medium
**Effort**: Medium (requires updating multiple skill files)

---

### 2. Skill Tag Standardization

**Description**: Many skills lack proper tags. Standardize and add tags to skills that are missing them.

**Current Status**:
- Some skills have `metadata.hermes.tags`
- Some skills have top-level `tags`
- Some skills have no tags at all

**Action Items**:
- Audit all skills for missing tags
- Create a tagging convention/guidelines document
- Add tags to skills that are missing them
- Document tags in skill creation guidelines

**Priority**: Low
**Effort**: Medium

---

### 3. Enhanced Skill Discovery UI

**Description**: Consider adding a dashboard or CLI command for visual skill discovery.

**Features**:
- Visual skill browser with tag filtering
- Skill comparison view
- Usage statistics

**Priority**: Low
**Effort**: High

---

## 🧠 Memory System Improvements

### 1. Memory Usage Analytics

**Description**: Track and display memory usage patterns.

**Features**:
- Show most frequently added topics
- Display memory usage over time
- Suggest cleanup of old entries

**Priority**: Low
**Effort**: Medium

---

### 2. Memory Export/Import

**Description**: Allow exporting and importing memory entries.

**Use Cases**:
- Backup memory to file
- Share memory between different Hermes instances
- Migrate memory to new system

**Priority**: Low
**Effort**: Low

---

## 🔧 Tool System Improvements

### 1. Tool Usage Analytics

**Description**: Track which tools are used most frequently.

**Use Cases**:
- Identify useful vs unused tools
- Improve tool documentation
- Guide tool development priorities

**Priority**: Low
**Effort**: Medium

---

### 2. Enhanced Error Messages

**Description**: Improve error messages across all tools.

**Current Issue**: Some tool errors are vague or don't provide actionable guidance.

**Action Items**:
- Audit tool error messages
- Add context to error responses
- Suggest corrective actions

**Priority**: Medium
**Effort**: Medium

---

## 📊 Dashboard & Monitoring

### 1. Cron Job Dashboard Enhancements

**Description**: Continue improving the cron dashboard to expose more backend features.

**Related**: Previous work on dashboard-investigation-and-enhancement skill

**Action Items**:
- Expose skill loading in job configuration
- Add model/provider selection to UI
- Show job history and success rates

**Priority**: Medium
**Effort**: Medium

---

### 2. Session Analytics

**Description**: Add analytics for conversation sessions.

**Features**:
- Tokens used per session
- Tools used per session
- Session duration statistics

**Priority**: Low
**Effort**: Medium

---

## 🚀 Agent Performance

### 1. Context Compression Improvements

**Description**: Enhance the context compressor for better token efficiency.

**Areas**:
- Better summarization of tool outputs
- Intelligent compression of repeated patterns
- Preserve critical context longer

**Priority**: Medium
**Effort**: High

---

### 2. Prompt Caching Optimization

**Description**: Improve prompt caching for better performance.

**Areas**:
- Cache hit rate optimization
- Cache invalidation strategy
- Memory management for cached prompts

**Priority**: Medium
**Effort**: Medium

---

## 📝 Documentation

### 1. API Documentation

**Description**: Generate API docs from code.

**Tools to Document**:
- All tool schemas
- Internal APIs
- Plugin system

**Priority**: Medium
**Effort**: Medium

---

### 2. Migration Guides

**Description**: Create guides for major version upgrades.

**Current Needs**:
- v0.10.x to v0.11.x migration
- Config file format changes

**Priority**: Low
**Effort**: Low

---

## 🧪 Testing

### 1. Increase Test Coverage

**Description**: Add more tests for core functionality.

**Priority**: Medium
**Effort**: High

---

### 2. Integration Tests

**Description**: Add end-to-end integration tests.

**Priority**: Low
**Effort**: High

---

## 📅 Prioritized Backlog

| Priority | Task | Effort |
|----------|------|--------|
| High | Create and merge skills-discovery PR | Low |
| Medium | Add `when_to_use` field to skills | Medium |
| Medium | Context compression improvements | High |
| Medium | Tool error message improvements | Medium |
| Medium | Cron dashboard enhancements | Medium |
| Low | Memory export/import | Low |
| Low | Skill tag standardization | Medium |
| Low | Session analytics | Medium |
| Low | Tool usage analytics | Medium |

---

*Last Updated: 2026-04-19*
*Document Owner: Hermes Agent Development*