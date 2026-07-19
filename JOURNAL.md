## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/64

**Issue title:** Prompt injection defense doesn't sanitize newline characters in user-supplied resume text

**Tier:** [ ] Tier 1  [x] Tier 2  [ ] Tier 3

**Problem summary:**
PathReview runs user resume text through a prompt-injection defense before generation. `PromptDefense.is_injection_attempt()` already flags dangerous newline patterns such as `\n---\n` and `\nSystem:`, but `PromptDefense.sanitize()` only strips characters like `<`, `>`, `{`, and `}`. That means adversarial resume text can still keep fake prompt boundaries after sanitization, so the model may treat injected lines as new system instructions. A successful fix should harden `sanitize()` in `safety/prompt_defense.py` so those newline injection patterns are neutralized (not only detected), with unit tests covering the cases called out in the issue.

**Selection notes (“Is this right for me?”):**
This is Tier 2 and scoped mainly to `safety/prompt_defense.py` plus existing unit tests — clearer than a Tier 3 cross-request monitoring change. Effort estimate is 4–6 hours, which fits the Module 3 window. I can explain the gap (detect vs sanitize), reproduce with a short malicious resume snippet, and verify with tests. Scope risk looks manageable if I stay focused on sanitizing known patterns without redesigning the whole safety pipeline.

**Branch name:** fix/64-prompt-defense-harden-sanitize

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger
