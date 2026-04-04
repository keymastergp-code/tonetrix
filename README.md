# tonetrix
tonetrix detects Motorola-style two-tone paging sequences from a live audio input and automatically records the associated dispatch audio.
# tonetrix

tonetrix (may be pronounced "tone-tricks" or "toe-net-ricks", either is acceptable) is a Python program that detects Motorola-style two-tone paging
sequences from a live audio input and automatically records the
associated dispatch audio.

It is intended for fire and EMS radio monitoring setups where a scanner,
receiver, or SDR provides continuous audio and the user wants automatic
recording when tone-out pages occur.

## Features

-   Detects Motorola two-tone paging sequences (Tone A followed by Tone
    B)
-   Supports multiple station tone pairs
-   Pre-record buffer captures audio before the trigger
-   Automatically records dispatch audio after a trigger
-   MP3 output using ffmpeg (WAV fallback supported)
-   Optional live signal meter for tuning audio levels
-   Cooldown protection to prevent repeated triggers
-   Lightweight and suitable for always-on Linux monitoring systems

## Requirements

tonetrix requires Python 3 and the following packages:

-   python3-numpy
-   python3-pyaudio
-   ffmpeg
-   alsa-utils (optional but recommended)
-   portaudio19-dev (optional but recommended)
-   python3-dev (optional but recommended)
  
Example installation on Debian or Ubuntu:

apt install python3-numpy python3-pyaudio ffmpeg

## Installation

Clone the repository:

git clone https://github.com/keymastergp-code/tonetrix.git cd tonetrix

Make the program executable:

chmod +x tonetrix.py

Run it once as root to generate the default configuration:

sudo ./tonetrix.py

This will create:

/etc/tonetrix/event_tones.cfg /var/spool/tonetrix/audio

Edit the configuration file to add your tone pairs.
tonetrix does not need sudo or root priviliges (on a standard Debian 12 or newer system) after the initial launch creates the config and log files.

## Configuration Example

\[general\] input_device_index = -1 sample_rate = 48000 chunk_size =
1024 output_dir = /var/spool/tonetrix/audio pre_record_seconds = 3
record_seconds = 45 cooldown_seconds = 60

output_format = mp3 mp3_bitrate_kbps = 128

meter_enabled = yes

\[Station_1\] tone_a = 600.9 tone_b = 903.2

Additional stations can be added by creating additional sections.

## Usage

Start tonetrix:

./tonetrix.py

When a configured tone pair is detected, tonetrix will:

1.  Capture audio from the pre-record buffer
2.  Record the dispatch audio
3.  Save the recording to the output directory

Example output:

tonetrix started TRIGGER matched event=Station_1
file=20260403-184512_Station_1.mp3 SAVED
file=/var/spool/tonetrix/audio/20260403-184512_Station_1.mp3

## Audio Meter

tonetrix can display a simple live signal meter to help tune audio
levels.

Enable it in the config:

meter_enabled = yes

Example output:

METER rms=0.0142 score=42.1 freq=600.9 \[############\]

## Output Files

Recordings, by default are stored in:

/var/spool/tonetrix/audio

but may be stored elsewhere as specified in the config.

Files are named:

YYYYMMDD-HHMMSS_stationname.mp3

Example:

20260403-184512_Station_1.mp3

## License

tonetrix is released under the GNU General Public License v3.0
(GPL-3.0).
