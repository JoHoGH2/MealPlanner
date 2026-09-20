# My Meal Planner

A local-first, GitHub Pages-ready weekly meal planner. It keeps the important distinction between portions eaten, portions cooked fresh, and portions taken from the freezer.

## Features

- Monday–Sunday planner with breakfast, lunch, and dinner.
- Per-meal **Eat**, **Cook**, and **From freezer** values.
- Recipe book with categories, base portions, ingredients, units, shopping categories, instructions, and timings.
- Automatic ingredient scaling from recipe base portions.
- Batch cooking and freezer balance calculation.
- Shopping list generated from fresh `Cook` portions only, grouped by category and combined by ingredient/unit.
- Checked-off and manual shopping items.
- JSON import/export backup.
- No backend, build step, account, or paid service: data is stored in `localStorage`.

## Run locally

Open `index.html` in a browser, or serve the folder with any static web server:

```bash
python3 -m http.server
```

Then visit <http://localhost:8000>.

## GitHub Pages

In the repository settings, open **Pages**, choose **Deploy from a branch**, select `main` and `/ (root)`, and save. GitHub Pages will serve `index.html`.

## Ingredient format

When adding a recipe, enter one ingredient per line using:

```text
quantity | unit | ingredient name | shopping category
```

For example:

```text
500 | g | beef mince | Meat & Fish
1 | | onion | Produce
```

## Calculation model

- Shopping quantities use `Cook / base portions` for each planned meal.
- Freezer balance adds `max(0, Cook - Eat)` and subtracts `From freezer`.
- A freezer-only meal (`Cook = 0`) contributes nothing to shopping.
- A mixed meal contributes only its fresh `Cook` portions to shopping.

The application is intentionally a static first version so it can later gain accounts, sync, pantry inventory, or a PWA without changing the core meal model.
