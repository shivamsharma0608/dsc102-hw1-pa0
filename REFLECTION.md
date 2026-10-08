Team Information
Team Members:
Shivam Sharma
Luis Corona
AI Coding Gym User ID: 19wvffz89b6l
GitHub Handle: shivamsharma0608
Email: shivamsharma0608@gmail.com
Selected Challenge: xarray-4687 — “xr. where not preserving attributes”
Difficulty: Medium
Final Submission: 7e42916
Final Status: Passed

Our team used AI as a debugging and code-navigation partner while working on the `xarray-4687` challenge, which involved `xr.where()` failing to preserve attributes. Rather than immediately asking Codex to implement a solution, we first asked it to reproduce the bug, inspect the relevant Xarray source code, locate existing tests, and trace the problem through the codebase. This helped us understand an unfamiliar and relatively large project much faster. Codex traced `xr.where()` into `apply_ufunc()` and Xarray's attribute-merging logic, which showed us that the issue was not simply that attributes were being deleted. The correct solution also needed to determine which input's attributes should be preserved. We learned that for `xr.where(cond, x, y)`, the intended behavior was to preserve the attributes of `x` when `keep_attrs=True`.

The process also showed us why AI-generated solutions still require human review. Codex initially considered simply passing `keep_attrs=True` to `apply_ufunc()`, but further investigation showed that this could preserve attributes from `cond` instead of `x`. It later produced a post-processing solution that copied `x.attrs` onto the result and passed our local tests. Instead of accepting it because the tests passed, we asked Codex to compare that approach with Xarray's existing architecture and with evaluator feedback from a failed baseline solution. This led us to a better implementation that converts `keep_attrs=True` into a callable attribute-selection policy and passes it through `apply_ufunc()`. After completing the challenge, we compared our implementation with the Golden Patch and found that it used essentially the same approach. This taught us that passing tests are important, but understanding why a solution works and whether it follows the design of the existing codebase is equally important.

We encountered another failure during our first submission when AI Coding Gym reported the solution as failed. Instead of assuming the implementation was incorrect, we inspected the GitHub Actions logs and discovered that the actual tests had never run. Our added regression tests modified the same area of `test_computation.py` where the evaluator attempted to apply its own test patch, causing the evaluator's patch to be rejected. We diagnosed the problem as a testing/integration conflict, restored the test file to its original state while keeping the production fix, and submitted again. The second submission passed. Overall, this assignment taught us that AI is most useful when paired with active human verification. AI helped us navigate unfamiliar code and generate possible solutions quickly, while our role was to question its assumptions, interpret test and CI results, compare alternatives, and guide it toward a solution that was both correct and consistent with the project's architecture.
