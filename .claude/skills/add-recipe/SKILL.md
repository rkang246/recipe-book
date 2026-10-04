---
name: add-recipe
description: Research and add an easy, low-cleanup recipe to the recipes DATA array in recipes.html. Invoked as /add-recipe <dish + description>.
argument-hint: <dish name + short description>
allowed-tools: Read, Edit, WebSearch, WebFetch
---

# /add-recipe <dish + description>

Add a recipe to `recipes.html`. `$ARGUMENTS` is a dish name plus the user's
description (e.g. "rice cooker gyudon — beef bowl all in the rice cooker").

Read [`CLAUDE.md`](../../../CLAUDE.md) first for the data model.

## Guiding principle: ease of creation

The user's recipes are tailored for **minimal effort and minimal cleanup**.
When researching and writing the recipe:

- Prefer the method the user described (rice cooker, one bowl, sheet pan…). If
  the description implies a single vessel, keep it to that vessel — don't add a
  separate skillet "for better browning" unless it's optional and labeled so.
- Prefer pantry-friendly or easy-to-find ingredients; put specialty items'
  substitutions in `note`.
- Write steps tersely and concretely (amounts already in ingredients; steps say
  what to do and the doneness cue). Usually 4–8 steps.
- Be honest in `cleanup` about what gets dirty.

## Workflow

1. **Research.** Use WebSearch/WebFetch to cross-check 2–3 reputable versions of
   the dish in the requested style. Reconcile ratios (especially liquid ratios for
   rice cooker / pressure cooker recipes — these must be correct).
2. **Pick the category.** Use an existing `CATEGORIES` key where it fits. If none
   fits, propose a new category (key, label, color, blurb).
3. **Add it.** Append a recipe object to `DATA` in the exact shape in CLAUDE.md.
   Numeric `qty` values so scaling works; `null` for to-taste items. Set
   `servings` and `time` (minutes). Add 2–4 practical `tips`. Add `source` if a
   single source was followed closely.
4. **Touch only `DATA` / `CATEGORIES`.** Match the existing indentation style.
5. **Verify** the script still parses (e.g. extract the `<script>` and run
   `node --check`), then commit and push to `main`.
6. **Reply** with a 2–3 line summary (category, time, servings, key equipment)
   and the deep link:
   `https://htmlpreview.github.io/?https://github.com/rkang246/recipe-book/blob/main/recipes.html#<slug>`

If the user's request is ambiguous (e.g. which variant, or a judgment call on
category), make a sensible choice, add it, and mention the choice in the reply
so they can redirect — the user prefers iterating on a live link over upfront
questions.
