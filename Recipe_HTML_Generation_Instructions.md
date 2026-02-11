# Recipe HTML Generation Instructions

## Purpose

This document provides instructions for generating HTML recipe files that can be imported into AnyList using the schema.org Recipe markup format. These HTML files serve as the bridge between recipes developed in conversation and the AnyList recipe management system.

---

## Overview

AnyList imports recipes from web pages that use the **schema.org Recipe** structured data format. We use **JSON-LD** (JavaScript Object Notation for Linked Data) embedded in the HTML `<head>` section, which is the preferred format for search engines and recipe apps.

When the user wants to add a recipe to AnyList:
1. Generate an HTML file following this template
2. User downloads and opens the file in their browser
3. User clicks the AnyList browser extension to import

---

## Required Schema.org Fields

The following fields should be included in every recipe:

### Essential Fields
| Field | Description | Example |
|-------|-------------|---------|
| `@context` | Always "https://schema.org/" | `"https://schema.org/"` |
| `@type` | Always "Recipe" | `"Recipe"` |
| `name` | Recipe title | `"Crispy Superfood Chicken Strips"` |
| `description` | 2-3 sentence description including superfood benefits | |
| `recipeIngredient` | Array of ingredient strings | `["1 lb chicken thighs", "1/2 cup almond flour"]` |
| `recipeInstructions` | Array of HowToStep or HowToSection objects | See structure below |

### Timing Fields (ISO 8601 Duration Format)
| Field | Format | Example |
|-------|--------|---------|
| `prepTime` | PT[hours]H[minutes]M | `"PT20M"` (20 minutes) |
| `cookTime` | PT[hours]H[minutes]M | `"PT1H30M"` (1 hour 30 min) |
| `totalTime` | PT[hours]H[minutes]M | `"PT1H50M"` |

### Yield & Servings
| Field | Description | Example |
|-------|-------------|---------|
| `recipeYield` | Number of servings or amount produced | `"4 servings"` |

### Nutrition (NutritionInformation object)
| Field | Description | Example |
|-------|-------------|---------|
| `servingSize` | Description of one serving | `"1 serving (approx 4 oz)"` |
| `calories` | Calories with unit | `"334 calories"` |
| `proteinContent` | Protein in grams | `"30g"` |
| `fatContent` | Fat in grams | `"19g"` |
| `carbohydrateContent` | Carbs in grams | `"11g"` |
| `fiberContent` | Fiber in grams | `"3.5g"` |

### Metadata
| Field | Description | Example |
|-------|-------------|---------|
| `author` | Person or Organization object | `{"@type": "Person", "name": "Ron's Superfood Kitchen"}` |
| `datePublished` | ISO date | `"2024-12-18"` |
| `recipeCategory` | Meal type | `"Main Dish"`, `"Snack"`, `"Side Dish"` |
| `recipeCuisine` | Cuisine style | `"American"`, `"Mediterranean"` |
| `keywords` | Comma-separated tags | `"superfood, high-protein, meal prep"` |

---

## Instructions Structure

Instructions can be formatted two ways:

### Simple (Array of HowToStep)
```json
"recipeInstructions": [
    {
        "@type": "HowToStep",
        "text": "Preheat oven to 425°F."
    },
    {
        "@type": "HowToStep",
        "text": "Mix dry ingredients in a bowl."
    }
]
```

### Grouped by Section (Array of HowToSection)
Use this for recipes with distinct components or methods:
```json
"recipeInstructions": [
    {
        "@type": "HowToSection",
        "name": "Prepare the Coating",
        "itemListElement": [
            {
                "@type": "HowToStep",
                "text": "Mix almond flour and spices in a shallow bowl."
            },
            {
                "@type": "HowToStep",
                "text": "Beat egg in a separate bowl."
            }
        ]
    },
    {
        "@type": "HowToSection",
        "name": "Cook the Chicken",
        "itemListElement": [
            {
                "@type": "HowToStep",
                "text": "Dip chicken in egg, then coat with flour mixture."
            }
        ]
    }
]
```

---

## Ingredient Formatting Guidelines

For best AnyList parsing:

1. **Start with quantity**: `"1 lb chicken thighs"` not `"Chicken thighs, 1 lb"`
2. **Use standard abbreviations**: tbsp, tsp, lb, oz, cup, g
3. **Include prep notes after comma**: `"1 medium sweet potato, cut into wedges"`
4. **Keep each ingredient atomic**: One ingredient per array item
5. **Spell out fractions or use unicode**: `"½ cup"` or `"1/2 cup"`

### Example Ingredient Array
```json
"recipeIngredient": [
    "1 lb boneless skinless chicken thighs, cut into strips",
    "1 large egg, beaten",
    "1/2 cup almond flour",
    "2 tbsp ground flaxseed",
    "1 tsp smoked paprika",
    "1/2 tsp turmeric",
    "Salt and pepper to taste"
]
```

---

## Complete JSON-LD Template

```json
{
    "@context": "https://schema.org/",
    "@type": "Recipe",
    "name": "[RECIPE NAME]",
    "author": {
        "@type": "Person",
        "name": "Ron's Superfood Kitchen"
    },
    "description": "[2-3 sentences describing dish and superfood benefits]",
    "datePublished": "[YYYY-MM-DD]",
    "prepTime": "PT[X]M",
    "cookTime": "PT[X]M",
    "totalTime": "PT[X]M",
    "recipeYield": "[X] servings",
    "recipeCategory": "[Main Dish/Side Dish/Snack/Dessert]",
    "recipeCuisine": "[Cuisine Type]",
    "keywords": "superfood, [other relevant tags]",
    "nutrition": {
        "@type": "NutritionInformation",
        "servingSize": "[Description of 1 serving]",
        "calories": "[X] calories",
        "proteinContent": "[X]g",
        "fatContent": "[X]g",
        "carbohydrateContent": "[X]g",
        "fiberContent": "[X]g"
    },
    "recipeIngredient": [
        "[Ingredient 1]",
        "[Ingredient 2]"
    ],
    "recipeInstructions": [
        {
            "@type": "HowToSection",
            "name": "[Section Name]",
            "itemListElement": [
                {
                    "@type": "HowToStep",
                    "text": "[Step instruction]"
                }
            ]
        }
    ]
}
```

---

## Complete HTML Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[RECIPE NAME]</title>
    <script type="application/ld+json">
    {
        [JSON-LD RECIPE DATA HERE]
    }
    </script>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            line-height: 1.6;
            color: #333;
        }
        h1 {
            color: #2c5530;
            border-bottom: 3px solid #4a7c4e;
            padding-bottom: 10px;
        }
        h2 {
            color: #4a7c4e;
            margin-top: 30px;
        }
        h3 {
            color: #666;
        }
        .meta {
            background: #f5f5f5;
            padding: 15px;
            border-radius: 8px;
            margin: 20px 0;
        }
        .meta span {
            margin-right: 20px;
        }
        .nutrition {
            background: #e8f5e9;
            padding: 15px;
            border-radius: 8px;
            margin: 20px 0;
        }
        .nutrition-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            text-align: center;
        }
        .nutrition-item {
            padding: 10px;
        }
        .nutrition-value {
            font-size: 1.4em;
            font-weight: bold;
            color: #2c5530;
        }
        .nutrition-label {
            font-size: 0.85em;
            color: #666;
        }
        .ingredients-section {
            background: #fff;
            border: 1px solid #ddd;
            border-radius: 8px;
            padding: 20px;
            margin: 20px 0;
        }
        .ingredient-group h3 {
            margin-bottom: 10px;
            color: #4a7c4e;
        }
        ul {
            padding-left: 20px;
        }
        li {
            margin-bottom: 8px;
        }
        .instructions ol {
            padding-left: 20px;
        }
        .instructions li {
            margin-bottom: 12px;
        }
        .section-header {
            background: #4a7c4e;
            color: white;
            padding: 8px 12px;
            border-radius: 4px;
            margin: 20px 0 10px 0;
            font-weight: bold;
        }
        .notes {
            background: #fff8e1;
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #ffc107;
            margin: 20px 0;
        }
        .superfood-badge {
            display: inline-block;
            background: #4a7c4e;
            color: white;
            padding: 3px 10px;
            border-radius: 15px;
            font-size: 0.85em;
            margin: 2px;
        }
        .tags {
            margin: 15px 0;
        }
        .back-link {
            margin-bottom: 15px;
        }
        .back-link a {
            color: #4a7c4e;
            text-decoration: none;
            font-size: 0.95em;
        }
        .back-link a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <article>
        <nav class="back-link"><a href="index.html">&larr; Back to All Recipes</a></nav>
        <h1>[RECIPE NAME]</h1>
        
        <p>[DESCRIPTION]</p>
        
        <div class="tags">
            <span class="superfood-badge">Superfood</span>
            <!-- Add relevant tags -->
        </div>
        
        <div class="meta">
            <span><strong>Prep:</strong> [X] min</span>
            <span><strong>Cook:</strong> [X] min</span>
            <span><strong>Total:</strong> [X] min</span>
            <span><strong>Servings:</strong> [X]</span>
        </div>
        
        <div class="nutrition">
            <h3 style="margin-top: 0;">Nutrition Per Serving</h3>
            <div class="nutrition-grid">
                <div class="nutrition-item">
                    <div class="nutrition-value">[X]</div>
                    <div class="nutrition-label">Calories</div>
                </div>
                <div class="nutrition-item">
                    <div class="nutrition-value">[X]g</div>
                    <div class="nutrition-label">Protein</div>
                </div>
                <div class="nutrition-item">
                    <div class="nutrition-value">[X]g</div>
                    <div class="nutrition-label">Fat</div>
                </div>
                <div class="nutrition-item">
                    <div class="nutrition-value">[X]g</div>
                    <div class="nutrition-label">Carbs</div>
                </div>
                <div class="nutrition-item">
                    <div class="nutrition-value">[X]g</div>
                    <div class="nutrition-label">Fiber</div>
                </div>
            </div>
        </div>
        
        <div class="ingredients-section">
            <h2>Ingredients</h2>
            
            <div class="ingredient-group">
                <h3>[Component Name]</h3>
                <ul>
                    <li>[Ingredient]</li>
                </ul>
            </div>
        </div>
        
        <h2>Instructions</h2>
        
        <div class="instructions">
            <div class="section-header">[Section Name]</div>
            <ol>
                <li>[Step]</li>
            </ol>
        </div>
        
        <div class="notes">
            <h3>📝 Notes</h3>
            <p>[Meal prep tips, variations, scaling info, superfood benefits]</p>
        </div>
    </article>
</body>
</html>
```

---

## Generation Checklist

When generating an HTML recipe file, verify:

- [ ] Recipe name matches in `<title>`, JSON-LD `name`, and `<h1>`
- [ ] All timing values use ISO 8601 format (PT#H#M)
- [ ] Ingredients are formatted quantity-first
- [ ] All ingredients from the recipe are included in the JSON-LD array
- [ ] Instructions are organized into logical sections
- [ ] Nutrition information matches calculated values
- [ ] `datePublished` is set to current date
- [ ] Keywords include "superfood" plus relevant tags
- [ ] Notes section includes meal prep tips and calorie target guidance
- [ ] Back-to-index navigation link is present (`<nav class="back-link">` after `<article>`)

---

## Filename Convention

Use this format for recipe HTML files:
```
[Recipe_Name_With_Underscores].html
```

Examples:
- `Crispy_Superfood_Chicken_Strips.html`
- `Mediterranean_Salmon_Bowl.html`
- `Southwest_Black_Bean_Sweet_Potato.html`

---

## Testing the Import

After generating the HTML file:

1. Open the file in a web browser
2. Verify the page renders correctly with all sections visible
3. Click the AnyList browser extension
4. Confirm AnyList captures:
   - Recipe name
   - All ingredients
   - All instructions
   - Timing information
   - Nutrition data
   - Servings/yield

If any fields don't import correctly, check the JSON-LD syntax and field names against the schema.org Recipe specification.

---

## Updating the Recipe Book

After generating an HTML file for AnyList import, also add the recipe to `Superfood_Recipe_Book.md` using the markdown template format. This keeps a central reference of all developed recipes in the project.

---

*Document Version: 1.0*
*Last Updated: December 2024*
