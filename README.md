mopidy-jukebox
==============

[![Build Status](https://travis-ci.org/lshift/mopidy-jukebox.svg?branch=master)](https://travis-ci.org/lshift/mopidy-jukebox)

As the name suggests, it’s based off of [Mopidy](https://www.mopidy.com/). I went through the many and varied frontends for Mopidy, and found the [Mopidy-MusicBox-Webclient](https://github.com/pimusicbox/mopidy-musicbox-webclient), which got pretty close to what we needed for a frontend – it looks nice, the search is decent – but it is still designed towards the single-person use case. I’ve [forked it](https://github.com/palfrey/mopidy-musicbox-webclient/tree/nih) and started ripping parts out so it better fits our use (many people picking tracks, but they should all be by default just added to the end of the queue, not override the queue).

The other item we needed was playback over HTTP from everyone’s machines, for compatibility with the existing music libraries. Mopidy doesn’t have this out of the box, and I couldn’t find a plugin to do this, so I ended up doing this in a way we’ve talked about for a while for the existing jukebox, namely by [writing an extension to cache files](cookbooks/mopidy-jukebox/files/default/mopidy-cacher) from the music libraries on people’s machines and then just handing it over to the Mopidy local file scanning. I then extended out further the Musicbox frontend to allow new sources to be added and status of them to be determined. I even added in Chef cookbooks, so that I could install it all on a Raspberry Pi and use my Raspberry Chef work with it (which has now been extended to support Berkshelf).

## Installation
1. On Debian, `sudo apt-get install libsqlite3-dev libffi-dev gstreamer1.0-fluendo-mp3 gstreamer1.0-plugins-bad gstreamer1.0-alsa gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly libssl-dev gir1.2-gstreamer-1.0 gir1.2-gst-plugins-base-1.0 python-dev python-gi`
2. On OS X, `brew install libffi openssl sqlite pygobject gst-python gst-plugins-good gst-plugins-bad gst-plugins-ugly --with-mad`
3. On OWLinux, `sudo dnf install sqlite-devel openssl-devel python2-gstreamer1 gstreamer-python-devel python-gitapi redhat-rpm-config libffi-devel gstreamer1-plugins-good gstreamer1-plugins-bad-free gstreamer1-plugins-ugly`
    * You may need to run `sudo pip uninstall pyopenssl` and then `sudo pip install pyopenssl` if you see the error `AttributeError: 'module' object has no attribute 'SSL_ST_INIT'` in the output from step 7
4. `pip install youtube-dl==2016.02.13` (to solve pafy's dependencies)
5. `pip install -r requirements.txt`
6. Run `mopidy deps` and see if there's any playback bits you care about and don't have
7. Run `mopidy`

## Cacher
You should make a Cron job to do the following `mopidy cacher && mopidy local scan` at whatever frequency you want the music files to be updated. Correct values for this depend on your local machines and how often they like getting scraped.

## Raspberry Pi notes
* A HiFiBerry and the [Linux install guide for said](https://www.hifiberry.com/guides/configuring-linux-3-18-x/) are recommended.
* If `aplay /usr/share/sounds/alsa/Front_Right.wav` plays back things ok, then Mopidy should be ok as well.
* Use of my [Raspberry Chef](https://github.com/palfrey/raspberry-chef) work and this repository *should* work...
