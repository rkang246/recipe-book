# Recipes project

A self-contained, offline-capable recipe collection focused on **ease of
creation and easy cleanup** (one-bowl / one-pot, rice cooker, sheet pan, etc.),
organized into category sections.

## Where the data lives

**All data lives in the `DATA` array (and the `CATEGORIES` list just above it)
inside [`recipes.html`](recipes.html).** There is no database, build step, or
external file. Open `recipes.html` in a browser to view it.

Live preview (GitHub HTML preview):
https://htmlpreview.github.io/?https://github.com/rkang246/recipe-book/blob/main/recipes.html

Deep-link to a recipe by appending `#<slug>` (lowercased name, non-alphanumerics
→ `-`), e.g. `…/recipes.html#rice-cooker-gyudon`.

## Data model

```js
const CATEGORIES = [
  { key: "one-bowl", label: "One Bowl / One Pot", color: "#ffa94d", blurb: "Everything in a single vessel — minimal cleanup." },
  // ...display order = page order
];

const DATA = [
  {
    name: "Rice Cooker Gyudon",           // card heading; also the URL slug
    category: "one-bowl",                 // a CATEGORIES key
    description: "One or two sentences.", // shown on card + detail
    cuisine: "Japanese",                  // optional
    equipment: ["Rice cooker"],           // drives the Equipment filter
    cleanup: "Rice cooker pot + cutting board", // what you wash afterwards
    servings: 3,                          // base servings (enables the scaler)
    time: { prep: 10, cook: 50 },         // minutes
    tags: ["beef", "rice"],               // searchable keywords
    ingredients: [
      { group: "Sauce", items: [          // group label optional
        { qty: 3, unit: "tbsp", item: "soy sauce", note: "optional detail" }
      ]}
    ],
    steps: ["Step one.", "Step two."],
    tips: ["Optional tip."],              // optional
    source: { label: "Site", url: "https://..." } // optional
  }
];
```

### Field notes

| Field | Meaning |
|-------|---------|
| `category` | Exactly one `CATEGORIES` key. Pick the category that best describes *why the recipe is easy* (method/vessel) unless it's clearly a course (appetizer, side, dessert). |
| `equipment` | Main appliances/vessels needed (Rice cooker, Instant Pot, Sheet pan, Dutch oven, Air fryer, Skillet, Mixing bowl…). Use consistent capitalization so filters merge. |
| `cleanup` | Short list of what gets dirty. Shown as a green 🧽 badge — keep it honest. |
| `qty` | A **number** (decimals OK: `0.5`, `0.333`) so servings scaling works; renders as fractions (½, ⅓, ¾). Use `null` for "to taste" / unscaled items. |
| `unit` | Free text (`tbsp`, `g`, `cups`, `rice-cooker cups`) or `""` for counted items. |
| `note` | Prep detail or substitutions, rendered muted after the item. |

## Ingredient search

The **Ingredients** box is a multi-select autocomplete built from every
ingredient `item` in `DATA`. Each selected chip narrows results (recipes must
contain **all** chips). Matching is word-prefix/substring with one-typo
tolerance on ingredient names (`shav` → `shabu-shabu beef`); `note` text is
searched exactly. Typing a word that matches several ingredients offers an
`Any "<word>"` option first. Implication for data entry: **name `item` by the
term you'd search for** (e.g. `"shabu-shabu beef"`, not `"thinly sliced
beef"`), and put prep/cut details in `note`.

## Editing rules

- **Only edit `DATA` (and `CATEGORIES` when a new category is genuinely needed).**
- Leave the HTML/CSS/JS rendering code intact.
- Recipes are sorted alphabetically within a category automatically.
- Empty categories are hidden automatically, so defining a category ahead of
  time is harmless.

## Commands

- `/add-recipe <dish + description>` — see
  [`.claude/skills/add-recipe/SKILL.md`](.claude/skills/add-recipe/SKILL.md).
