---
name: elixir-anti-patterns-review
description: Spots Elixir anti-patterns in user code and suggests concrete refactors. Treats them as checklists rather than hard rules; calls out issues with evidence (file/line or snippet) and proposes minimal, idiomatic fixes. Covers 4 anti-pattern categories - Code, Design, Macro, and Process.
license: Complete terms in LICENSE
---

# Elixir Anti-Pattern Review Skill

This skill provides needed context to check elixir code against the anti-pattern guidelines.

## Purpose

This skill enables Claude to identify and address Elixir anti-patterns during code reviews, refactoring sessions, and new code development. When reviewing or writing Elixir code, Claude should:

- **Detect anti-patterns** by comparing code against the documented patterns below
- **Provide evidence-based feedback** with specific file paths, line numbers, or code snippets
- **Suggest minimal, idiomatic refactors** that address the issue without over-engineering
- **Treat patterns as guidelines** rather than strict rules—context matters, and some patterns may be acceptable depending on the situation

The anti-patterns are organized into four categories that can be referenced based on the type of code being reviewed:
- **Code-related**: Language idioms, syntax patterns, and Elixir-specific features
- **Design-related**: Module organization, function design, and API boundaries
- **Process-related**: GenServer, Agent, supervision trees, and concurrency patterns
- **Meta-programming**: Macros, code generation, and compile-time concerns

## Usage Instructions

### When to Apply This Skill

Activate this skill when:
- Reviewing Elixir/Phoenix pull requests or code changes
- Refactoring existing Elixir codebases
- Writing new Elixir modules, functions, or processes
- Debugging issues that may stem from architectural problems
- User mentions "anti-pattern", "code smell", "refactor", or "review" in context of Elixir

### How to Use

1. **Identify the relevant category** based on what's being reviewed:
   - Working with functions, variables, pattern matching → Check **Code-related** anti-patterns
   - Designing modules, APIs, or data structures → Check **Design-related** anti-patterns
   - Using GenServer, Agent, Tasks, or supervision → Check **Process-related** anti-patterns
   - Writing or reviewing macros → Check **Meta-programming** anti-patterns

2. **Compare code against patterns** documented below, looking for matches

3. **When an anti-pattern is detected**:
   - Name the specific anti-pattern
   - Quote the problematic code with file/line reference
   - Explain why it's problematic in this context
   - Provide a concrete refactored example

4. **Exercise judgment**: Not every match requires action. Consider:
   - Is this a one-off case or a recurring pattern?
   - Does the refactoring provide meaningful benefit?
   - Are there trade-offs that justify the current approach?

### Multiple Violations

A single piece of code can violate multiple anti-patterns simultaneously. When this occurs:

- **Report each anti-pattern separately** with its own name and explanation
- **Prioritize by impact**: Address the most significant issues first (e.g., security concerns like dynamic atom creation before style issues like comment overuse)
- **Look for root causes**: Multiple violations often share a common underlying issue—fixing the root cause may resolve several anti-patterns at once
- **Consider combined refactoring**: When violations overlap, propose a single refactored solution that addresses all issues rather than incremental fixes

**Example**: A GenServer that uses `String.to_atom/1` on user input while also scattering its interface across modules violates both "Dynamic atom creation" and "Scattered process interfaces". The refactoring should address both: centralize the interface AND use `String.to_existing_atom/1`.

### Example Review Output

~~~
Anti-pattern detected: Non-assertive map access

In `lib/my_app/users.ex:45`:

```elixir
def get_email(user), do: user[:email]
```

Since `email` is a required field on `User`, use static access for clarity and fail-fast behavior:

```elixir
def get_email(user), do: user.email
```
~~~

## What are anti-patterns?

Anti-patterns describe common mistakes or indicators of problems in code.
They are also known as "code smells".

The goal of these guides is to document potential anti-patterns found in Elixir software
and teach developers how to identify them and their pitfalls. If an existing piece
of code matches an anti-pattern, it does not mean your code must be rewritten.
Sometimes, even if a snippet matches a potential anti-pattern and its limitations,
it may be the best approach to the problem at hand. No codebase is free of anti-patterns
and one should not aim to remove all of them.

The anti-patterns in these guides are broken into 4 main categories:

  * **Code-related anti-patterns:** related to your code and particular
    language idioms and features;

  * **Design-related anti-patterns:** related to your modules, functions,
    and the role they play within a codebase;

  * **Process-related anti-patterns:** related to processes and process-based
    abstractions;

  * **Meta-programming anti-patterns:** related to meta-programming.

Each anti-pattern is documented using the following structure:

  * **Name:** Unique identifier of the anti-pattern. This name is important to facilitate
    communication between developers;

  * **Problem:** How the anti-pattern can harm code quality and what impacts this can have
    for developers;

  * **Example:** Code and textual descriptions to illustrate the occurrence of the anti-pattern;

  * **Refactoring:** Ways to change your code to improve its qualities. Examples of refactored
    code are presented to illustrate these changes.

## For code-related anti-patterns, see [code-anti-patterns.md](anti-patterns/code-anti-patterns.md)

## For design-related anti-patterns, see [design-anti-patterns.md](anti-patterns/design-anti-patterns.md)

## For macro-related anti-patterns, see [macro-anti-patterns.md](anti-patterns/macro-anti-patterns.md)

## For process-related anti-patterns, see [process-anti-patterns.md](anti-patterns/process-anti-patterns.md)
