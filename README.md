# Contribution 5869: Handle file name encoding and special characters.
]

**Contribution Number:** 1
**Student:** Mariam Jafri
**Issue:** https://github.com/apache/gravitino/issues/5869
**Status:** Phase 1

---

## Why I Chose This Issue

I wanted to make sure the issue was doable for me which is why I chose something with languages including Java, Python, and JavaScript, and the issue seemed to be open. I want to practice testing software as well. 

---

## Understanding the Issue

### Problem Description

Looking to test special character encodings to see if there are any bugs or if it is running smoothly 

### Expected Behavior

[What should happen?]

### Current Behavior

[What actually happens?]

### Affected Components

clients/client-python/gravitino/filesystem/gvfs_storage_handler.py 
clients/client-python/gravitino/filesystem/gvfs_utils.py — add helper if needed?
clients/client-python/tests/unittests/test_gvfs_filename_encoding.py — new test file

---

## Reproduction Process

### Environment Setup

Because we are testing, we need to stay in the development environment. So, I cloned the repo and then downloaded rust. 

### Steps to Reproduce

1. Clone the repo
2. Download rust with: curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
3. cd client to see client files

### Reproduction Evidence
- no reproduction of tests bc need to write tests first!

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

In gvfs_storage_handler.py there is no handling of non-ASCII encodings. This means the functions actual_path_to_gvfs_path() and actual_info_to_gvfs_info() could fail. 

### Proposed Solution

Using the actual_path_to_gvfs_path() in gvfs_storage_handler.py, I will test the function with non-ASCII encodings. 

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** In gvfs_storage_handler.py there is no handling of non-ASCII encodings. This means the functions actual_path_to_gvfs_path() and actual_info_to_gvfs_info() could fail. 

**Match:** Existing integrations exists like test_gvfs_with_s3.py, test_gvfs_with_gcs.py, and test_gvfs_with_hdfs.py in clients/client-python/tests/integration/ show the pattern for how GVFS file operations are tested

**Plan:** 
1. Run existing local tests (cd clients/client-python and pytest tests/ -k "local" -v)
2. Create a new test file following the integration test files above 
3. Write unit tests for actual_path_to_gvfs_path() for filenames such as spaces, signs such as #, %, &, ?, unicode (ex. other languages), emojis, and other special characters ex. /
4. If tests fail, test where the encoding/decoding needs to be
5. Check gvfs_utils.py for any handlers 

**Implement:** https://github.com/mariamjafri/gravitino/tree/fix-issue-5869

**Review:** From contributing.md: 
- Use feature branches
- Write clear commit messages and PR descriptions
- Link to issues (e.g., Fixes #123)
- Respond to reviewer feedback
- Follow Java and Python idioms
- Include useful comments
- Format with Spotless

**Evaluate:** The new tests cases should pass with pytest tests/unittests/test_gvfs_filename_encoding.py -v and baseline from above should also still pass 

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
