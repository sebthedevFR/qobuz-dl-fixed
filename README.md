# qobuz-dl
> **Patched fork (0.9.9.10.post1).** Qobuz stopped accepting email and password logins from this tool, so sign-in now uses a `user_auth_token` copied from the browser. See [Token sign-in](#token-sign-in-patched-fork-windows).
Search, explore and download Lossless and Hi-Res music from [Qobuz](https://www.qobuz.com/). It *just works*™ (2025).
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=VZWSWVGZGJRMU&source=url)

## Features

* Download FLAC and MP3 files from Qobuz
* Explore and download music directly from your terminal with **interactive** or **lucky** mode
* Download albums, tracks, artists, playlists and labels with **download** mode
* Download music from last.fm playlists (Spotify, Apple Music and Youtube playlists are also supported through this method)
* Queue support on **interactive** mode
* Effective duplicate handling with own portable database
* Support for albums with multiple discs
* Support for M3U playlists
* Downloads URLs from text file
* Extended tags
* And more

## Getting started

> You'll need an **active subscription**. Do **not** `pip install qobuz-dl` from PyPI — that's the old version that can no longer log in. Install this fork from source.

### Install (Windows)

Clone the repo, then install it into a virtual environment:

```powershell
git clone https://github.com/sebthedevFR/qobuz-dl-fixed.git
cd qobuz-dl-fixed
py -m venv .venv
.venv\Scripts\python -m pip install windows-curses -e .
```

Run it with `.venv\Scripts\qobuz-dl` (or activate the venv with `.venv\Scripts\Activate.ps1`, then just `qobuz-dl`).

### Install (Linux / macOS)

```bash
git clone https://github.com/sebthedevFR/qobuz-dl-fixed.git
cd qobuz-dl-fixed
python3 -m venv .venv
.venv/bin/python -m pip install -e .
```

### Sign in

This fork uses a browser token instead of email/password — see [Token sign-in](#token-sign-in-patched-fork-windows) below. `qobuz-dl -r` still works for the other settings, but don't use it for the token (it hashes what you type).

## Examples

### Download mode
Download URL in 24B<96khz quality
```
qobuz-dl dl https://play.qobuz.com/album/qxjbxh1dc3xyb -q 7
```
Download multiple URLs to custom directory
```
qobuz-dl dl https://play.qobuz.com/artist/2038380 https://play.qobuz.com/album/ip8qjy1m6dakc -d "Some pop from 2020"
```
Download multiple URLs from text file
```
qobuz-dl dl this_txt_file_has_urls.txt
```
Download albums from a label and also embed cover art images into the downloaded files
```
qobuz-dl dl https://play.qobuz.com/label/7526 --embed-art
```
Download a Qobuz playlist in maximum quality
```
qobuz-dl dl https://play.qobuz.com/playlist/5388296 -q 27
```
Download all the music from an artist except singles, EPs and VA releases
```
qobuz-dl dl https://play.qobuz.com/artist/2528676 --albums-only
```

#### Last.fm playlists
> Last.fm has a new feature for creating playlists: you can create your own based on the music you listen to or you can import one from popular streaming services like Spotify, Apple Music and Youtube. Visit: `https://www.last.fm/user/<your profile>/playlists` (e.g. https://www.last.fm/user/vitiko98/playlists) to get started.

Download a last.fm playlist in the maximum quality
```
qobuz-dl dl https://www.last.fm/user/vitiko98/playlists/11887574 -q 27
```

Run `qobuz-dl dl --help` for more info.

### Interactive mode
Run interactive mode with a limit of 10 results
```
qobuz-dl fun -l 10
```
Type your search query
```
Logging...
Logged: OK
Membership: Studio


Enter your search: [Ctrl + c to quit]
- fka twigs magdalene
```
`qobuz-dl` will bring up a nice list of releases. Now choose whatever releases you want to download (everything else is interactive).

Run `qobuz-dl fun --help` for more info.

### Lucky mode
Download the first album result
```
qobuz-dl lucky playboi carti die lit
```
Download the first 5 artist results
```
qobuz-dl lucky joy division -n 5 --type artist
```
Download the first 3 track results in 320 quality
```
qobuz-dl lucky eric dolphy remastered --type track -n 3 -q 5
```
Download the first track result without cover art
```
qobuz-dl lucky jay z story of oj --type track --no-cover
```

Run `qobuz-dl lucky --help` for more info.

### Other
Reset your config file
```
qobuz-dl -r
```

By default, `qobuz-dl` will skip already downloaded items by ID with the message `This release ID ({item_id}) was already downloaded`. To avoid this check, add the flag `--no-db` at the end of a command. In extreme cases (e.g. lost collection), you can run `qobuz-dl -p` to completely reset the database.

## Usage
```
usage: qobuz-dl [-h] [-r] {fun,dl,lucky} ...

The ultimate Qobuz music downloader.
See usage examples on https://github.com/vitiko98/qobuz-dl

optional arguments:
  -h, --help      show this help message and exit
  -r, --reset     create/reset config file
  -p, --purge     purge/delete downloaded-IDs database

commands:
  run qobuz-dl <command> --help for more info
  (e.g. qobuz-dl fun --help)

  {fun,dl,lucky}
    fun           interactive mode
    dl            input mode
    lucky         lucky mode
```

## Module usage 
Using `qobuz-dl` as a module is really easy. Basically, the only thing you need is `QobuzDL` from `core`.

```python
import logging
from qobuz_dl.core import QobuzDL

logging.basicConfig(level=logging.INFO)

email = "your@email.com"
password = "your_password"

qobuz = QobuzDL()
qobuz.get_tokens() # get 'app_id' and 'secrets' attrs
qobuz.initialize_client(email, password, qobuz.app_id, qobuz.secrets)

qobuz.handle_url("https://play.qobuz.com/album/va4j3hdlwaubc")
```

Attributes, methods and parameters have been named as self-explanatory as possible.

## A note about Qo-DL
`qobuz-dl` is inspired in the discontinued Qo-DL-Reborn. This tool uses two modules from Qo-DL: `qopy` and `spoofer`, both written by Sorrow446 and DashLt.
## Disclaimer
* This tool was written for educational purposes. I will not be responsible if you use this program in bad faith. By using it, you are accepting the [Qobuz API Terms of Use](https://static.qobuz.com/apps/api/QobuzAPI-TermsofUse.pdf).
* `qobuz-dl` is not affiliated with Qobuz


## Token sign-in (patched fork, Windows)

1. Install (this repository is private, so download it rather than installing by URL): on GitHub click **Code > Download ZIP**, then run `py -m pip install --force-reinstall "$env:USERPROFILE\Downloads\qobuz-dl-fixed-master.zip"`
2. Sign in at play.qobuz.com and play a track. Press F12, open the Network tab, filter by `user/login`, click the request, open Response and copy `user_auth_token`.
3. Put the token in the `password` line of `%APPDATA%\qobuz-dl\config.ini` with PowerShell. Do **not** use `qobuz-dl -r` for this, because it hashes whatever you type.
   `(Get-Content "$env:APPDATA\qobuz-dl\config.ini") -replace '^password = .*', 'password = PASTE_TOKEN_HERE' | Set-Content "$env:APPDATA\qobuz-dl\config.ini"`
4. Each run refreshes the token and saves it back. If it expires, repeat steps 2 and 3.
