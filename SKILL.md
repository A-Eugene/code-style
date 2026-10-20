---
name: code-style
description: Rules for writing and reviewing code: the shortest version that works, one job per function or module, names that make the flow readable, one-line docstrings, code that matches the proposal it implements, and UI without explanatory text. Load it on any turn that writes, edits or reviews code.
---

# code-style

- **Shortest working version.** Add no abstraction the task does not need: no
  interface with one implementation, no factory for one product, no config for
  a value that never changes. Use the standard library before a dependency.
  Use no machine learning.
- **One function or module does one job.** This is the only SOLID rule that
  applies by default. The other four push toward an interface with one
  implementation, or matter only where real subclass trees exist, both of which
  the rule above rules out. A project with several genuine implementations of
  one interface adds them in its own CLAUDE.md.
- **Name for readability, and let a good name earn its line.** A named
  intermediate that explains a step beats the dense one-liner it replaces.
  `is_overnight = start.hour >= 18` reads better than burying the comparison
  inside a filter. Name what a thing is. Never hide a value or a step behind a
  name or a helper that obscures it. This is the one place readability
  outranks line count.
- **Comments are one-line docstrings in plain English. Delete any comment that
  repeats what the code says.** Never leave a comment describing code that no
  longer exists ("used to", "previously", "no longer"). The reason for a change
  goes in the commit message.
- **Code that implements a proposal is read against it, so it matches it
  exactly.**
  - Every name, constant and mechanism uses the proposal's wording.
  - Every number the proposal states is a named constant.
  - Every claim the proposal makes has a test that checks it.
- **UI carries no explanatory text.** A feature's name is its explanation. Keep
  only text the screen does not already show: an error, a count, a date, or
  what an action will destroy. Every animation is one the user can explain step
  by step.
