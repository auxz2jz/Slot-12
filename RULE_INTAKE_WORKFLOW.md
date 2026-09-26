# Rule Intake and Maintenance Workflow

This file defines how new reusable instructions should be added to the Master Instruction Library.

## Purpose

When the user provides a new instruction, rule, preference, development safeguard, diagnostic requirement, testing requirement, workflow rule, or project-wide standard, do not automatically append it blindly.

First determine the best way to integrate it into the existing library.

## Required intake process

For every proposed global rule:

1. Read the relevant existing instruction files.
2. Determine whether the new rule:
   - already exists,
   - clarifies an existing rule,
   - strengthens an existing rule,
   - conflicts with an existing rule,
   - belongs as a new subsection,
   - or requires a new instruction file/category.
3. Avoid duplicate wording when the same behavior is already covered.
4. Preserve stronger existing protections unless the user explicitly changes them.
5. If the new rule conflicts with an older rule, follow the user's latest explicit instruction and document the change.
6. Put the rule in the most appropriate file rather than simply appending it to the end of an unrelated document.
7. If a new category is needed, create a clearly named instruction file and add it to INSTRUCTION_INDEX.md.
8. Record meaningful changes in RULE_CHANGELOG.md.

## Integration preference

Prefer this order:

1. Merge into an existing rule when it is the same concept.
2. Add a subsection when it extends an existing category.
3. Create a new file only when the rule represents a distinct reusable category.

## Preserve clarity

The instruction library should remain:

- modular,
- readable,
- non-duplicative,
- internally consistent,
- easy for another developer or ChatGPT conversation to follow.

Do not allow the library to become one giant unsorted prompt.

## User shorthand

When the user says things such as:

- "Add this to my Master Instruction Library."
- "I want this rule in all my projects."
- "Save this as a global project rule."
- "I want future projects to follow this."
- or otherwise clearly indicates a reusable cross-project instruction,

treat the message as a request to run this intake process and update the library accordingly.

## This chat as intake point

This conversation may be used as the user's ongoing place to submit new reusable instructions. For each new instruction, determine the best integration strategy and update the canonical repository rather than requiring the user to decide which file it belongs in.
