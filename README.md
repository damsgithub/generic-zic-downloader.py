# generic-zic-downloader.py


Supports myzuka.club and musify.club, maybe more later

Command line downloader with optionnal rich interface (think ncurses), features included:
* Cover downloading
* Windows (powershell or cmd prompt) and Linux support (even WSL)
* Resume incomplete songs and albums downloads (see BUGS)
* Creation of directory with "Artist - Album (year)" name (see BUGS).
* Multiple simultaneous downloads to download faster
* Able to download all albums from an artist
* Socks proxy support
* Colored output
* progress bars with [rich](https://github.com/willmcgugan/rich)

Inspired by [xor512 script](https://github.com/xor512/musicmp3spb.org)

TODO:
* streaming mode?

BUGS:
* it is more difficult to interrupt the script with ctrl-c in Windows with latests Python version, even with [this bug](https://bugs.python.org/issue42296) corrected.
* must use urllib3<2 with "requests" module if you want good Tor socks proxy support, see [this bug](https://github.com/psf/requests/issues/6443) It can be disabled, see notes below (userequests = 0)
* Only with musify.club, if you interrupt the script and then relaunch it, it will sometimes append to the mp3 file the beginning of the song instead of continuing... prefer myzuka.club until it is corrected.
* It does not support multiple artists when creating the directory, it will fall back to "Unknown", you will have to rename the dir by hand afterwards.

Install:
* install python 3 (tested with 3.6, 3.9, 3.12) if not already present on your distrib. For Windows, see [here](https://www.python.org/downloads/windows/) or install from the Windows Store.
* install required modules. Use your standard repo or the command line below for linux, for Windows do it in an administrator powershell command prompt:

```sh
python -m pip install BeautifulSoup4 Pysocks rich requests cfscrape urllib3<2
```

Notes: 
* you don't need to install "requests", "cfscrape" and "urllib3" if you set "userequests" to "0" at the start of the script. They are only usefull if you want to download through Tor socks proxy due to cloudflare more ofently detecting the script as a bot with urllib.request (even with the same useragent).
* you need rich >= 10.0.0

Usage:
* Just give it an album or artist url from myzuka.club or musify.club as argument, see below:

```
Python script to download albums from myzuka.club or musify.club, version 5.14

Exemple with myzuka.club:

------------------------------------------------------------------------------------------------------------------
# To download an album, give it an url with 'https://myzuka.club/Album/' or 'https://musify.club/release/' in it #
------------------------------------------------------------------------------------------------------------------
user@computer:/tmp$ generic-zic-downloader.py [-p /path] https://myzuka.club/Album/630746/The-6-Cello-Suites-Cd1-1994
** We will try to use 3 simultaneous downloads, progress will be shown **
** after each completed file but not necessarily in album's order. **

Artist: Johann Sebastian Bach
Album: The 6 Cello Suites (CD1)
Year: 1994
cover.jpg                                                 00.01 of 00.01 MB [100%]
05_johann_sebastian_bach_maurice_gendron_bwv1007_menuets.mp3        07.04 of 07.04 MB [100%]
01_johann_sebastian_bach_maurice_gendron_bwv1007_prelude.mp3        05.57 of 05.57 MB [100%]
03_johann_sebastian_bach_maurice_gendron_bwv1007_courante.mp3        05.92 of 05.92 MB [100%]
06_johann_sebastian_bach_maurice_gendron_bwv1007_gigue.mp3        04.68 of 04.68 MB [100%]
04_johann_sebastian_bach_maurice_gendron_bwv1007_sarabande.mp3        07.06 of 07.06 MB [100%]
[...]

It will create an "Artist - Album" directory in the path given as argument (or else in current
 directory if not given), and download all songs and covers available on that page.

------------------------------------------------------------------------------------------------------------------
##### To download all albums from an artist, give it an url with '/Artist/' or '/artist/'in it ###################
------------------------------------------------------------------------------------------------------------------

user@computer:/tmp$ generic-zic-downloader.py [-p /path] https://myzuka.club/Artist/7110/Johann-Sebastian-Bach/Albums
** We will try to use 3 simultaneous downloads, progress will be shown **
** after each completed file but not necessarily in album's order. **
** Warning: we are going to download all albums from this artist! **

Artist: Johann Sebastian Bach
Album: The 6 Cello Suites (CD1)
Year: 1994
cover.jpg                                                 00.01 of 00.01 MB [100%]
05_johann_sebastian_bach_maurice_gendron_bwv1007_menuets.mp3        07.04 of 07.04 MB [100%]
01_johann_sebastian_bach_maurice_gendron_bwv1007_prelude.mp3        05.57 of 05.57 MB [100%]
03_johann_sebastian_bach_maurice_gendron_bwv1007_courante.mp3        05.92 of 05.92 MB [100%]
[...]

Artist: Johann Sebastian Bach
Album: Prelude and Fugue in E Minor, BWV 548
Year: 1964
cover.jpg                                                 00.01 of 00.01 MB [100%]
01_johann_sebastian_bach_praeludium_myzuka.mp3            09.51 of 09.51 MB [100%]
02_johann_sebastian_bach_fuga_myzuka.mp3                  10.80 of 10.80 MB [100%]
** ALBUM DOWNLOAD FINISHED **

[...]

It will iterate on all albums of this artist.

------------------------------------------------------------------------------------------------------------------
################# Command line help ##############################################################################
------------------------------------------------------------------------------------------------------------------

For more info, see https://github.com/damsgithub/generic-zic-downloader.py

positional arguments:
  url                   URL of album or artist page

optional arguments:
  -h, --help            show this help message and exit
  -d {0,1,2}, --debug {0,1,2}
                        Debug verbosity: 0, 1, 2
  -l {0,1}
                        Use live display (rich): 0, 1
  -s SOCKS, --socks SOCKS
                        Socks proxy: "address:port" without "http://"
  -t TIMEOUT, --timeout TIMEOUT
                        Timeout for HTTP connections in seconds
  -n NB_CONN, --nb_conn NB_CONN
                        Number of simultaneous downloads (max 3 for tempfile.ru)
  -p PATH, --path PATH  Base directory in which album(s) will be downloaded. Defaults to current directory.
  -v, --version         show program's version number and exit
  
```

![term_capture](https://user-images.githubusercontent.com/24474244/109500836-0f489f00-7a97-11eb-8bd8-f1b5d6e036d6.jpg)
