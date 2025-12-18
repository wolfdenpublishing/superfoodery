# Ron's Superfood Kitchen - Claude Code Project

## Project Overview

This project manages a personal recipe collection focused on evidence-based "superfoods" and clean eating. Recipes are developed through conversation, then exported as HTML files with schema.org markup for import into AnyList. The site is hosted via GitHub Pages.

## Key Files

### Reference Documents

- **Superfood_Eating_Plan.md** - The foundational document defining Ron's eating framework. Contains:
  - Approved superfoods list with health benefits
  - Calorie targets by phase (1,500 fat loss / 2,000 transition / 2,500 maintenance)
  - OMAD (One Meal A Day) meal structure
  - Nutritional coverage targets and daily checklist
  - Meal prep strategies and sample recipes
  - **Always reference this when developing recipes** to ensure ingredients align with the plan

- **Recipe_HTML_Generation_Instructions.md** - Technical specification for creating HTML recipe files. Contains:
  - Schema.org JSON-LD template for AnyList compatibility
  - Complete HTML template with styling
  - Ingredient formatting guidelines
  - Filename conventions
  - **Follow this exactly when generating HTML recipe files**

### Website Files

- **index.html** - Recipe index page listing all recipes with nutrition info and links
  - **Must be updated whenever a new recipe HTML file is created**
  - Organized by category: Main Dishes, Side Dishes, Snacks, Desserts, Sauces & Dressings
  - Update the recipe count in the header
  - Add new recipe card in appropriate category section

- **[Recipe_Name].html** - Individual recipe pages (e.g., `Peach_Chia_Pudding.html`)
  - Contains both human-readable recipe and JSON-LD structured data
  - AnyList browser extension reads the JSON-LD for import

## Typical Workflow

### 1. Recipe Development (Conversation Phase)
- User describes a recipe idea or requests suggestions
- Reference `Superfood_Eating_Plan.md` to ensure ingredients align with the plan
- Develop recipe with ingredients, method, and calorie calculations
- Iterate based on user feedback until recipe is finalized

### 2. HTML Generation
When user says to create the HTML file (or similar):
1. Read `Recipe_HTML_Generation_Instructions.md` for the template
2. Create `[Recipe_Name_With_Underscores].html` following the template exactly
3. Include complete JSON-LD structured data in the `<head>`
4. Include human-readable recipe in the `<body>`
5. Verify nutrition values match the developed recipe

### 3. Index Update
After creating the recipe HTML:
1. Open `index.html`
2. Increment the recipe count in the header
3. Add a new recipe card in the appropriate category section
4. If the category doesn't exist, create a new `<h2>` section for it

### 4. Git Commit
After both files are ready:
```bash
git add [Recipe_Name].html index.html
git commit -m "Add [Recipe Name] recipe"
git push
```

## Recipe Development Guidelines

### Calorie Awareness
- **Fat Loss Phase (current)**: ~1,500 cal/day
- Main meals: typically 800-1,200 calories
- Snacks: typically 200-400 calories
- Always provide per-serving nutrition breakdown

### Flavor Preferences
- Hispanic heritage, loves spicy food
- Has homemade habañero powder available
- Favorite fruits: peaches (#1), strawberries (#2), blueberries (#3)
- Prefers bold, authentic flavors over bland "diet food"

### Equipment Available
- Instant Pot (preferred for building complex flavors)
- Air fryer
- Standard oven, stovetop, toaster oven
- Blenders (bullet and standard)

### Key Superfoods to Incorporate
Reference the full list in `Superfood_Eating_Plan.md`, but prioritize:
- Omega-3 sources: salmon, walnuts, chia seeds, flaxseeds
- Cruciferous vegetables: broccoli, cauliflower, Brussels sprouts, cabbage
- Alliums: garlic, onions
- Anti-inflammatory spices: turmeric (with black pepper), ginger
- Berries for antioxidants
- Quality olive oil as finishing fat

### Foods to Avoid
- Sardines, anchovies (taste preference)
- Brazil nuts (taste preference)
- Kefir (not tolerated)
- Processed foods, artificial additives

## File Naming Convention

Recipe HTML files: `[Recipe_Name_With_Underscores].html`

Examples:
- `Crispy_Superfood_Chicken_Strips.html`
- `Chicken_Fajita_Soup_Instant_Pot.html`
- `Peach_Chia_Pudding.html`

## Categories for index.html

- Main Dishes
- Side Dishes
- Snacks
- Desserts
- Sauces & Dressings

## Quick Reference: Recipe Card Template

When adding to index.html, use this structure:
```html
<div class="recipe-card">
    <h3><a href="[FILENAME].html">[RECIPE NAME]</a></h3>
    <p class="recipe-description">[2-3 sentence description]</p>
    <div class="recipe-meta">
        <span>⏱️ [X] min total</span>
        <span>🍽️ [X] servings</span>
        <span>📦 Meal Prep Friendly</span>
    </div>
    <div class="nutrition-highlight">
        <div class="item"><span class="value">[X]</span><span class="label">Calories</span></div>
        <div class="item"><span class="value">[X]g</span><span class="label">Protein</span></div>
        <div class="item"><span class="value">[X]g</span><span class="label">Fat</span></div>
        <div class="item"><span class="value">[X]g</span><span class="label">Carbs</span></div>
        <div class="item"><span class="value">[X]g</span><span class="label">Fiber</span></div>
    </div>
    <div class="tags">
        <span class="tag primary">Superfood</span>
        <span class="tag">[Relevant Tag]</span>
    </div>
</div>
```
