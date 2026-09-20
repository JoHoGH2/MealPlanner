# MealPlanner


My Meal Planner — Master Specification

1. Purpose

A personal meal-planning web app designed primarily for one household, with a normal default of 2 people, but capable of handling individual meals for different numbers of people.

It should eventually be a real, standalone web app that can be hosted from GitHub Pages, rather than remaining dependent on ChatGPT.

The first version should require no backend or paid service.


---

2. Weekly meal planner

The main screen is a Monday–Sunday planner.

Each day has:

Breakfast

Lunch

Dinner


Each meal has:

Recipe selection

Eat — number of portions/people eating that meal

Cook — number of portions prepared fresh

From freezer — number of portions taken from the freezer


The default household size is 2, but each individual meal can override that.

Example:

Meal	Eat	Cook	From freezer

Monday dinner	2	6	0
Tuesday dinner	4	4	0
Wednesday dinner	2	0	2


This distinction is fundamental.

People eating ≠ portions cooked.


---

3. Recipes

The recipe book allows the user to:

Add recipes

Edit recipes

Delete recipes

Search/browse recipes later

Categorise recipes


Each recipe contains:

Recipe name

Category

Base number of portions

Ingredients

Ingredient quantities

Units

Shopping category

Instructions

Preparation time

Cooking time


For example:

Spaghetti Bolognese

Base recipe: 4 portions

500 g beef mince

1 onion

2 × 400 g cans chopped tomatoes

400 g spaghetti


The base recipe quantity is independent from how many portions are required for a particular meal.


---

4. Portion scaling

Recipes automatically scale from their base number of portions.

For example, if the recipe is written for 4:

2 portions → 0.5 × recipe

4 portions → 1 × recipe

6 portions → 1.5 × recipe


This scaling should be used when calculating ingredients for cooking.


---

5. Batch cooking

This is one of the most important features.

The app must understand that someone may eat 2 portions but cook 6.

Example:

Monday

Spaghetti Bolognese:

Eat: 2

Cook: 6

From freezer: 0


Therefore:

4 portions are left over and frozen.

The shopping list must use 6 portions, not 2, because six portions are actually being cooked.


---

6. Freezer system

The app has a dedicated Freezer section.

It tracks:

Recipe

Number of frozen portions

Date frozen


Example:

> Spaghetti Bolognese — 4 portions — frozen 20 September



When planning a later meal, the user can say:

> Eat: 2
Cook: 0
From freezer: 2



The freezer then supplies those two portions.

The shopping list should not buy ingredients for those two portions because they already exist.


---

7. Mixed fresh + freezer meals

The model should also support combinations.

For example:

> Eat: 4
Cook: 2
From freezer: 2



This means:

2 portions are freshly cooked

2 portions come from the freezer

4 people eat


The shopping list only needs ingredients for the 2 fresh portions.


---

8. Batch cooking and freezer calculation

For every meal:

Fresh surplus = Cook − Eat

when Cook is greater than Eat.

For example:

> Eat 2
Cook 6
Freezer used 0



→ 4 new freezer portions

Another example:

> Eat 2
Cook 6
Freezer used 2



→ 4 freshly cooked portions, 2 eaten fresh, so 4 fresh portions become available, while 2 freezer portions are consumed.

The net freezer change is therefore:

new frozen portions − freezer portions used

The app needs to keep this distinction clear.


---

9. Shopping list

The shopping list is generated automatically from the weekly plan.

Crucially, it is based on:

> What needs to be cooked fresh



rather than simply:

> What will be eaten.



Therefore:

Batch cook

Eat 2, Cook 6:

→ shopping ingredients for 6 portions.

Freezer meal

Eat 2, Cook 0, From freezer 2:

→ no new ingredients.

Mixed meal

Eat 4, Cook 2, From freezer 2:

→ ingredients for 2 portions.


---

10. Shopping-list behaviour

The shopping list should:

Combine duplicate ingredients

Add quantities together

Preserve units where possible

Group items by category

Allow items to be checked off

Allow manual items to be added

Allow the list to be regenerated


Suggested categories:

Produce

Meat & Fish

Dairy & Eggs

Cupboard

Frozen

Bakery

Drinks

Other


Example:

If three meals require:

2 onions

1 onion

3 onions


the list should show:

> 6 onions



rather than three separate entries.


---

11. Data storage

Initial version should be a client-side application.

Use browser storage, such as:

localStorage

This means:

No server required

No database required

No account required

No subscription required

Suitable for GitHub Pages


The app should also provide:

Export backup

Download the user's data as JSON.

Import backup

Restore a previously exported JSON file.

This is important because the data belongs to the user.


---

12. GitHub hosting

The application should be structured as a normal web project:

my-meal-planner/
│
├── index.html
├── style.css
├── app.js
└── README.md

It should work on:

GitHub Pages

The eventual flow is:

GitHub repository
       ↓
       GitHub Pages
              ↓
              My Meal Planner web app
                     ↓
                     Browser local storage

                     No backend is initially required.


                     ---

                     13. Future architecture

                     The first version should be written cleanly enough that we can later add:

                     User accounts

                     Cloud database

                     Sync between phone and computer

                     Multiple household profiles

                     Recipe search

                     Favourite recipes

                     More sophisticated ingredient/unit handling

                     Pantry inventory

                     Automatic pantry deduction

                     Leftover tracking

                     Calendar integration

                     PWA/mobile installation

                     Possibly multiple freezer locations


                     But these should not complicate the first working version.


                     ---

                     14. Important design principle

                     The underlying data model should remain:

                     Recipe
                        ↓
                        Planned Meal
                           ├── Eat
                              ├── Cook
                                 └── From Freezer
                                           ↓
                                                  Freezer
                                                            ↓
                                                                 Future Meal

                                                                 And separately:

                                                                 Planned Meals
                                                                       ↓
                                                                       Fresh portions that need cooking
                                                                             ↓
                                                                             Recipe ingredient scaling
                                                                                   ↓
                                                                                   Combined ingredients
                                                                                         ↓
                                                                                         Shopping List

                                                                                         That separation is important because it prevents the classic problem where the app assumes:

                                                                                         > "2 people are eating, therefore cook for 2."



                                                                                         Instead it understands:

                                                                                         > "2 people are eating, but I may deliberately cook 6."




                                                                                         ---

                                                                                         15. Current build target

                                                                                         When we continue, the target is:

                                                                                         My Meal Planner — Version 3

                                                                                         A proper GitHub-ready web application containing:

                                                                                         Weekly Planner + Recipes + Portion Scaling + Batch Cooking + Freezer + Shopping List + Local Storage + Import/Export

                                                                                         The immediate goal is not to add lots of extra features. The priority is getting this core model correct and usable.

                                                                                         Once that's working, we can test it with realistic scenarios such as:

                                                                                         > Monday: cook 6 portions, eat 2, freeze 4
                                                                                         Thursday: eat 2 from freezer
                                                                                         Saturday: cook 8, eat 2, freeze 6



                                                                                         and verify that both the freezer balance and shopping list remain correct.

                                                                                         That is the specification I'd use to recreate the project from scratch if necessary.Absolutely. Here is the master specification for the app so we can recreate it accurately if needed.

                                                                                         My Meal Planner — Master Specification

                                                                                         1. Purpose

                                                                                         A personal meal-planning web app designed primarily for one household, with a normal default of 2 people, but capable of handling individual meals for different numbers of people.

                                                                                         It should eventually be a real, standalone web app that can be hosted from GitHub Pages, rather than remaining dependent on ChatGPT.

                                                                                         The first version should require no backend or paid service.


                                                                                         ---

                                                                                         2. Weekly meal planner

                                                                                         The main screen is a Monday–Sunday planner.

                                                                                         Each day has:

                                                                                         Breakfast

                                                                                         Lunch

                                                                                         Dinner


                                                                                         Each meal has:

                                                                                         Recipe selection

                                                                                         Eat — number of portions/people eating that meal

                                                                                         Cook — number of portions prepared fresh

                                                                                         From freezer — number of portions taken from the freezer


                                                                                         The default household size is 2, but each individual meal can override that.

                                                                                         Example:

                                                                                         Meal	Eat	Cook	From freezer

                                                                                         Monday dinner	2	6	0
                                                                                         Tuesday dinner	4	4	0
                                                                                         Wednesday dinner	2	0	2


                                                                                         This distinction is fundamental.

                                                                                         People eating ≠ portions cooked.


                                                                                         ---

                                                                                         3. Recipes

                                                                                         The recipe book allows the user to:

                                                                                         Add recipes

                                                                                         Edit recipes

                                                                                         Delete recipes

                                                                                         Search/browse recipes later

                                                                                         Categorise recipes


                                                                                         Each recipe contains:

                                                                                         Recipe name

                                                                                         Category

                                                                                         Base number of portions

                                                                                         Ingredients

                                                                                         Ingredient quantities

                                                                                         Units

                                                                                         Shopping category

                                                                                         Instructions

                                                                                         Preparation time

                                                                                         Cooking time


                                                                                         For example:

                                                                                         Spaghetti Bolognese

                                                                                         Base recipe: 4 portions

                                                                                         500 g beef mince

                                                                                         1 onion

                                                                                         2 × 400 g cans chopped tomatoes

                                                                                         400 g spaghetti


                                                                                         The base recipe quantity is independent from how many portions are required for a particular meal.


                                                                                         ---

                                                                                         4. Portion scaling

                                                                                         Recipes automatically scale from their base number of portions.

                                                                                         For example, if the recipe is written for 4:

                                                                                         2 portions → 0.5 × recipe

                                                                                         4 portions → 1 × recipe

                                                                                         6 portions → 1.5 × recipe


                                                                                         This scaling should be used when calculating ingredients for cooking.


                                                                                         ---

                                                                                         5. Batch cooking

                                                                                         This is one of the most important features.

                                                                                         The app must understand that someone may eat 2 portions but cook 6.

                                                                                         Example:

                                                                                         Monday

                                                                                         Spaghetti Bolognese:

                                                                                         Eat: 2

                                                                                         Cook: 6

                                                                                         From freezer: 0


                                                                                         Therefore:

                                                                                         4 portions are left over and frozen.

                                                                                         The shopping list must use 6 portions, not 2, because six portions are actually being cooked.


                                                                                         ---

                                                                                         6. Freezer system

                                                                                         The app has a dedicated Freezer section.

                                                                                         It tracks:

                                                                                         Recipe

                                                                                         Number of frozen portions

                                                                                         Date frozen


                                                                                         Example:

                                                                                         > Spaghetti Bolognese — 4 portions — frozen 20 September



                                                                                         When planning a later meal, the user can say:

                                                                                         > Eat: 2
                                                                                         Cook: 0
                                                                                         From freezer: 2



                                                                                         The freezer then supplies those two portions.

                                                                                         The shopping list should not buy ingredients for those two portions because they already exist.


                                                                                         ---

                                                                                         7. Mixed fresh + freezer meals

                                                                                         The model should also support combinations.

                                                                                         For example:

                                                                                         > Eat: 4
                                                                                         Cook: 2
                                                                                         From freezer: 2



                                                                                         This means:

                                                                                         2 portions are freshly cooked

                                                                                         2 portions come from the freezer

                                                                                         4 people eat


                                                                                         The shopping list only needs ingredients for the 2 fresh portions.


                                                                                         ---

                                                                                         8. Batch cooking and freezer calculation

                                                                                         For every meal:

                                                                                         Fresh surplus = Cook − Eat

                                                                                         when Cook is greater than Eat.

                                                                                         For example:

                                                                                         > Eat 2
                                                                                         Cook 6
                                                                                         Freezer used 0



                                                                                         → 4 new freezer portions

                                                                                         Another example:

                                                                                         > Eat 2
                                                                                         Cook 6
                                                                                         Freezer used 2



                                                                                         → 4 freshly cooked portions, 2 eaten fresh, so 4 fresh portions become available, while 2 freezer portions are consumed.

                                                                                         The net freezer change is therefore:

                                                                                         new frozen portions − freezer portions used

                                                                                         The app needs to keep this distinction clear.


                                                                                         ---

                                                                                         9. Shopping list

                                                                                         The shopping list is generated automatically from the weekly plan.

                                                                                         Crucially, it is based on:

                                                                                         > What needs to be cooked fresh



                                                                                         rather than simply:

                                                                                         > What will be eaten.



                                                                                         Therefore:

                                                                                         Batch cook

                                                                                         Eat 2, Cook 6:

                                                                                         → shopping ingredients for 6 portions.

                                                                                         Freezer meal

                                                                                         Eat 2, Cook 0, From freezer 2:

                                                                                         → no new ingredients.

                                                                                         Mixed meal

                                                                                         Eat 4, Cook 2, From freezer 2:

                                                                                         → ingredients for 2 portions.


                                                                                         ---

                                                                                         10. Shopping-list behaviour

                                                                                         The shopping list should:

                                                                                         Combine duplicate ingredients

                                                                                         Add quantities together

                                                                                         Preserve units where possible

                                                                                         Group items by category

                                                                                         Allow items to be checked off

                                                                                         Allow manual items to be added

                                                                                         Allow the list to be regenerated


                                                                                         Suggested categories:

                                                                                         Produce

                                                                                         Meat & Fish

                                                                                         Dairy & Eggs

                                                                                         Cupboard

                                                                                         Frozen

                                                                                         Bakery

                                                                                         Drinks

                                                                                         Other


                                                                                         Example:

                                                                                         If three meals require:

                                                                                         2 onions

                                                                                         1 onion

                                                                                         3 onions


                                                                                         the list should show:

                                                                                         > 6 onions



                                                                                         rather than three separate entries.


                                                                                         ---

                                                                                         11. Data storage

                                                                                         Initial version should be a client-side application.

                                                                                         Use browser storage, such as:

                                                                                         localStorage

                                                                                         This means:

                                                                                         No server required

                                                                                         No database required

                                                                                         No account required

                                                                                         No subscription required

                                                                                         Suitable for GitHub Pages


                                                                                         The app should also provide:

                                                                                         Export backup

                                                                                         Download the user's data as JSON.

                                                                                         Import backup

                                                                                         Restore a previously exported JSON file.

                                                                                         This is important because the data belongs to the user.


                                                                                         ---

                                                                                         12. GitHub hosting

                                                                                         The application should be structured as a normal web project:

                                                                                         my-meal-planner/
                                                                                         │
                                                                                         ├── index.html
                                                                                         ├── style.css
                                                                                         ├── app.js
                                                                                         └── README.md

                                                                                         It should work on:

                                                                                         GitHub Pages

                                                                                         The eventual flow is:

                                                                                         GitHub repository
                                                                                                ↓
                                                                                                GitHub Pages
                                                                                                       ↓
                                                                                                       My Meal Planner web app
                                                                                                              ↓
                                                                                                              Browser local storage

                                                                                                              No backend is initially required.


                                                                                                              ---

                                                                                                              13. Future architecture

                                                                                                              The first version should be written cleanly enough that we can later add:

                                                                                                              User accounts

                                                                                                              Cloud database

                                                                                                              Sync between phone and computer

                                                                                                              Multiple household profiles

                                                                                                              Recipe search

                                                                                                              Favourite recipes

                                                                                                              More sophisticated ingredient/unit handling

                                                                                                              Pantry inventory

                                                                                                              Automatic pantry deduction

                                                                                                              Leftover tracking

                                                                                                              Calendar integration

                                                                                                              PWA/mobile installation

                                                                                                              Possibly multiple freezer locations


                                                                                                              But these should not complicate the first working version.


                                                                                                              ---

                                                                                                              14. Important design principle

                                                                                                              The underlying data model should remain:

                                                                                                              Recipe
                                                                                                                 ↓
                                                                                                                 Planned Meal
                                                                                                                    ├── Eat
                                                                                                                       ├── Cook
                                                                                                                          └── From Freezer
                                                                                                                                    ↓
                                                                                                                                           Freezer
                                                                                                                                                     ↓
                                                                                                                                                          Future Meal

                                                                                                                                                          And separately:

                                                                                                                                                          Planned Meals
                                                                                                                                                                ↓
                                                                                                                                                                Fresh portions that need cooking
                                                                                                                                                                      ↓
                                                                                                                                                                      Recipe ingredient scaling
                                                                                                                                                                            ↓
                                                                                                                                                                            Combined ingredients
                                                                                                                                                                                  ↓
                                                                                                                                                                                  Shopping List

                                                                                                                                                                                  That separation is important because it prevents the classic problem where the app assumes:

                                                                                                                                                                                  > "2 people are eating, therefore cook for 2."



                                                                                                                                                                                  Instead it understands:

                                                                                                                                                                                  > "2 people are eating, but I may deliberately cook 6."




                                                                                                                                                                                  ---

                                                                                                                                                                                  15. Current build target

                                                                                                                                                                                  When we continue, the target is:

                                                                                                                                                                                  My Meal Planner — Version 3

                                                                                                                                                                                  A proper GitHub-ready web application containing:

                                                                                                                                                                                  Weekly Planner + Recipes + Portion Scaling + Batch Cooking + Freezer + Shopping List + Local Storage + Import/Export

                                                                                                                                                                                  The immediate goal is not to add lots of extra features. The priority is getting this core model correct and usable.

                                                                                                                                                                                  Once that's working, we can test it with realistic scenarios such as:

                                                                                                                                                                                  > Monday: cook 6 portions, eat 2, freeze 4
                                                                                                                                                                                  Thursday: eat 2 from freezer
                                                                                                                                                                                  Saturday: cook 8, eat 2, freeze 6



                                                                                                                                                                                  and verify that both the freezer balance and shopping list remain correct.

                                                                                                                                                                                  That is the specification I'd use to recreate the project from scratch if necessary.