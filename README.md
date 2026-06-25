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

Unit tests should reveal whether the functions are passing with special characters or not. 

### Current Behavior

There are no tests for special characters yet. 

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

- [ ] Test case : test_actual_path_to_gvfs_path_special_filenames: A special-character file name maps back to the matching gvfs path
- [ ] Test case 2: test_actual_path_to_gvfs_path_special_nested_dirs(self): Special characters inside nested directory segments survive
- [ ] Test case 3: test_actual_info_to_gvfs_info_special_filenames(self): File-info conversion rewrites the name and passes metadata through.
- [ ] Test case 4: test_round_trip_special_chars_in_sub_path(self): A gvfs path with a special-character sub-path round-trips cleanly
- [ ] Test case 5: test_round_trip_special_chars_in_fileset_name(self): Special characters in the fileset name segment are preserved
- [ ] Test case 6: test_actual_path_to_gvfs_path_recurring_prefix_segment(self): actual_path_to_gvfs_path uses str.replace(actual_prefix, ...), which replaces *every* occurrence of the prefix string rather than only the leading one.

### Integration Tests

- N/A

### Manual Testing

pytest tests/unittests/test_gvfs_filename_encoding.py -v → **5 passed, 1 xfailed, 100 subtests passed**.

---

## Implementation Notes

### Week 1 Progress

Wrote unit tests and tested. 

### Code Changes

- **Files modified:** tests/unittests/test_gvfs_filename_encoding.py
- **Key commits:** https://github.com/apache/gravitino/compare/main...mariamjafri:gravitino:fix-issue-5869
- **Approach decisions:** Focused on testing each of the specified functions with a list of special characters. 

---

## Pull Request

**PR Link:** https://github.com/apache/gravitino/pull/11797

**PR Description:** Added a new unit-test file: tests/unittests/test_gvfs_filename_encoding.py which tests file-name encodings that contain special characters in the GVFS path-conversion layer. 

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** Awaiting Review

---

## Learnings & Reflections

### Technical Skills Gained

How to write and format unit tests - for example using @unittest.expectedFailure for tests that fail. 

### Challenges Overcome

Just understanding how many tests were needed. For example the last unit test may not be needed but was a technical encoding flaw, although hard to envounter because of the previous filtering lines. 

### What I'd Do Differently Next Time

Start writing the tests earlier. I was nervous to start but it was a lot easier than expected. 

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
