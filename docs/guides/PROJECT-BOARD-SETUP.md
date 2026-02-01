# GitHub Project Board Configuration Guide

## Overview

This guide explains how to set up and configure GitHub Projects for managing ideas and innovations using the Devviaai Innovation Workflow.

## 🎯 Purpose

The Project Board provides a visual Kanban-style workflow to track ideas from submission through implementation and release. It integrates seamlessly with Issues, Pull Requests, and Milestones.

## 🚀 Quick Setup

### Step 1: Create New Project

1. Navigate to your repository on GitHub
2. Click the **Projects** tab
3. Click **New project**
4. Select **Board** template
5. Name it: "Innovation & Ideas Management"
6. Click **Create project**

### Step 2: Configure Board Columns

Delete default columns and create these:

1. **📥 Idea Pool**
2. **🔍 Under Review**
3. **✅ Approved**
4. **🚧 In Progress**
5. **✔️ Implemented**
6. **📦 Released**

### Step 3: Add Custom Fields

Click **+** next to Fields to add:

1. **Priority**
   - Type: Single select
   - Options: Critical, High, Medium, Low
   
2. **Effort**
   - Type: Single select
   - Options: Small, Medium, Large, Extra Large
   
3. **Framework**
   - Type: Single select
   - Options: Design Thinking, Lean Startup, Agile
   
4. **Target Release**
   - Type: Text
   - Format: v1.x.0 or Q1 2026
   
5. **Impact Score**
   - Type: Number
   - Range: 1-10

### Step 4: Configure Automation

Set up automated workflows:

1. Click **⋯** menu on each column
2. Select **Manage automation**
3. Configure as follows:

**📥 Idea Pool**:
- Auto-add items: Issues labeled with `idea` and `new`

**🔍 Under Review**:
- Auto-add items: Issues labeled with `under review`
- Auto-remove from Idea Pool

**✅ Approved**:
- Auto-add items: Issues labeled with `approved`
- Auto-remove from Under Review

**🚧 In Progress**:
- Auto-add items: Pull requests linked to idea issues
- Auto-add items: Issues labeled with `in-progress`

**✔️ Implemented**:
- Auto-add items: Pull requests merged
- Auto-add items: Issues labeled with `implemented`

**📦 Released**:
- Auto-add items: Issues closed with label `released`

## 📊 Detailed Configuration

### Column Configuration Details

#### 📥 Idea Pool

**Purpose**: New ideas awaiting review

**Automation**:
```yaml
When: Issue is created
If: Has label "idea" AND "new"
Then: Add to this column
```

**Column Limit**: None (collect all new ideas)

**WIP Limit**: Not applicable

---

#### 🔍 Under Review

**Purpose**: Ideas being actively evaluated

**Automation**:
```yaml
When: Issue is labeled
If: Label added is "under review"
Then: Move to this column

When: Issue is labeled
If: Label added is "approved" OR "not feasible"
Then: Remove from this column
```

**Column Limit**: Recommended 10 (to ensure timely reviews)

**WIP Limit**: 10

**Review SLA**: 1-2 weeks

---

#### ✅ Approved

**Purpose**: Validated ideas ready for prioritization

**Automation**:
```yaml
When: Issue is labeled
If: Label added is "approved"
Then: Add to this column

When: Issue is labeled
If: Label added is "in-progress"
Then: Remove from this column
```

**Column Limit**: None

**Actions Required**:
- Assign to milestone
- Set priority and effort
- Assign owner (optional)

---

#### 🚧 In Progress

**Purpose**: Ideas being actively implemented

**Automation**:
```yaml
When: Pull request is opened
If: PR links to issue with "approved" label
Then: Add to this column

When: Issue is labeled
If: Label added is "in-progress"
Then: Add to this column

When: Pull request is merged
Then: Remove from this column
```

**Column Limit**: Recommended 5-10 (based on team capacity)

**WIP Limit**: 10

---

#### ✔️ Implemented

**Purpose**: Code complete, awaiting release

**Automation**:
```yaml
When: Pull request is merged
Then: Add to this column

When: Issue is labeled
If: Label added is "implemented"
Then: Add to this column

When: Issue is labeled
If: Label added is "released"
Then: Remove from this column
```

**Column Limit**: None

**Actions Required**:
- Verify in staging
- Prepare release notes
- Plan release

---

#### 📦 Released

**Purpose**: Ideas released to production

**Automation**:
```yaml
When: Issue is closed
If: Has label "released"
Then: Add to this column
```

**Column Limit**: None

**Actions Required**:
- Measure impact
- Collect feedback
- Complete impact measurement template

---

### Custom Field Details

#### Priority Field

**Purpose**: Indicate urgency and importance

**Values**:
- **Critical**: Blocking work, must be addressed immediately
- **High**: Significantly improves workflow, high value
- **Medium**: Nice to have improvement, moderate value
- **Low**: Future consideration, low immediate value

**Color Coding**:
- Critical: Red
- High: Orange
- Medium: Yellow
- Low: Green

**Assignment**: Set during "Under Review" or "Approved" stage

---

#### Effort Field

**Purpose**: Estimate implementation complexity

**Values**:
- **Small**: < 1 week, low complexity
- **Medium**: 1-4 weeks, moderate complexity
- **Large**: 1-3 months, high complexity
- **Extra Large**: > 3 months, very high complexity

**Guidance**:
- Small: Bug fixes, minor enhancements, documentation
- Medium: New features, moderate refactoring
- Large: Major features, significant architecture changes
- Extra Large: Consider breaking down into smaller ideas

**Assignment**: Set during "Under Review" stage

---

#### Framework Field

**Purpose**: Track which innovation framework was used

**Values**:
- **Design Thinking**: User-centered approach
- **Lean Startup**: Hypothesis-driven experimentation
- **Agile**: User story-based features

**Color Coding**:
- Design Thinking: Purple
- Lean Startup: Orange
- Agile: Blue

**Assignment**: Auto-set based on issue template used

---

#### Target Release Field

**Purpose**: Track planned release version or date

**Format**: 
- Version: `v1.2.0`, `v2.0.0`
- Quarter: `Q1 2026`, `Q2 2026`
- Date: `2026-03-31`

**Assignment**: Set when moved to "Approved" and assigned to milestone

---

#### Impact Score Field

**Purpose**: Quantify expected or actual impact

**Scale**: 1-10
- 1-3: Low impact (minor improvement)
- 4-6: Medium impact (notable improvement)
- 7-9: High impact (significant improvement)
- 10: Exceptional impact (transformative)

**Calculation**: Based on:
- User value (40%)
- Technical feasibility (20%)
- Resource efficiency (20%)
- Strategic alignment (20%)

**Assignment**: 
- Initial: During evaluation (expected impact)
- Final: Post-release (actual impact)

---

## 🔄 Workflow Examples

### Example 1: Design Thinking Idea Flow

```
1. User submits idea using Design Thinking template
   → Auto-added to "📥 Idea Pool"
   → Framework field set to "Design Thinking"

2. Team member adds "under review" label
   → Auto-moved to "🔍 Under Review"
   → Evaluation begins

3. After review, team adds "approved" label
   → Auto-moved to "✅ Approved"
   → Priority set to "High"
   → Effort set to "Medium"
   → Target Release set to "v1.2.0"
   → Assigned to Q1 2026 milestone

4. Developer picks up and creates PR
   → Auto-moved to "🚧 In Progress"
   → "in-progress" label added

5. PR is merged
   → Auto-moved to "✔️ Implemented"
   → "implemented" label added

6. Feature is released to production
   → Auto-moved to "📦 Released"
   → "released" label added
   → Issue closed
   → Impact measured
```

### Example 2: Lean Startup Hypothesis Flow

```
1. Hypothesis submitted via Lean Startup template
   → "📥 Idea Pool"
   → Framework: "Lean Startup"

2. Team reviews hypothesis and metrics
   → "🔍 Under Review"
   → Metrics validated

3. MVP approved for experimentation
   → "✅ Approved"
   → Priority: "High"
   → Effort: "Small" (MVP only)
   → Target: "Q1 2026 - Week 1"

4. MVP built and tested
   → "🚧 In Progress"
   → Rapid iteration

5. MVP complete, measuring results
   → "✔️ Implemented"
   → 4-week measurement period

6. Results analyzed, decision made
   → If successful: "📦 Released" & expand
   → If failed: Close with learnings documented
```

### Example 3: Agile Feature Flow

```
1. Feature request via Agile template
   → "📥 Idea Pool"
   → Framework: "Agile"

2. Product owner reviews user story
   → "🔍 Under Review"
   → Acceptance criteria validated

3. Added to sprint backlog
   → "✅ Approved"
   → Priority: "Must Have"
   → Story Points: 5
   → Target: "Sprint 12"

4. Sprint work begins
   → "🚧 In Progress"
   → Daily standups track progress

5. Story complete, definition of done met
   → "✔️ Implemented"
   → Demo in sprint review

6. Deployed to production
   → "📦 Released"
   → Retrospective learnings captured
```

---

## 📈 Views & Filters

### Recommended Views

#### View 1: All Ideas (Default Board View)

**Layout**: Board  
**Group by**: Status (columns)  
**Sort by**: Priority (descending), then Created date  
**Filter**: None (show all)

---

#### View 2: This Sprint

**Layout**: Table  
**Filter**: 
- Status: "In Progress" OR "Implemented"
- Target Release: Current sprint/milestone

**Columns to Show**:
- Title
- Priority
- Effort
- Assignee
- Status
- Framework

**Sort by**: Priority (descending)

---

#### View 3: Backlog

**Layout**: Table  
**Filter**: 
- Status: "Approved"
- Label: "approved"

**Columns to Show**:
- Title
- Priority
- Effort
- Target Release
- Impact Score
- Framework

**Sort by**: Priority (descending), Impact Score (descending)

---

#### View 4: Needs Review

**Layout**: Board  
**Filter**: 
- Status: "Under Review"
- Label: "under review"

**Group by**: Framework  
**Sort by**: Created date (oldest first)

---

#### View 5: Released This Quarter

**Layout**: Table  
**Filter**: 
- Status: "Released"
- Closed: Last 3 months

**Columns to Show**:
- Title
- Framework
- Impact Score (Actual)
- Release Date
- Closed date

**Sort by**: Closed date (descending)

---

#### View 6: By Framework

**Layout**: Board  
**Group by**: Framework  
**Filter**: Status not "Released"  
**Sort by**: Priority, Created date

---

## 🎛️ Advanced Features

### Insights & Reports

Enable **Insights** to track:

1. **Velocity**: Ideas completed per sprint/month
2. **Cycle Time**: Time from submission to release
3. **Approval Rate**: % of ideas approved vs. rejected
4. **Framework Distribution**: Ideas by framework type
5. **Cumulative Flow**: Visual representation of idea flow

### Integration with Milestones

**Link ideas to milestones**:

1. Create milestones for releases (v1.1.0, v1.2.0, Q1 2026)
2. Assign approved ideas to milestones
3. Track milestone progress via Project
4. Use milestone as filter in views

### Labels Integration

**Required Labels** (auto-applied by workflow):
- `idea` - All ideas
- `new` - Newly submitted
- `under review` - Being evaluated
- `approved` - Validated
- `in-progress` - Being implemented
- `implemented` - Code complete
- `released` - In production

**Framework Labels** (from templates):
- `design-thinking`
- `lean-startup`
- `agile`

**Stage Labels** (optional):
- `stage-1`, `stage-2`, `stage-3`, `stage-4`

### Automated Reports

Use GitHub Actions to generate weekly reports:

```yaml
# .github/workflows/idea-metrics-report.yml
name: Weekly Ideas Report

on:
  schedule:
    - cron: '0 9 * * MON'  # Every Monday at 9 AM

jobs:
  generate-report:
    runs-on: ubuntu-latest
    steps:
      - name: Generate metrics
        uses: actions/github-script@v7
        with:
          script: |
            # Count ideas by status
            # Calculate metrics
            # Post to discussion or issue
```

---

## 👥 Team Workflows

### For Idea Submitters

1. Submit idea via issue template
2. Watch issue for feedback
3. Respond to questions during review
4. Celebrate when approved!
5. Track progress via project board

### For Reviewers

1. Check "Needs Review" view weekly
2. Evaluate using idea evaluation template
3. Add labels to move through workflow
4. Comment with decision rationale
5. Assign milestones for approved ideas

### For Implementers

1. Check "Backlog" view for approved ideas
2. Claim idea by commenting and adding "in-progress" label
3. Create linked PR
4. Track in "In Progress" view
5. Merge and move to "Implemented"

### For Product Owners

1. Review "Approved" ideas for prioritization
2. Assign to milestones
3. Set priority and effort
4. Monitor "This Sprint" view
5. Validate completed features
6. Move to "Released" and measure impact

---

## 🔧 Maintenance

### Regular Cleanup

**Weekly**:
- Review "Under Review" - chase stale reviews
- Update "In Progress" - check for blockers
- Verify "Implemented" - confirm release plans

**Monthly**:
- Archive old "Released" ideas
- Review and adjust priorities
- Update metrics and reports
- Refine automation rules

**Quarterly**:
- Analyze trends and patterns
- Update process based on learnings
- Train new team members
- Celebrate successes

### Best Practices

1. **Keep columns moving**: Don't let ideas stagnate
2. **Respect WIP limits**: Don't overload in-progress work
3. **Update regularly**: Keep status and fields current
4. **Communicate**: Use comments for updates
5. **Measure**: Track metrics and improve

---

## 📚 Resources

- [GitHub Projects Documentation](https://docs.github.com/en/issues/planning-and-tracking-with-projects)
- [Innovation Workflow Guide](INNOVATION-WORKFLOW.md)
- [Issue Templates](../../.github/ISSUE_TEMPLATE/)
- [GitHub Actions Workflow](../../.github/workflows/idea-management.yml)

---

## 🆘 Troubleshooting

### Ideas Not Auto-Adding to Board

**Problem**: New ideas don't appear in "Idea Pool"

**Solutions**:
1. Check automation rules are enabled
2. Verify issue has correct labels (`idea`, `new`)
3. Manually add to project if automation failed
4. Check project settings → Workflows

### Columns Not Moving Automatically

**Problem**: Items don't move when labels change

**Solutions**:
1. Verify automation rules for each column
2. Check that labels are spelled correctly
3. Ensure workflows are enabled in project settings
4. Manually move items if needed

### Custom Fields Not Showing

**Problem**: Fields added but not visible

**Solutions**:
1. Edit view settings to show field
2. Add field to specific view layouts
3. Refresh page
4. Check field permissions

---

**Version**: 1.0.0  
**Last Updated**: 2026-02-01  
**Maintained By**: Devviaai Framework Team

---

[⬅️ Back to Innovation Workflow](INNOVATION-WORKFLOW.md) | [View Example Board](https://github.com/omidta/Devviaai/projects)
