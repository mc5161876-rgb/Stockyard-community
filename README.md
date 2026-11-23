# Stockyard community library

Themes, indicators, indicator stacks, workspaces, and plugins that people made for [Stockyard](https://github.com/mc5161876-rgb/Stockyard-releases). Everything here is free.

## Browse and install

1. Open Stockyard.
2. Click **Community** at the top of the window.
3. Pick an item and click **Install**.

Stockyard downloads the item from this repository. It checks the file against the checksum in `index.json` and refuses the file if the two differ. When a newer version is listed, the item's button changes to **Update**.

## Share something you made

1. In Stockyard, open **Community** and switch to **Share**.
2. Click **Share** next to the theme, indicator, stack, workspace, or plugin you want to send.
3. Stockyard copies the item to your clipboard and opens the **Submit to the community** form in your browser.
4. Check the form. If the **Content** box is empty, paste your clipboard into it.
5. Submit the issue. You need a free GitHub account.

You can also open the form from the **Issues** tab of this repository and paste the content yourself.

## How items stay safe

A maintainer reviews every submission before it is listed. Each kind of item has a trust level, and Stockyard shows it on the item's card.

| Kind | Trust | What it can do |
| --- | --- | --- |
| Theme | Data only | Sets colors. Stockyard cleans the values before it uses them. |
| Stack | Data only | Lists indicators and their settings. |
| Workspace | Data only | Sets the chart layout, symbols, and indicators. |
| Indicator | Sandboxed | Runs in a separate sandbox that sees only the chart's candles. It has no network, settings, or file access. |
| Plugin | Full access | Runs inside Stockyard with the same access as the app. Stockyard asks before it installs a plugin and shows the code first. |

## This repository is generated

The library's source lives in the Stockyard source repository, in `community/items/`. A workflow builds it and pushes the result here. Pull requests to this repository are overwritten, so submit items through the issue form.

## Add an item (maintainers)

1. Create two files in `community/items/<kind>/`, where `<kind>` is `theme`, `indicator`, `stack`, `workspace`, or `plugin`:
   - `<id>.json` is the listing.
   - `<id>.<kind>.<ext>` is the file that Stockyard installs.
2. Paste the submitted content into the second file.
3. Run `node scripts/build-community.mjs --check`.
4. To try the item in the app, run `node scripts/build-community.mjs --serve`. Then run `VITE_COMMUNITY_BASE=http://localhost:1421/ npm run dev` in another terminal.
5. Merge to `main`. The **Community library** workflow publishes the change here.

To update an item, change its file and raise `version` in its listing. To remove an item, delete both files.

### Item files

| Kind | Installed file | Content |
| --- | --- | --- |
| `theme` | `<id>.theme.json` | Theme JSON, as **Share** makes it |
| `indicator` | `<id>.indicator.js` | An indicator file that calls `indicator({ ... })` |
| `stack` | `<id>.stack.json` | `{ "name", "description", "indicators": [...] }` |
| `workspace` | `<id>.workspace.json` | The text that **Copy to share** makes in the **Workspaces** menu |
| `plugin` | `<id>.plugin.js` | A plugin module. See `docs/plugins.md` in the source repository. |

The listing has these fields:

| Field | Required | Meaning |
| --- | --- | --- |
| `id` | yes | Lowercase words joined by dashes. It must match the file name and be unique across the library. |
| `kind` | yes | The folder the item is in. |
| `name` | yes | The name shown on the card. |
| `description` | yes | One or two sentences about what the item does. |
| `author` | yes | Who made the item. |
| `version` | yes | Like `1.0.0`. Raise it to offer an update. |
| `tags` | yes | Words that search matches. The list can be empty. |
| `minAppVersion` | no | The oldest Stockyard version that can use the item, like `0.6.0`. |

The build adds `file`, `sha256`, and `updatedAt` to each listing in `index.json`.
