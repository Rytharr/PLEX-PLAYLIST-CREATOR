# PLEX PLAYLIST CREATOR

A Python script that automatically creates Plex playlists based on keyword searches across your media library. Search through movie titles, descriptions, TV series, and individual episodes to build custom playlists.bThis was mainly made for creating a Halloween playlist consisting of shows that have Halloween in the discription and a Christmas list show shows that contain Christmas, Thanksgiving,"New Years"

## Features

### 🎬 **Multi-Media Support**
- **Movies**: Search movie titles and descriptions
- **TV Shows**: Search series titles, descriptions, AND individual episodes
- **Mixed Playlists**: Combine series and individual episodes in the same playlist

### 🔍 **Advanced Search Capabilities**
- **Individual Keywords**: Search for single words like `action`, `comedy`, `thriller`
- **Multi-word Phrases**: Use quotes for exact phrases like `"New Years"`, `"Star Wars"`
- **Mixed Search**: Combine both approaches: `action, "New Years", comedy, "Star Trek"`
- **Episode-Level Search**: Find TV episodes by episode titles and descriptions

### 📊 **Comprehensive Logging**
- **Debug Logging**: Detailed logs saved to `plex_playlist_debug.log`
- **Real-time Feedback**: See search progress and results as they happen
- **Error Handling**: Graceful handling of connection and search errors

### 🎯 **Smart Playlist Management**
- **Duplicate Prevention**: Automatically prevents duplicate entries
- **Overwrite Protection**: Prompts before overwriting existing playlists
- **Auto-naming**: Generates playlist names based on keywords if not specified

## Installation

### Prerequisites
- Python 3.7 or higher
- Plex Media Server with accessible libraries
- Plex authentication token

### Setup
1. Clone or download this repository
2. Install required dependencies:
   ```bash
   pip install PlexAPI
   ```

## Usage

### Basic Usage
1. Run the script:
   ```bash
   python "Plex-playlist creator"
   ```

2. Follow the interactive prompts:
   - Enter your Plex server URL (e.g., `http://localhost:32400`)
   - Enter your Plex authentication token
   - Choose media type (`movies` or `tv`)
   - Enter search keywords
   - Specify playlist name
   - Confirm playlist creation

### Keyword Examples

#### Single Words
```
action, comedy, thriller
```
Finds content containing any of these individual words.

#### Exact Phrases
```
"New Years", "Star Wars", "Game of Thrones"
```
Finds content containing these exact phrases.

#### Mixed Format
```
action, "New Years", comedy, "Star Trek"
```
Combines individual words and exact phrases in one search.

### TV Show Search Behavior

When searching TV shows, the script will:

1. **Series-Level Matches**: If keywords match the series title or description, the entire series is added to the playlist
2. **Episode-Level Matches**: If keywords match individual episode titles or descriptions, only those specific episodes are added
3. **Combined Results**: You can have both full series and individual episodes in the same playlist

#### Example:
- Search for `"New Years"` in TV shows
- If "Friends" series description mentions "New Years" → entire "Friends" series added
- If only "The One with the Blackout" episode mentions "New Years" → only that episode added

## Getting Your Plex Token

To use this script, you'll need a Plex authentication token:

1. **Web Method**: 
   - Log into Plex Web App
   - Open browser developer tools (F12)
   - Go to Network tab, refresh page
   - Look for requests containing `X-Plex-Token` in headers

2. **Settings Method**:
   - Go to Plex Web App → Settings → Account
   - Look for "Authorized Devices" or similar section
   - Token may be visible in URL or device listings

3. **Third-party Tools**: Use tools like Plex Token Generator websites (use at your own discretion)

## Configuration

The script uses interactive prompts, but you can modify the source code for automation:

- **Server URL**: Modify the `PLEX_URL` input prompt
- **Token**: Modify the `PLEX_TOKEN` input prompt
- **Log Level**: Change `logging.DEBUG` to `logging.INFO` for less verbose logging

## Output Files

- **`plex_playlist_debug.log`**: Detailed debug log with timestamps
- **Console Output**: Real-time progress and results

## Troubleshooting

### Common Issues

1. **Import Error**: Make sure PlexAPI is installed (`pip install PlexAPI`)
2. **Connection Failed**: Verify server URL and token are correct
3. **No Libraries Found**: Check that your Plex server has movie/TV libraries
4. **Permission Denied**: Ensure your Plex token has sufficient permissions

### Debug Information

The script provides extensive logging. Check `plex_playlist_debug.log` for:
- Connection attempts and results
- Library search progress
- Match details (which keywords matched which content)
- Playlist creation steps
- Error details

### Getting Help

If you encounter issues:
1. Check the debug log file for detailed error information
2. Verify your Plex server is accessible
3. Ensure your token has the necessary permissions
4. Try searching with simpler keywords first

## Examples

### Create a Christmas Movie Playlist
```
Media type: movies
Keywords: christmas, "New Years", holiday, santa
Playlist name: Holiday Movies
```

### Create a Sci-Fi TV Playlist
```
Media type: tv
Keywords: "Star Trek", "Star Wars", space, alien, "science fiction"
Playlist name: Sci-Fi Shows
```

### Create a Comedy Collection
```
Media type: movies
Keywords: comedy, funny, "romantic comedy", laugh
Playlist name: Comedy Collection
```

## Requirements

- Python 3.7+
- PlexAPI library
- Active Plex Media Server
- Valid Plex authentication token
- Network access to Plex server

## License

See LICENSE file for details.

## Contributing

Feel free to submit issues, feature requests, or pull requests to improve this tool.
