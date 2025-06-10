# Hotfix Pull Request Template

**Target Audience**: Senior Developers, DevOps  
**Last Updated**: 2025-06-10 by System  
**Template Type**: Emergency Hotfix

## 🚨 Emergency Hotfix Details
**Incident ID:** 
**Severity:** [ ] Critical [ ] High
**Started:** [Date/Time]
**Affected Systems:** 

**Problem Description:**
<!-- Clear, concise description of the critical issue -->

**Business Impact:**
- Users affected: 
- Revenue impact: 
- System downtime: 
- Customer complaints: 

## Root Cause Analysis
**Immediate Cause:**
<!-- What directly caused the failure -->

**Technical Details:**
<!-- Technical explanation of the failure -->

**Contributing Factors:**
- Factor 1
- Factor 2

## Hotfix Implementation
**Technology Stack:** [ ] Spring Boot [ ] ASP.NET Core [ ] NextJS [ ] Flutter

**Fix Strategy:** [ ] Rollback [ ] Forward Fix [ ] Configuration Change

**Changes Made:**
<!-- Minimal, targeted changes to resolve the issue -->

**Files Modified:**
- `file1.ext` - Description of change
- `file2.ext` - Description of change

## Risk Assessment
**Fix Risk Level:** [ ] Low [ ] Medium [ ] High

**Potential Impact:**
- [ ] No side effects expected
- [ ] Possible impact on: ______
- [ ] Requires monitoring: ______

**Rollback Strategy:**
<!-- How to quickly revert this hotfix -->

## Testing (Minimal but Critical)
**Testing Strategy:** [ ] Unit [ ] Integration [ ] Manual [ ] Smoke Test

- [ ] Critical path tested
- [ ] Hotfix validates the fix
- [ ] No regressions in core functionality
- [ ] Performance acceptable

**Test Results:**
```
Issue reproduction: ✅ Confirmed
Fix validation: ✅ Resolved
Smoke tests: ✅ Passed
Performance: ✅ Acceptable
```

## Deployment Plan
**Deployment Window:** [Immediate/Scheduled]
**Deployment Method:** [ ] Blue-Green [ ] Rolling [ ] Immediate Replace

**Pre-deployment:**
- [ ] Backup current state
- [ ] Notify stakeholders
- [ ] Prepare rollback plan

**Post-deployment:**
- [ ] Verify fix working
- [ ] Monitor error rates
- [ ] Confirm user reports resolved

## Communication
**Stakeholders Notified:**
- [ ] Product team
- [ ] Customer support
- [ ] Management
- [ ] Development team

**Communication Channels:**
- [ ] Slack incident channel
- [ ] Email notifications
- [ ] Status page update
- [ ] Customer communication

## Follow-up Actions
**Immediate (Next 24 hours):**
- [ ] Monitor system stability
- [ ] Track error rates
- [ ] Gather user feedback

**Short-term (Next week):**
- [ ] Comprehensive fix implementation
- [ ] Additional testing
- [ ] Process improvement

**Long-term:**
- [ ] Root cause prevention
- [ ] Process improvements
- [ ] Documentation updates

## Incident Timeline
| Time | Action | Result |
|------|--------|--------|
| [Time] | Issue detected | |
| [Time] | Investigation started | |
| [Time] | Root cause identified | |
| [Time] | Hotfix developed | |
| [Time] | Hotfix deployed | |
| [Time] | Issue resolved | |

---
**Incident Commander:** @  
**Developer:** @  
**DevOps:** @  
**Product Owner:** @  
**Approval Required:** [ ] Yes [ ] No - Emergency Authority
