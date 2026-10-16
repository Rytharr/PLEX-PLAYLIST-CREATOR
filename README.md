# PLEX PLAYLIST CREATOR

A Python script that creates and updates Plex playlists from keyword searches across your media library. It searches movie titles and descriptions, TV series, and individual episodes. It was made for holiday playlists (a Halloween playlist of everything that mentions Halloween, a Christmas one for Christmas, Thanksgiving and New Years), but works for any theme.

## Features

- **Presets**: save a search once (keywords, media type, playlist name) and re-run it every year with one command
- **Updates in place**: re-running a search adds new matches and removes ones that no longer match, keeping the playlist's poster and order
- **Movies, TV, or both** in the same playlist
- **TV search at two levels**: a show whose title/description matches goes in whole; otherwise only the matching episodes go in
- **Whole-word matching**: `elf` finds *Elf* but not *herself*; plurals and apostrophes are handled (`new years` finds *New Year's Eve*); put `*` at either end to match part of a word (`*christmas*` finds *Christmastime* and *Ludachristmas*)
- **Exclude keywords**: e.g. find `halloween` but skip anything mentioning `"doctor who"`
- **Preview / dry run** before anything changes
- Server URL and token are asked for once and remembered

## Setup

Requires Python 3.8+ and a Plex Media Server.

```bash
python -m venv .venv
.venv/bin/pip install -r requirements.txt
```

Optionally put the launcher on your PATH so you can run `plex-playlist` from anywhere:

```bash
ln -s "$PWD/plex-playlist" ~/.local/bin/plex-playlist
```

The first run asks for your Plex server URL and token and saves them to `~/.config/plex-playlist-creator/config.json` (readable only by you). Run with `--setup` to change them. `PLEX_URL` / `PLEX_TOKEN` environment variables override the saved values.

### Getting your Plex token

In Plex Web, open any movie or episode → **⋯** → **Get Info** → **View XML**. The URL of the page that opens ends in `X-Plex-Token=...`; that value is your token. See also [Plex's guide](https://support.plex.tv/articles/204059436-finding-an-authentication-token-x-plex-token/).

## Usage

### Interactive menu

```bash
plex-playlist
```

```
Plex Playlist Creator
  1) Halloween Episodes   tv     *halloween*
  2) Halloween Movies     movies *halloween*
  3) Christmas Episodes   tv     *christmas*, *thanksgiving*, "new years", "happy new year"
  4) Christmas Movies     movies *christmas*, *thanksgiving*, "new years", "happy new year"
  a) Update all presets
  c) Custom search
  q) Quit
```

A custom search asks for keywords, exclusions, media type and playlist name, shows what it found, and offers to save it as a preset.

### Yearly holiday refresh

```bash
plex-playlist --all-presets            # preview each change and confirm
plex-playlist --all-presets --dry-run  # just show what would change
plex-playlist --preset halloween -y    # one preset, no questions
```

### One-off searches

```bash
plex-playlist -k 'zombie, "living dead"' -t movies -n "Zombie Night"
plex-playlist -k halloween -x '"doctor who"' -n Halloween
```

### All options

| Option | Meaning |
| --- | --- |
| `-p NAME`, `--preset NAME` | Run a saved preset (repeatable) |
| `--all-presets` | Run every saved preset |
| `--list-presets` | Show saved presets |
| `-k`, `--keywords` | One-off search keywords |
| `-x`, `--exclude` | Keywords that disqualify a match |
| `-t`, `--type` | `movies`, `tv` or `both` (default `both`) |
| `-n`, `--name` | Playlist name for `--keywords` (default: first keyword) |
| `-y`, `--yes` | Don't ask for confirmation |
| `--add-only` | When updating, don't remove items that no longer match (keeps things you added by hand) |
| `--dry-run` | Show what would change, change nothing |
| `--setup` | Re-enter server URL and token |

## Keywords

Comma-separated; use quotes for phrases: `action, "New Years", comedy`

- Matching is case-insensitive and whole-word, with an optional plural `s`
- Apostrophes are ignored, so `new years` matches *New Year's*
- A `*` at either end matches part of a word: `vampir*` finds *vampires*, `*christmas*` finds *Ludachristmas*
- Excluded keywords are checked against the item's title and description (and, for episodes, the show's title)

## Presets

Presets live in `presets.json` next to the script. Edit it directly or save one from the custom-search menu. The included presets:

| Preset | Playlist | Searches | Keywords |
| --- | --- | --- | --- |
| `halloween` | Halloween Episodes | TV | `*halloween*` |
| `halloween-movies` | Halloween Movies | Movies | `*halloween*` |
| `christmas` | Christmas Episodes | TV | `*christmas*`, `*thanksgiving*`, `new years`, `happy new year` (excludes Guilty Crown) |
| `christmas-movies` | Christmas Movies | Movies | `*christmas*`, `*thanksgiving*`, `new years`, `happy new year` |

Each entry looks like this:

```json
{
  "christmas": {
    "name": "Christmas Episodes",
    "type": "tv",
    "keywords": ["*christmas*", "*thanksgiving*", "new years", "happy new year"],
    "exclude": ["guilty crown"]
  }
}
```

`name` is the Plex playlist name. Running a preset whose playlist already exists updates that playlist. Change `name` to match your own playlists so they get updated rather than duplicated.

## Troubleshooting

- **Could not connect**: check the URL (include `http://` and port `32400`) and token, then run with `--setup`
- **A show floods the playlist**: its description matched, so every episode was added. Add the show's name to `exclude`; its individual episodes are excluded too
- **One wrong episode**: exclude a phrase from its title (e.g. `career day`) rather than the show's name, so the show's other matching episodes stay
- **Details**: `plex_playlist_debug.log` (rewritten each run) lists every match and which keyword caused it

## License

See LICENSE file for details.

## Contributing

Feel free to submit issues, feature requests, or pull requests to improve this tool.
