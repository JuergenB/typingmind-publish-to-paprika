# Publish to Paprika — a TypingMind plugin

Publishes a finished recipe straight into [Paprika Recipe Manager 3](https://www.paprikaapp.com/),
photo included, from any device.

Two functions:

| Function | What it does |
|---|---|
| `preview_recipe_images` | Draws several food-photography options and returns their URLs, so you can see them and pick one *before* the recipe is published |
| `publish_recipe` | Writes the recipe into Paprika, with the photo you chose, and tells your devices to sync immediately |

## Installing

TypingMind → **Plugins** → **Import** → paste this repository's URL:

```
https://github.com/JuergenB/typingmind-publish-to-paprika
```

Then open the plugin's settings and fill in:

- **Service URL** — the base URL of your `paprika-aware-chef` deployment, no trailing slash
- **Service Token** — the `SERVICE_TOKEN` from that deployment's environment

Leave the plugin running **server-side**, which is the default. That is what lets
it work from a phone with nothing installed, and what keeps the token off your
devices.

## What it needs

A running [paprika-aware-chef](https://github.com/JuergenB/paprika-aware-chef)
service. This repository is only the plugin definition — the definition on its
own does nothing.

## Why the ingredient rules are so specific

The parameter descriptions carry Paprika's own formatting rules: quantity first
on every line, no bullet characters, ingredients grouped by cooking stage under
headings that end in a colon. Those live in the schema rather than in an agent's
system prompt on purpose — the schema is what the model reads at the moment it
writes the call, which is where formatting rules actually get followed.

## Security

This repository holds no secrets. `serviceUrl` and `serviceToken` are empty
settings you fill in yourself, stored server-side by TypingMind and never shown
in chat. Every route on the service requires the bearer token, so the URL on its
own grants nothing.
