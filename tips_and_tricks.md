# Tips and Tricks for Ubuntu 20.04

## Making Spotify the default audio player
To make Spotify the default music player, edit the file (or create new if it doesn't exist) `~/.local/share/applications/mimeapps.list` so that it contains these lines:

```
[Default Applications]
audio/x-vorbis+ogg=spotify_spotify.desktop
[Added Associations]
audio/x-vorbis+ogg=spotify_spotify.desktop;
```
Notice that under `[Added Associations]` each line must end with a semi colon `;`.

When dowloading with snap, the Spotify desktop file is found in folder `/var/lib/snapd/desktop/applications` and is called `spotify_spotify.desktop`.

The explanation for how to make Spotify the default music player is found [here](https://askubuntu.com/questions/95345/how-to-make-spotify-the-default-music-player).
The answer to where the Spotify desktop file is located when installing with snap is found [here](https://askubuntu.com/questions/1043345/media-shortcuts-no-longer-work-after-installing-a-different-version-of-spotify-a).
