# Paprika Recipes — a TypingMind plugin

Gives a chef agent hands in [Paprika Recipe Manager 3](https://www.paprikaapp.com/).
It can look through the recipes you already have, read one, revise it after
you have cooked it, and publish a new one with a photo you choose — from any
device, including a phone with nothing installed.

## What it can do

| Function | What it does |
|---|---|
| `find_recipes` | Searches the recipes you already have - across names, ingredients, directions, notes and source - and filters by rating, favourite, category or source |
| `get_recipe` | Reads one recipe in full, by uid |
| `update_recipe` | Revises a recipe you already have. **Overwrites — there is no undo** |
| `preview_recipe_images` | Draws several food-photography options and returns their URLs, so you see them and pick one *before* the recipe is published |
| `publish_recipe` | Writes a new recipe into Paprika, with the photo you chose, and tells your devices to sync immediately |

It reads, adds and revises recipes. It does not delete anything, and it does
not touch meal plans, the grocery list or the pantry.

## Installing

TypingMind → **Plugins** → **Import** → paste this repository's URL:

```
https://github.com/JuergenB/typingmind-publish-to-paprika
```

Then open the plugin's settings:

- **Service URL** — arrives filled in with the deployment's address
- **Service Token** — the `SERVICE_TOKEN` from that deployment's environment.
  The only thing you have to type. Re-importing creates a fresh copy with this
  empty, so it needs pasting again each time.

Leave the plugin running **server-side**, which is the default. That is what
lets it work from a phone with no local setup, and what keeps the token off
your devices.

## What it needs

A running [paprika-aware-chef](https://github.com/JuergenB/paprika-aware-chef)
service. This repository is only the plugin definition — on its own it does
nothing.

## Two things the parameter descriptions insist on, and why

**Line breaks are written as `\n`, never by pressing Enter.** TypingMind pastes
parameter text straight into a JSON body without escaping it, so one real line
break makes the request unparseable and the call dies before it is sent. The
service converts them back.

**Ingredients are grouped by cooking stage** under headings ending in a colon,
one per line, quantity first, no bullet characters. That is what Paprika's
portion scaling reads, and a leading bullet breaks its parser. Those rules live
in the schema rather than in an agent's system prompt because the schema is
what the model reads at the moment it writes the call.

## Security

This repository holds no secrets. `serviceToken` ships empty and is stored
server-side by TypingMind, never shown in chat. Every route on the service
requires the bearer token, so the URL alone grants nothing.
