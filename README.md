# PR #417: isolated offline naming reproduction

These are real screenshots of the native Beluga app displaying outputs calculated from the old and fixed Claude completion code. They are **an offline reproduction in a separate demo environment**, not screenshots of the reporter's original account failure or a live Magpie request.

Input task: `Review branch cleanup`

The deterministic mocked SDK result has `type: 'result'`, `subtype: 'success'`, `is_error: true`, and `result: 'Failed to authenticate. OAuth session expired. Please log in again.'`.

The reproduction evaluated the PR's actual `claudeComplete` implementation with the SDK query mocked to yield that result, then called Beluga's existing `nameTask`. The before variant removes only the `!msg.is_error` condition; the after variant keeps it. Result data is in [reproduction.json](reproduction.json).

- Before: `failed-to-authenticate-oauth-session-exp`
- After: `review-branch-cleanup` (existing local fallback from the task words)

The two resulting strings were applied through the supported workspace rename API to the same isolated demo worktree. The UI did not initiate a provider request. Renaming also updated the demo branch. No real project, credentials, or external model API was used.

## Before

![Before: authentication error used as worktree and branch name](before-native-name.png)

## After

![After: task-based local fallback name](after-native-name.png)

Both captures use the same native 1394 × 860 point window, 2× screenshot scale, sidebar width and hidden inspector. Each is cropped at x=500, y=0 to 2288 × 110 pixels to show only the relevant toolbar. The native UI is the locally installed version-compatible v3.0.0-mac proof build; the completion code under test is PR head `6087d7b4c6ff88455e676441fc0638a82737f014`.

## Magpie context and compatibility

The reporter uses Magpie as a local provider gateway. Provider routing and model aliases are configured under the `env` object in the owner's Claude user settings. Beluga's isolated Claude side call uses `settingSources: []`, so that file is not loaded; inherited process environment is a separate mechanism. In the reported setup, the side call encountered an expired OAuth result instead of the intended gateway route.

Provider configuration reaching the side call and rejecting an SDK error result are separate concerns. PR #417 fixes the latter. It does not promise to load settings-only Magpie configuration. It preserves settings isolation, uses structured protocol status instead of error-text matching, and introduces no provider-specific endpoint, credential, model alias, or settings hook loading.

## Validation

- Root and companion typechecks passed.
- Focused side-call/Codex/OpenCode/Copilot suites: 186 tests passed in 15 files.
- Complete Vitest suite: 7,428 passed, 4 skipped; 612 test files passed, 1 skipped.
- The 32 new non-Claude protocol tests include startup cancellation: the three corresponding regressions failed before the three one-line fixes and passed afterward.
- Protocol fixtures are offline; these checks do not claim live-provider certification.
