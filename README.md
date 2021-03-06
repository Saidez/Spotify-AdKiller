# Spotify-AdKiller (for LINUX)

Your Party with Spotify - but without ads!

![Spotify-AdKiller](http://fs1.d-h.st/view/vJJ/00134/Spotify-AdKiller.png)

We all love Spotify, but sometimes people (like us) want to throw a party without interrupting ads before having bought premium. Well, now you can!

This is for testing purposes ONLY! Spotify is a fantastic service and worth every penny. This script is **NOT** meant to circumvent buying premium! Please do consider switching to premium to support Spotify - especially if you're going to use it on mobile. If the script does not work for you, help us improve it!

### Dependencies

Utilities used in the script:

  - xprop
  - pacmd
  - notify-send

You will also need to have one of the following audio/media players installed:

  - mpv
  - vlc
  - mplayer
  - mpg321
  - avplay
  - ffplay
  
Please consult the Settings section below for information on setting a custom audio player.

Install all utilities + VLC on **Ubuntu** with:

    sudo apt-get install x11-utils pulseaudio-utils libnotify-bin vlc

Install all utilities + VLC on **openSUSE** with:

    sudo zypper in binutils pulseaudio-utils libnotify-tools vlc

### Installation

**Automated installation**

Grab the latest release of Spotify-AdKiller:

    git clone https://github.com/SecUpwN/Spotify-AdKiller.git

Run the provided installer:

    cd Spotify-AdKiller
    ./install.sh

**Troubleshooting**

- if you've installed Spotify from any source other than the official repository please make sure that the `spotify` executable is in your `PATH`. 
 
    You can create a symbolic link, if necessary (e.g. linking `my-spotify` to `spotify` if you are using the user installation of [spotify-make](https://github.com/leamas/spotify-make)).

- the installer script will install Spotify-AdKiller to `$HOME/bin`, which should be recognized by Ubuntu and openSUSE
 
- if `$HOME/bin` didn't exist before, a relog might be necessary to complete the installation 

    Technical explanation: Ubuntu automatically adds `$HOME/bin` to your PATH if it exists when you log in. Relogging reloads `$HOME/.profile` and updates your PATH.

    If the script doesn't work after relogging you can either [manually add](http://askubuntu.com/q/3744) `$HOME/bin` to your PATH or follow the installation instructions below. This also applies if you're using a distro that is configured differently than Ubuntu/OpenSUSE or if you want to install the script in a custom location.

**Manual installation**

1. Copy `spotify-adkiller.sh` and `spotify-wrapper.sh` to your PATH (e.g. `$HOME/bin` or `/usr/local/bin` on openSUSE and Ubuntu) and make both of them executable with `chmod +x spotify-adkiller.sh` and `chmod +x spotify-wrapper.sh`.

2. Copy `Spotify (AdKiller).desktop` to `$HOME/.local/share/applications` or any other folder your distro reads `.desktop` launchers from (e.g. `/usr/share/applications`, `/usr/local/share/applications`)

The default configuration file will be written automatically on the first startup of the script.

### Usage

If you installed Spotify-AdKiller correctly a new entry called 'Spotify (AdKiller)' should appear in your menu. From now on please use this launcher to start Spotify.

The script will terminate automatically as soon as Spotify exits.

**Important note:** Please make sure you don't have notifications disabled in your Spotify configuration (`ui.track_notifications_enabled=true` in `~/.config/spotify/User/<your username>/prefs`).

### Settings

The configuration file for Spotify-AdKiller is located under `$HOME/.config/Spotify-AdKiller/Spotify-AdKiller.cfg`. There are several settings that control how Spotify-AdKiller operates:

**Modes**

`CUSTOM_MODE` controls the ad blocking mode. The following modes are available:

- `simple`: mute Spotify, unmute when ad is over
- `interstitial`: mute Spotify, play random local track, stop and unmute when ad is over
    + if the local track is shorter than the ad, Spotify-AdKiller will automatically try to loop it. This will only work with players that support a loop option. If you are planning to use this feature with a custom player make sure to also supply a custom loop option in your configuration file
- `continuous`: mute Spotify, play random local track, stop and unmute when track is over
    + You can skip the local track as soon as the ad is over. To do so, simply press Play or Forward/Next in your Spotify client (or use the corresponding hotkeys)
    + Please note that the `continuous` ad blocking mode works best with tracks that are longer than the average ad duration (≈30-45s). If a custom track ends prematurely/is shorter than the current ad Spotify-AdKiller will switch to the next random local track in line.

The default ad blocking mode is `continuous`.

Spotify-AdKiller will automatically fall back to `simple` mode if no local tracks are found and/or if no supported music player is available on the system.

**Local playback**

The following settings control local music playback during ads:

- `CUSTOM_PLAYER`: local music player to use; chosen automatically by default
- `CUSTOM_LOOPOPT`: loop option for custom player (e.g. `-loop 0`); we recommend setting this if you are planning to use interstitial adblocking mode
- `CUSTOM_VOLUME`: volume of local playback; set to 100 by default
- `CUSTOM_MUSIC`: local track to play / local music directory to choose tracks from; set to XDG standard music directory by default (e.g. `$HOME/Music`)
- `CUSTOM_ALERT`: audio alert to play when switching to local playback; XDG standard 'bell' sound by default; set to `none` to disable 

**Debug setting**

You can make the CLI output more verbose and enable the log file by setting `DEBUG` to `1`. The log will be written to `$HOME/.Spotify-AdKiller.log` and replaced each time the AdKiller is run.

### Important notes

Please make sure to always use the provided launcher when running Spotify. Under some circumstances Spotify might remain muted when exiting the application. This is a technical limitation with PulseAudio. Spotify AdKiller automatically unmutes Spotify when initially run.

If, for some reason, Spotify does remain muted you can use the following command to unmute it manually while it's running:

```bash
for i in $(LC_ALL=C pactl list | grep -E '(^Sink Input)|(media.name = \"Spotify\"$)' | cut -d \# -f2 | grep -v Spotify); do pactl set-sink-input-mute "$i" no; done
```

### Similar projects

- [blockify](https://github.com/mikar/blockify) - automatic/blacklist-based ad-blocker written in python
- [indicator-muteads](https://launchpad.net/indicator-muteads) - blacklist-based ad-blocker written in C
- [spotify_ad_blocker_linux.rb](https://github.com/superr4y/hacks/blob/master/spotify/spotify_ad_blocker_linux.rb) - automatic ad-blocker written in Ruby

### License

Many people have contributed to make our script become what it is today (huge shout-out to the initial creator [pcworld](https://github.com/pcworld)). If you are like us and think that it is very sad when projects die, please accept that all code here is fully licensed under GPL v3+. Have a look at the full [License](https://github.com/SecUpwN/Spotify-AdKiller/blob/master/LICENSE).

**This product is not endorsed, certified or otherwise approved in any way by Spotify. Spotify is the registered trade mark of the Spotify Group. Use your brainz prior to formatting your HDD.**
