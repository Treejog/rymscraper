# rymscraper

[![Build Status](https://travis-ci.com/dbeley/rymscraper.svg?branch=master)](https://travis-ci.com/dbeley/rymscraper)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/8601652424ab44698fd00f6a46a2140e)](https://www.codacy.com/app/dbeley/rymscraper?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=dbeley/rymscraper&amp;utm_campaign=Badge_Grade)

Python API to extract data from rateyourmusic.com.

Warning : Be aware that an excessive usage of this can make your IP address banned by rateyourmusic for a few days.

## Requirements

- beautifulsoup4
- lxml
- requests
- pandas
- selenium
- tqdm

## Installation

Installation in a virtualenv with pipenv (recommended)

```
pipenv install '-e .'
```

Or you can simply install the package with

```
python setup.py install
```

## Example

The data format used by the library is the python dict. It can be easily converted to CSV or JSON.

```
import rymscraper
import pandas as pd
from rymscraper import RymUrl

network = rymscraper.RymNetwork()

# artist
artist_infos = network.get_artist_infos(name="Daft Punk")
# you can easily convert all returned values to a pandas dataframe
df = pd.DataFrame([artist_infos])

# several artists
list_artists_infos = network.get_artists_infos(names=["Air", "Bonobo", "Aphex Twin", "M83"])
df = pd.DataFrame(list_artists_infos)

# chart (slow for very long charts)
rym_url = RymUrl.RymUrl()
chart_infos = network.get_chart_infos(rym_url=rym_url, max_page=3)
df = pd.DataFrame(chart_infos)

# discography
discography_infos = network.get_discography_infos(name="Aufgang", complementary_infos=True)
df = pd.DataFrame.from_records(discography_infos)

# album
# name field should use the format Artist - Album name (not ideal but it works for now)
album_infos = network.get_album_infos(name="XTC - Black Sea")
df = pd.DataFrame([album_infos])

# several albums
list_album_infos = network.get_albums_infos(names=["Ride - Nowhere", "Electrelane - Axes", "Stereolab - Dots and Loops", "Blur - The Great Escape"])
df = pd.DataFrame(list_album_infos)

# don't forget to close and quit the browser (prevent memory leaks)
network.browser.close()
network.browser.quit()
```

## Example Scripts

Some scripts are included in the examples folder.

- get_artist_infos.py : extract informations about one or several artists by name or url in a csv file.
- get_chart.py : extract albums information appearing in a chart by name, year or url in a csv file.
- get_discography.py : extract the discography of one or several artists by name or url in a csv file.
- get_album_infos.py : extract informations about one or several albums by name or url in a csv file.

### Usage

```
python get_artist_infos.py -a "u2,xtc,brad mehldau"
python get_artist_infos.py --file_artist artist_list.txt

python get_chart.py -g rock

python get_discography.py -a magma

python get_album_infos.py -a "ride - nowhere"
```

### Help

```
python get_artist_infos.py -h
```

```
usage: get_artist_infos.py [-h] [--debug] [-u URL] [--file_url FILE_URL]
                           [--file_artist FILE_ARTIST] [-a ARTIST] [-s]
                           [--no_headless]

Scraper rateyourmusic (artist version).

optional arguments:
  -h, --help            show this help message and exit
  --debug               Display debugging information
  -u URL, --url URL     URL to extract (separated by comma)
  --file_url FILE_URL   File containing the URL to extract (one by line)
  --file_artist FILE_ARTIST
                        File containing the artist to extract (one by line)
  -a ARTIST, --artist ARTIST
                        Artist to extract (separated by comma)
  -s, --separate_export
                        Also export the artists in separate files
  --no_headless         Launch selenium in foreground (background by default)
```

```
python get_chart.py -h
```

```
usage: get_chart.py [-h] [--debug] [-u URL] [-g GENRE] [-y YEAR] [-c COUNTRY]
                    [-e] [--no_headless]

Scraper rateyourmusic (chart version).

optional arguments:
  -h, --help            show this help message and exit
  --debug               Display debugging information
  -u URL, --url URL     Chart URL to parse
  -g GENRE, --genre GENRE
                        Genre (use + if you need a space)
  -y YEAR, --year YEAR  Year
  -c COUNTRY, --country COUNTRY
                        Country
  -e, --everything      Everything (otherwise only albums)
  --no_headless         Launch selenium in foreground (background by default)
```

```
python get_discography.py -h
```

```
usage: get_discography.py [-h] [--debug] [-u URL] [--file_url FILE_URL]
                          [--file_artist FILE_ARTIST] [-a ARTIST] [-s] [-c]
                          [--no_headless]

Scraper rateyourmusic (discography version).

optional arguments:
  -h, --help            show this help message and exit
  --debug               Display debugging information
  -u URL, --url URL     URL to extract (separated by comma)
  --file_url FILE_URL   File containing the URL to extract (one by line)
  --file_artist FILE_ARTIST
                        File containing the artist to extract (one by line)
  -a ARTIST, --artist ARTIST
                        Artist to extract (separated by comma)
  -s, --separate_export
                        Also export the artists in separate files
  -c, --complementary_infos
                        Extract complementary_infos for each releases (slower,
                        more requests on rym)
  --no_headless         Launch selenium in foreground (background by default)
```

```
python get_album_infos.py -h
```

```
usage: get_album_infos.py [-h] [--debug] [-u URL] [--file_url FILE_URL]
                          [--file_album_name FILE_ALBUM_NAME] [-a ALBUM_NAME]
                          [-s] [--no_headless]

Scraper rateyourmusic (album version).

optional arguments:
  -h, --help            show this help message and exit
  --debug               Display debugging information
  -u URL, --url URL     URL to extract (separated by comma)
  --file_url FILE_URL   File containing the URL to extract (one by line)
  --file_album_name FILE_ALBUM_NAME
                        File containing the name of the albums to extract (one
                        by line, format Artist - Album)
  -a ALBUM_NAME, --album_name ALBUM_NAME
                        Album to extract (separated by comma, format Artist -
                        Album)
  -s, --separate_export
                        Also export the artists in separate files
  --no_headless         Launch selenium in foreground (background by default)
```
