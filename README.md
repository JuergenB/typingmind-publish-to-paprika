# Paprika Recipes — a TypingMind plugin

**Version 12.** The plugin's title carries the version too, so you can see it in
the plugin list without opening anything. TypingMind shows no version of its own
and freezes this page at install time, so whatever number these name is the
version you actually have installed.

Gives a chef agent hands in [Paprika Recipe Manager 3](https://www.paprikaapp.com/).
It can look through the recipes you already have, read one, revise it after
you have cooked it, publish a new one with a photo you choose, put your own
photographs onto recipes you have cooked, read and add to your shopping list,
and keep a list of your kitchen equipment - from any device, including a phone
with nothing installed.

## What it can do

| Function | What it does |
|---|---|
| `find_recipes` | Searches the recipes you already have - across names, ingredients, directions, notes and source - and filters by rating, favourite, category or source. A search of several words that matches nothing exactly comes back with near misses rather than nothing |
| `get_recipe` | Reads one recipe in full, by uid |
| `update_recipe` | Revises a recipe you already have. **Every revision keeps the version it replaces**, so a bad one can be put back |
| `preview_recipe_images` | Draws several food-photography options and returns their URLs, so you see them and pick one *before* the recipe is published |
| `publish_recipe` | Writes a new recipe into Paprika, with the photo you chose, and tells your devices to sync immediately |
| `get_groceries` | Reads your shopping list, grouped by aisle, which is the order you walk a shop in |
| `add_groceries` | Adds items to the shopping list. Anything already waiting to be bought is refused and named, so the list does not fill with near-duplicates |
| `check_off_groceries` | Ticks items off as bought |
| `attach_photo` | Puts a photograph you attach to the chat onto a recipe you already have. **Replaces the picture it has now - there is no way back to the old one** |
| `find_equipment` | Looks through your kitchen equipment list - knives, pans, appliances, boards, utensils - by category or by words, including the wish list and things you no longer have |
| `get_equipment` | Reads one piece of equipment in full: notes, cautions, anything not yet confirmed, and its photo |
| `add_equipment` | Adds a piece of equipment, with the photo you attach to the message |
| `update_equipment` | Corrects or adds to one: a note, a caution, the right size, a new photo, or marking it gone. **Every change keeps the version before it** |

It reads, adds and revises recipes, puts your own photographs on them, reads
and adds to the shopping list, and keeps the kitchen equipment list.
It does not delete anything, and it does not touch meal plans or the pantry.

Items added to the shopping list are filed into an aisle by **the plugin's
guess**, because Paprika does not assign one itself. Something it cannot
place is left unsorted rather than filed wrongly, and any guess is easy to
move in the app.

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

`attach_photo`, `add_equipment` and `update_equipment` are the exceptions, and they cannot be otherwise - see below.

## Putting your own photograph on a recipe

Attach the picture to the message that asks for it, and say which recipe:

> *"put this on the pork belly recipe"*

Then the plugin finds the recipe, reads the attached image, shrinks it in your
browser and uploads it. **The picture never passes through the conversation**,
so this works even with a model that cannot see images at all - the model only
ever handles the recipe's id.

Two things follow from how TypingMind works, and both are worth knowing:

- **The photo must be attached to the message making the request.** TypingMind
  hands a plugin the attachments on the current message only. One sent earlier
  in the conversation is out of reach, so send it again with the request.
- **These functions run in your browser rather than on the server**, because
  TypingMind gives attachments to JavaScript functions and to nothing else. They
  therefore need the service token in the browser to upload. Every other
  function still runs server-side with the token hidden.

An iPhone photo in HEIC is fine. The browser re-encodes it to JPEG on the way
through.

## Your kitchen equipment

Tell the chef about something in your kitchen and attach a photo to the same
message:

> *"add our Breville coffee machine"*

It goes on the list straight away, and the chef knows about it from your next
new conversation. Tell it when something is wrong or missing - *"the wok handle
gets hot"*, *"that pan is 10 inches, not 8"* - and it adds your words to that
item rather than rewriting what is there. Something you no longer have is
marked gone, never deleted, and every change keeps the version before it.

The equipment list is not stored in Paprika, which has nowhere to keep it. It
lives with the service. A photo has to be attached to the message that asks for
it, for the same reason as below.

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
