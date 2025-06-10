# Version Control & Git Practices

## Current State

### Pain Points (Team Feedback)
- Inconsistent commit messages across teams
- Merge conflicts from long-running branches  
- PR reviews taking 2+ days
- Unclear commit history

### What's Working
- Feature branch workflow
- Required code reviews
- CI/CD integration

## Standards

### Branch Naming
```
feature/TICKET-123-short-description
bugfix/TICKET-456-fix-description  
hotfix/TICKET-789-critical-issue
release/v1.2.0
```

### Commit Messages
```
<type>(<scope>): <description>

<body>

<footer>
```

**Types**: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

**Examples**:
```
feat(auth): add JWT token validation
fix(payment): resolve timeout issue  
docs(api): update authentication guide
```

### Pull Request Process
1. Create PR using [template](../templates/pr-template.md)
2. Minimum 2 reviewers required
3. All CI checks must pass
4. Squash merge to main

## Implementation

### Week 1: Setup
- [ ] Configure branch protection rules
- [ ] Set up commit message templates
- [ ] Create PR templates per stack

### Week 2: Rollout  
- [ ] Train team on new standards
- [ ] Enable automated checks
- [ ] Monitor compliance

### Week 3: Optimization
- [ ] Gather feedback
- [ ] Refine processes
- [ ] Document lessons learned

## Tools

### Git Hooks
```bash
# Pre-commit: lint and test
#!/bin/sh
npm run lint && npm run test:unit
```

### Commit Template
```bash
git config --global commit.template ~/.gitmessage
```

## Metrics

| Metric | Current | Target |
|--------|---------|---------|
| Commit Message Compliance | 40% | 95% |
| PR Review Time | 2.3 days | <1 day |
| Merge Conflicts | 15% | <5% |

---

**Team Input Section**

### @parseen254 (2025-06-10)
**Pain Points:**
- Need commit message automation
- PR templates too generic

**Suggestions:**
- Use Conventional Commits spec
- Stack-specific PR templates

### @[team-member] (date)
*Add your feedback here*