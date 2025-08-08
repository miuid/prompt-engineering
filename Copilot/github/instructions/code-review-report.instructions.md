# Code Review Report Instructions
## Phase 1: Comprehensive Review Report

### 1.1 Executive Summary
```markdown
## Pull Request Review Summary

**PR Title**: [Pull Request Title]
**Author**: [Author Name]
**Scope**: [Brief description of changes]
**Overall Assessment**: [Approve/Request Changes/Needs Discussion]

### Key Findings
- **Critical Issues**: [Number] issues that must be addressed
- **Major Concerns**: [Number] issues that should be addressed
- **Minor Suggestions**: [Number] improvements that could be made
- **Positive Highlights**: [Notable good practices or implementations]
```

### 1.2 Detailed Findings by Category

**Critical Issues (Must Fix):**
```markdown
### 🚨 Critical Issues

#### Issue 1: [Title]
**File**: `path/to/file.js:123`
**Severity**: Critical
**Category**: [Security/Correctness/Breaking Change]

**Problem**:
[Detailed explanation of the issue]

**Impact**:
[How this affects the application/users]

**Solution**:
[Specific steps to resolve]

**Code Example**:
```javascript
// ❌ Current (problematic)
[current code]

// ✅ Suggested (corrected)
[suggested code]
```

**Major Concerns (Should Fix):**
```markdown
### ⚠️ Major Concerns

#### Concern 1: [Title]
**File**: `path/to/file.js:456`
**Severity**: Major
**Category**: [Maintainability/Performance/Testing]

**Issue**:
[Description of the concern]

**Recommendation**:
[Suggested improvement]

**Rationale**:
[Why this improvement matters]
```

**Minor Suggestions (Could Improve):**
```markdown
### 💡 Minor Suggestions

#### Suggestion 1: [Title]
**File**: `path/to/file.js:789`
**Category**: [Code Style/Documentation/Optimization]

**Current**:
[Current implementation]

**Suggestion**:
[Proposed improvement]

**Benefit**:
[Why this would be better]
```

### 1.3 Pattern Analysis Results

**Consistency Assessment:**
```markdown
### 📊 Pattern Analysis

#### Existing Pattern Compliance
- **✅ Follows Established Patterns**: [List areas where code follows existing patterns]
- **⚠️ Pattern Deviations**: [List areas where code deviates from established patterns]
- **🔍 New Patterns Introduced**: [List any new patterns this PR introduces]

#### Recommendations
- **Adopt Existing Patterns**: [Specific recommendations to align with existing code]
- **Pattern Documentation**: [Suggest documenting new patterns if they're beneficial]
- **Consistency Improvements**: [Ways to improve overall consistency]
```

### 1.4 Breaking Change Assessment

**Impact Analysis:**
```markdown
### 💥 Breaking Change Analysis

#### Identified Breaking Changes
1. **[Change Description]**
   - **Affected Components**: [List of affected areas]
   - **Migration Required**: [Yes/No and details]
   - **Backwards Compatibility**: [Assessment]

#### Mitigation Strategies
- **Immediate Actions**: [What needs to be done before merge]
- **Communication**: [Who needs to be notified]
- **Documentation**: [What documentation needs updating]
- **Testing**: [Additional testing required]
```

### 1.5 Testing Assessment

**Test Coverage Analysis:**
```markdown
### 🧪 Testing Assessment

#### Test Coverage
- **Unit Tests**: [Adequate/Needs Improvement/Missing]
- **Integration Tests**: [Adequate/Needs Improvement/Missing]
- **Edge Cases**: [Covered/Partially Covered/Not Covered]

#### Testing Recommendations
- **Required Tests**: [List essential tests that must be added]
- **Suggested Tests**: [List tests that would improve confidence]
- **Test Quality**: [Assessment of existing test quality]

#### Test Examples Needed
```javascript
// Example of missing test case
describe('UserValidator', () => {
  it('should handle edge case when email is null', () => {
    // This test is missing but should be added
  });
});
```

## Phase 2: Constructive Feedback & Mentoring

### 2.1 Educational Comments

**Teaching Moments:**
```markdown
### 🎓 Learning Opportunities

#### Best Practice Explanation
**Topic**: [Specific programming concept]
**Context**: [Where this applies in the PR]

**Explanation**:
[Detailed explanation of the best practice and why it matters]

**Example**:
[Code example demonstrating the concept]

**Further Reading**:
[Links to relevant documentation or articles]
```

### 2.2 Positive Reinforcement

**Good Practices Recognition:**
```markdown
### 👏 Well Done

#### Excellent Implementation
**What**: [Specific good practice observed]
**Why it's Good**: [Explanation of why this is a good approach]
**Impact**: [How this benefits the codebase]

This is a great example of [specific principle/pattern] that other team members can learn from.
```

### 2.3 Future Considerations

**Architectural Guidance:**
```markdown
### 🔮 Future Considerations

#### Scalability
- [Considerations for how this code will scale]
- [Potential future modifications needed]

#### Maintenance
- [How to make this code easier to maintain]
- [Documentation or tooling that would help]

#### Evolution
- [How this fits into the broader system architecture]
- [Potential integration points for future features]
```


## Review Completion Checklist

Before finalizing the review, ensure:

- [ ] **All files analyzed** for correctness and quality
- [ ] **Breaking changes identified** and impact assessed
- [ ] **Pattern consistency checked** against existing codebase
- [ ] **Security implications evaluated**
- [ ] **Performance impact considered**
- [ ] **Test coverage assessed** and gaps identified
- [ ] **Documentation requirements** evaluated
- [ ] **Clarifying questions asked** if needed
- [ ] **Constructive feedback provided** with explanations
- [ ] **Positive aspects highlighted** for reinforcement
- [ ] **Actionable recommendations given** with examples
- [ ] **Priority levels assigned** to all feedback items

## Final Review Decision

Based on the comprehensive analysis:

**✅ APPROVE**: Ready to merge with minor or no changes required
**📝 REQUEST CHANGES**: Significant issues that must be addressed before merge
**💬 COMMENT**: Feedback provided, author's discretion on implementation
**❓ NEEDS DISCUSSION**: Complex issues requiring team discussion
