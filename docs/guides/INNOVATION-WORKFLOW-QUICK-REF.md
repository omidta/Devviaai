# Innovation Workflow - Quick Reference Guide

## 📋 At a Glance

The Innovation Workflow helps you submit, evaluate, and implement ideas using proven frameworks.

## 🚀 Submit an Idea

### Choose Your Framework

| Framework | Best For | Template |
|-----------|----------|----------|
| **Design Thinking** | User-centered innovations, understanding needs | [Submit Idea](../../.github/ISSUE_TEMPLATE/01-idea-design-thinking.yml) |
| **Lean Startup** | Testing hypotheses, data-driven experiments | [Submit Idea](../../.github/ISSUE_TEMPLATE/02-idea-lean-startup.yml) |
| **Agile** | Clear feature requests with acceptance criteria | [Submit Idea](../../.github/ISSUE_TEMPLATE/03-idea-agile-feature.yml) |

### Quick Start

1. Go to [Issues → New Issue](https://github.com/omidta/Devviaai/issues/new/choose)
2. Select appropriate idea template
3. Fill in all required fields
4. Submit - automation handles the rest!

## 📊 Idea Lifecycle

```
┌─────────────┐
│  Idea Pool  │  ← Newly submitted ideas
└──────┬──────┘
       │
       ▼
┌─────────────┐
│Under Review │  ← Team evaluation (1-2 weeks)
└──────┬──────┘
       │
       ├─────────────┐
       ▼             ▼
┌──────────┐   ┌─────────────┐
│ Approved │   │Not Feasible │  ← Closed with explanation
└────┬─────┘   └─────────────┘
     │
     ▼
┌─────────────┐
│In Progress  │  ← Implementation started
└──────┬──────┘
       │
       ▼
┌─────────────┐
│Implemented  │  ← Code complete
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  Released   │  ← In production, impact measured
└─────────────┘
```

## 🏷️ Labels & Meaning

### Status Labels
- `idea` - All ideas
- `new` - Newly submitted
- `under review` - Being evaluated
- `approved` - Validated for implementation
- `not feasible` - Not moving forward
- `in-progress` - Being implemented
- `implemented` - Code complete
- `released` - In production

### Framework Labels
- `design-thinking` - Design Thinking approach
- `lean-startup` - Lean Startup approach
- `agile` - Agile approach

### Priority Labels
- `Critical` - Blocking current work
- `High` - Significantly improves workflow
- `Medium` - Nice to have
- `Low` - Future consideration

## 📝 Framework Comparison

### Design Thinking (5 Stages)
1. **Empathize**: Understand user needs
2. **Define**: Frame the problem
3. **Ideate**: Generate solutions
4. **Prototype**: Build to test
5. **Test**: Validate with users

**Best for**: Complex user problems, multiple solution approaches

---

### Lean Startup (Build-Measure-Learn)
1. **Hypothesis**: What are we testing?
2. **Build**: Create minimum viable product (MVP)
3. **Measure**: Collect data and metrics
4. **Learn**: Analyze and decide (persevere/pivot)

**Best for**: Uncertain value, need data-driven validation

---

### Agile (User Stories)
1. **User Story**: As [role], I want [action], so that [benefit]
2. **Acceptance Criteria**: Specific, testable requirements
3. **Definition of Done**: Complete checklist

**Best for**: Clear requirements, known implementation path

## 🎯 Key Documents

### For Submitters
- [Innovation Workflow Guide](INNOVATION-WORKFLOW.md) - Complete documentation
- [Issue Templates](../../.github/ISSUE_TEMPLATE/) - Submission forms

### For Reviewers
- [Idea Evaluation Template](../templates/idea-evaluation-template.md) - Systematic evaluation
- [Project Board Setup](PROJECT-BOARD-SETUP.md) - Track ideas

### For Implementers
- [Prototype Planning Template](../templates/prototype-planning-template.md) - Plan implementation
- [PR Template](../../.github/PULL_REQUEST_TEMPLATE.md) - Submit code

### For Post-Release
- [Impact Measurement Template](../templates/impact-measurement-template.md) - Measure results

## 💡 Tips for Success

### Submitting Ideas
✅ **DO**:
- Search for duplicates first
- Be specific and concrete
- Include data and examples
- Quantify expected impact
- Choose the right framework

❌ **DON'T**:
- Submit vague ideas
- Assume everyone has context
- Skip required fields
- Ignore feedback/questions

### During Review
✅ **DO**:
- Respond promptly to questions
- Provide additional context
- Be open to feedback
- Engage constructively

❌ **DON'T**:
- Get defensive
- Disappear after submission
- Argue without data
- Take rejection personally

### Implementation
✅ **DO**:
- Follow the PR template
- Link to the idea issue
- Include tests
- Update documentation
- Track metrics (for Lean Startup)

❌ **DON'T**:
- Skip acceptance criteria
- Forget to measure impact
- Ignore code review feedback
- Leave work incomplete

## 📞 Getting Help

### Where to Ask

| Question Type | Where to Ask |
|---------------|--------------|
| Process questions | [Discussions - Q&A](https://github.com/omidta/Devviaai/discussions/categories/q-a) |
| Idea feedback | [Discussions - Feedback](https://github.com/omidta/Devviaai/discussions/categories/feedback-requests) |
| Technical issues | [Issues](https://github.com/omidta/Devviaai/issues) |
| General discussion | [Discussions - General](https://github.com/omidta/Devviaai/discussions/categories/general) |

## 📈 Metrics to Track

### For Lean Startup Ideas
- Primary hypothesis metric
- Secondary support metrics
- User adoption rate
- Time to value
- Cost vs. benefit

### For Design Thinking Ideas
- User satisfaction (NPS)
- Task completion rate
- Time on task
- Error rate reduction
- User feedback themes

### For Agile Features
- Acceptance criteria met
- Definition of done complete
- User story points delivered
- Sprint velocity impact
- Defect rate

## 🔧 Common Workflows

### Submit → Review → Approve → Implement
```bash
# 1. Submit idea via template
# 2. Team adds "under review" label
# 3. After evaluation, team adds "approved" label
# 4. Team assigns milestone and priority
# 5. Developer picks up, adds "in-progress"
# 6. PR created and linked
# 7. PR merged, adds "implemented"
# 8. Feature released, adds "released"
# 9. Impact measured and documented
```

### MVP Testing (Lean Startup)
```bash
# 1. Submit hypothesis via Lean Startup template
# 2. Approved for MVP testing
# 3. Build minimal version (1-2 weeks)
# 4. Deploy and measure (4 weeks)
# 5. Analyze data
# 6. Decision: Persevere, Pivot, or Sunset
```

## 📚 Related Resources

- [Full Innovation Workflow Guide](INNOVATION-WORKFLOW.md)
- [Project Board Configuration](PROJECT-BOARD-SETUP.md)
- [Main Framework Documentation](../README.md)
- [4-Stage Workflow](../../README.md)

## 🎉 Examples

### Example 1: TBD Validation Tool (Design Thinking)
- **Problem**: Manual TBD validation is slow and error-prone
- **Solution**: Automated CLI validation tool
- **Impact**: 30 min → 30 sec validation time
- **Status**: Implemented and released

### Example 2: AI Architecture Assistant (Lean Startup)
- **Hypothesis**: AI recommendations improve TBD quality by 30%
- **MVP**: Basic OpenAI integration with 5 patterns
- **Metrics**: Quality score, time savings, accuracy
- **Decision**: Validated - expand to more patterns

### Example 3: GitHub Actions Deployment (Agile)
- **User Story**: As a Platform Engineer, I want automated deployment via GitHub Actions
- **Acceptance Criteria**: Deploy to dev/staging/prod with approval gates
- **Result**: 2-hour manual → 10-min automated deployment

---

**Quick Links**:
- [Submit New Idea](https://github.com/omidta/Devviaai/issues/new/choose)
- [View All Ideas](https://github.com/omidta/Devviaai/issues?q=is%3Aissue+label%3Aidea)
- [Project Board](https://github.com/omidta/Devviaai/projects)
- [Discussions](https://github.com/omidta/Devviaai/discussions)

---

**Version**: 1.0.0  
**Last Updated**: 2026-02-01

[⬅️ Back to Innovation Workflow](INNOVATION-WORKFLOW.md) | [📖 Full Documentation](../README.md)
