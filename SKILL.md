---
name: code-style
description: Rules for writing and reviewing code: the shortest version that works, one job per function or module, names that make the flow readable, typed signatures and typed constants, keyword arguments, files ordered as you would explain them, classes only where data lives, qualified calls, entry points apart from libraries, following the codebase, errors that surface, comments that say why, tests that fail on broken code, code that matches the proposal it implements, and UI without explanatory text. Load it on any turn that writes, edits or reviews code.
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
  name or a helper that obscures it. Readability outranks line count here and
  in the three rules below.
- **Type every function signature.** In a dynamically typed language, use its
  type annotations (Python hints, TypeScript over plain JavaScript where the
  project allows). Give a domain type used in two or more places a named alias
  (`Cells`). Run the language's strict type checker where the project has one,
  such as `mypy --strict`, `"strict": true` in tsconfig, or the compiler's
  warnings as errors.
- **The type goes in the annotation, not the name.** Write `cells: Cells`, not
  `cells_tuple`. Collections are plural, booleans start with `is` or `has`,
  counts start or end with `count`, and maps are named `X_by_Y`. Follow the
  language's casing (`is_ready` in Python, `isReady` in TypeScript or Kotlin).
  - Group related constants into the language's enum type, named for their
    kind: `Owner.HUMAN`, not a bare `HUMAN = 1`. Give members their own type
    when the language allows it, so they never compare equal to plain numbers
    or to another enum's members. Use number-backed members only where the
    values must be numbers. Where the language has no enum, use its closest
    form: a string union in TypeScript, a frozen object in JavaScript.
  - A lone constant states its type and that it never changes
    (`BOARD_SIZE: Final[int] = 7`, `const val BOARD_SIZE: Int = 7`).
  - A single sentinel stays the language's null value or a typed constant, and
    a yes-or-no state stays a boolean. Neither gets an enum.
- **A function with three or more parameters, or two that could be swapped,
  is called with its arguments named.** Use the language's named-argument form
  and require it where the language can: `cell_at(*, row, column)` in Python,
  named arguments in Kotlin or C#, one options object in TypeScript or
  JavaScript. A call with parameters of different types, such as
  `apply(state, move)`, stays positional. Calls into the standard library or
  another package follow that API. The same holds for a record or data class
  with three or more fields, or two that could be swapped: construct it with
  named fields, and require that where the language can (`kw_only=True` in a
  Python dataclass).
- **Name functions as spoken phrases, and order a file as you would explain it
  aloud.** A name is a verb and its object (`count_lighthouses(cells)`) or a
  question (`who_owns_each_cell(cells)`, `opponent_of(player)`). The file runs
  top-down: the entry point or main flow first, each helper after the code
  that calls it, where the language allows that order. Code that runs when the
  file loads and calls the file's own functions goes at the end: computed
  constants first, then the entry-point guard (`if __name__ == "__main__":`).
- **A class bundles data with the behaviour that reads or changes it.** It is
  either an object with a lifetime whose state changes (a running game, a
  clock), or an immutable value whose methods return new values (a game
  position). Functions that own no data stay module functions, and the module
  is the component boundary. A class with no data of its own is a module in
  disguise.
- **Call another module's functions through the module name**
  (`engine.apply(state, move)`), so a reader sees where each one lives. Import
  only types, classes and enums by name.
- **Keep entry points apart from modules that are only imported.** Pages,
  routes, command-line tools and scripts sit where a reader looks for things
  to run. Library modules never start anything when imported.
- **Follow the codebase.** Before adding a pattern, find how the repository
  already solves the same problem and do it that way. A second way of doing
  one thing is a cost every reader pays.
- **Let errors surface.** Catch an error only where the code can act on it:
  retry, fall back, or report it to the user. Never catch around code that
  cannot fail, never swallow an error silently, and never log and carry on
  with a value that is now wrong. Keep the checks at trust boundaries: user
  input, files, network calls and other outside APIs.
- **Comments are one-line docstrings in plain English, and a comment says why,
  never what.** Delete any comment that repeats what the code says. Keep one
  that records a decision or a failure a reader would otherwise repeat. Never
  leave a comment describing code that no longer exists ("used to",
  "previously", "no longer"). The reason for a change goes in the commit
  message.
- **A test must fail when the code is wrong.** Break the code once and confirm
  the test catches it. A test's name states the behaviour it checks
  (`test_lighthouse_blocks_light_beyond_it`). Test helpers are exempt from the
  typing rules above.
- **Code that implements a proposal is read against it, so it matches it
  exactly.**
  - Every name, constant and mechanism uses the proposal's wording. A name the
    proposal gives wins over the naming rules above.
  - Every number the proposal states is a named constant.
  - Every claim the proposal makes has a test that checks it.
- **UI carries no descriptions or instructions.** Most features are obvious
  from the controls themselves, and the people using an app already know the
  job. A feature's name is its whole explanation. Never stack text under it: no
  subheading that restates the name, no sentence saying what the feature does,
  no hint text walking through the steps, no footer saying how it works. A
  panel titled "Price Alerts" gets its alerts and its controls. It does not get
  "Get notified when prices move" under the title or "How it works: we compare
  the latest price with your target" at the bottom.
  - Keep only text that carries what the screen does not already show: an
    error, a count, a date, a sender, or what an action will destroy that its
    label does not say.
  - Test each string. If it would still be true on a different screen, or the
    user could write it themselves after one look, delete it.
  - Every animation is one the user can explain step by step.
