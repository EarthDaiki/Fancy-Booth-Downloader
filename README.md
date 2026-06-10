# Fancybooths Media Downloader

Downloader for images, videos, and GIFs from event galleries hosted at
`https://photos.fancybooths.com/`.

The project uses SmugMug/Fancybooths API endpoints for most operations and
Beautiful Soup only where page data has to be extracted from gallery HTML.

## Features

- Download media from public Fancybooths event pages.
- Unlock password-protected event pages and download their albums.
- Download all albums or only selected albums by number.
- Save each album into its own folder under a chosen root directory.
- Download media concurrently with retry handling.

## Requirements

- Python 3.9+
- `requests`
- `beautifulsoup4`

Install dependencies:

```bash
pip install requests beautifulsoup4
```

## Project Files

- `public_main.py` - entry point for public event pages.
- `private_main.py` - entry point for password-protected event pages.
- `api.py` - API calls for folder, album, token, and gallery metadata.
- `medias_getter.py` - collects media URLs and downloads files.
- `unlock.py` - unlocks protected galleries using a password or cookies.
- `session_manager.py` - shared `requests.Session` setup.
- `example/example_public.py` - example for public galleries.
- `example/example_private.py` - example for protected galleries.

## Usage

### Public Event Page

Edit `example/example_public.py`:

```python
from public_main import MainPublicDL

app = MainPublicDL()
url = "https://photos.fancybooths.com/Shoreline-Prom-2026"
root_dir = r"D:\fancybooths"
folder_number_list = [1]

app.download(url, root_dir, folder_number_list)
```

Run it:

```bash
python example/example_public.py
```

### Password-Protected Event Page

Edit `example/example_private.py`:

```python
from private_main import MainPrivateDL

app = MainPrivateDL()
password = "your-gallery-password"
url = "https://photos.fancybooths.com/Issaquah-High-School-Prom-2024"
root_dir = r"D:\fancybooths"
folder_number_list = [1, 3]

app.download(password, url, root_dir, folder_number_list)
```

Run it:

```bash
python example/example_private.py
```

## Download Selection

`folder_number_list` controls which albums are downloaded.

```python
folder_number_list = None      # download all albums
folder_number_list = [1]       # download only the first album
folder_number_list = [1, 3]    # download the first and third albums
```

Album numbers are 1-based and match the order returned by the event page.

## Output

Downloaded files are saved under `root_dir`. The downloader creates album
subfolders automatically based on the album URL path.

Example:

```text
D:\fancybooths\
  Shoreline-Prom-2026\
    Album-Name\
      image1.jpg
      video1.mp4
```

## Notes

- This is an unofficial downloader for Fancybooths/SmugMug-hosted galleries.
- Only download galleries and media that you are allowed to access.
- The API key in `api.py` is used as a public API key, not as a private secret.
- The downloader prints progress and a summary after downloads complete.
- Failed downloads are retried up to three times and then printed in the console.
