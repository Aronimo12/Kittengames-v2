# Adding games to KittenGames

The game catalog is `kittengames/public/gf/games.json`. Each game also needs a
folder under `kittengames/public/gf/` containing an `index.html` entry point
and its thumbnail.

## 1. Add the game files

Create a folder with a short name using letters, numbers, and hyphens. Copy the
game's complete set of files into it, keeping the main HTML file named
`index.html`. For example:

```text
kittengames/
└── public/
    └── gf/
        ├── games.json
        └── my-new-game/
            ├── index.html
            ├── thumbnail.png
            └── (any other files the game needs)
```

The game must work when opened from that folder; keep its scripts, stylesheets,
images, and other dependencies alongside it and preserve their relative paths.
Use a small PNG or JPG image named `thumbnail.png` for the card.

## 2. Add the game to the catalog

Open `kittengames/public/gf/games.json`. Add an object to the JSON array, with
a comma after the previous object if needed. Set `path` to the exact folder
name from step 1:

```json
{
  "name": "My New Game",
  "type": "Other",
  "cloud_save": false,
  "added": "2026-11-11",
  "thumbnail": "/gf/my-new-game/thumbnail.png",
  "path": "my-new-game"
}
```

Keep all six fields. `name` is shown on the card, `type` is the category,
`cloud_save` is a boolean, and `added` uses `YYYY-MM-DD`. The `thumbnail` is a
URL from the site root; `path` is the folder under `public/gf` (not a URL and
not including `index.html`). Use a unique game name and make sure the complete
`games.json` remains valid JSON.

## 3. Make the files available to Git and deployment

Game files under `public/gf` are ignored by Git by default so large game
libraries are not accidentally committed. If this game should be included in
this repository's deployment, add these lines to `kittengames/.gitignore`,
replacing `my-new-game` with the exact folder name:

```gitignore
!public/gf/my-new-game/
!public/gf/my-new-game/**
```

The manifest is already allowed by the current ignore rules. Check that the
new files are not ignored before committing:

```sh
cd kittengames
git check-ignore -v public/gf/games.json public/gf/my-new-game/index.html public/gf/my-new-game/thumbnail.png
```

No output for the new game's files means they are not ignored. Do not commit
very large game files unless your Git host and deployment platform support
them; use an appropriately configured external host or Git LFS for large
assets.

## 4. Test the game

From the `kittengames` directory, start the development server:

```sh
npm run dev
```

Open `http://localhost:6969`, click the game card, and confirm the game loads.
You can also check the files directly:

```text
http://localhost:6969/gf/games.json
http://localhost:6969/gf/my-new-game/index.html
```

Both URLs should load successfully. If the card is missing, check the JSON
syntax and the `thumbnail` path. If the card opens a not-found page, check
that `path` exactly matches the game folder and that its `index.html` exists.
After changing the catalog, reload the page. The app caches the fetched game
list in memory for up to five minutes.

## Self-hosting

This repository contains the KittenGames frontend. Install dependencies, build
and run it from `kittengames`; the included `Caddyfile` can be used as the
reverse proxy configuration. The production server uses Next.js's default
port unless configured otherwise.
