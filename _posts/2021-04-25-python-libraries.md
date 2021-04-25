---
title: Python Libraries
author: Richard Hernandez
date: 2021-04-25 12:00:00 +0800 
categories: [Cheatsheets]
tags: [python, cheatsheet, plot, numpy, audio, pygame, progressbar, scraping, pandas]
---


**Libraries:**  **[`Progress_Bar`](#progress-bar)**__,__ **[`Plot`](#plot)**__,__ **[`Table`](#table)**__,__ **[`Curses`](#curses)**__,__ **[`Logging`](#logging)**__,__ **[`Scraping`](#scraping)**__,__ **[`Web`](#web)**__,__ **[`Profile`](#profiling)**__,__  **[`NumPy`](#numpy)**__,__ **[`Image`](#image)**__,__ **[`Audio`](#audio)**__,__ **[`Games`](#pygame)**__,__ **[`Data`](#pandas)**__.__

Libraries
=========

Progress Bar
------------
```python
# $ pip3 install tqdm
>>> from tqdm import tqdm
>>> from time import sleep
>>> for el in tqdm([1, 2, 3], desc='Processing'):
...     sleep(1)
Processing: 100%|████████████████████| 3/3 [00:03<00:00,  1.00s/it]
```


Plot
----
```python
# $ pip3 install matplotlib
import matplotlib.pyplot as plt
plt.plot(<y_data> [, label=<str>])
plt.plot(<x_data>, <y_data>)
plt.legend()                                   # Adds a legend.
plt.savefig(<path>)                            # Saves the figure.
plt.show()                                     # Displays the figure.
plt.clf()                                      # Clears the figure.
```


Table
-----
#### Prints a CSV file as an ASCII table:
```python
# $ pip3 install tabulate
import csv, tabulate
with open('test.csv', encoding='utf-8', newline='') as file:
    rows   = csv.reader(file)
    header = [a.title() for a in next(rows)]
    table  = tabulate.tabulate(rows, header)
    print(table)
```


Curses
------
#### Runs a basic file explorer in the terminal:
```python
from curses import wrapper, ascii, A_REVERSE, KEY_UP, KEY_DOWN, KEY_LEFT, KEY_RIGHT, KEY_ENTER
from os import listdir, chdir, path

def main(screen):
    ch, first, selected, paths = 0, 0, 0, listdir()
    while ch != ascii.ESC:
        height, _ = screen.getmaxyx()
        screen.clear()
        for y, a_path in enumerate(paths[first : first+height]):
            screen.addstr(y, 0, a_path, A_REVERSE * (selected == first + y))
        ch = screen.getch()
        selected += (ch == KEY_DOWN) - (ch == KEY_UP)
        selected = max(0, min(len(paths)-1, selected))
        first += (first <= selected - height) - (first > selected)
        if ch in [KEY_LEFT, KEY_RIGHT, KEY_ENTER, 10, 13]:
            new_dir = '..' if ch == KEY_LEFT else paths[selected]
            if path.isdir(new_dir):
                chdir(new_dir)
                first, selected, paths = 0, 0, listdir()

if __name__ == '__main__':
    wrapper(main)
```


Logging
-------
```python
# $ pip3 install loguru
from loguru import logger
```

```python
logger.add('debug_{time}.log', colorize=True)  # Connects a log file.
logger.add('error_{time}.log', level='ERROR')  # Another file for errors or higher.
logger.<level>('A logging message.')
```
* **Levels: `'debug'`, `'info'`, `'success'`, `'warning'`, `'error'`, `'critical'`.**

### Exceptions
**Exception description, stack trace and values of variables are appended automatically.**

```python
try:
    ...
except <exception>:
    logger.exception('An error happened.')
```

### Rotation
**Argument that sets a condition when a new log file is created.**
```python
rotation=<int>|<datetime.timedelta>|<datetime.time>|<str>
```
* **`'<int>'` - Max file size in bytes.**
* **`'<timedelta>'` - Max age of a file.**
* **`'<time>'` - Time of day.**
* **`'<str>'` - Any of above as a string: `'100 MB'`, `'1 month'`, `'monday at 12:00'`, ...**

### Retention
**Sets a condition which old log files get deleted.**
```python
retention=<int>|<datetime.timedelta>|<str>
```
* **`'<int>'` - Max number of files.**
* **`'<timedelta>'` - Max age of a file.**
* **`'<str>'` - Max age as a string: `'1 week, 3 days'`, `'2 months'`, ...**


Scraping
--------
#### Scrapes Python's URL, version number and logo from its Wikipedia page:
```python
# $ pip3 install requests beautifulsoup4
import requests, bs4, sys

WIKI_URL = 'https://en.wikipedia.org/wiki/Python_(programming_language)'
try:
    html       = requests.get(WIKI_URL).text
    document   = bs4.BeautifulSoup(html, 'html.parser')
    table      = document.find('table', class_='infobox vevent')
    python_url = table.find('th', text='Website').next_sibling.a['href']
    version    = table.find('th', text='Stable release').next_sibling.strings.__next__()
    logo_url   = table.find('img')['src']
    logo       = requests.get(f'https:{logo_url}').content
    with open('test.png', 'wb') as file:
        file.write(logo)
    print(python_url, version)
except requests.exceptions.ConnectionError:
    print("You've got problems with connection.", file=sys.stderr)
```


Web
---
```python
# $ pip3 install bottle
from bottle import run, route, static_file, template, post, request, response
import json
```

### Run
```python
run(host='localhost', port=8080)        # Runs locally.
run(host='0.0.0.0', port=80)            # Runs globally.
```

### Static Request
```python
@route('/img/<image>')
def send_image(image):
    return static_file(image, 'img_dir/', mimetype='image/png')
```

### Dynamic Request
```python
@route('/<sport>')
def send_page(sport):
    return template('<h1>{{title}}</h1>', title=sport)
```

### REST Request
```python
@post('/odds/<sport>')
def odds_handler(sport):
    team = request.forms.get('team')
    home_odds, away_odds = 2.44, 3.29
    response.headers['Content-Type'] = 'application/json'
    response.headers['Cache-Control'] = 'no-cache'
    return json.dumps([team, home_odds, away_odds])
```

#### Test:
```python
# $ pip3 install requests
>>> import threading, requests
>>> threading.Thread(target=run, daemon=True).start()
>>> url = 'http://localhost:8080/odds/football'
>>> data = {'team': 'arsenal f.c.'}
>>> response = requests.post(url, data=data)
>>> response.json()
['arsenal f.c.', 2.44, 3.29]
```


Profiling
---------
### Stopwatch
```python
from time import time
start_time = time()                     # Seconds since the Epoch.
...
duration = time() - start_time
```

#### High performance:
```python
from time import perf_counter
start_time = perf_counter()             # Seconds since the restart.
...
duration = perf_counter() - start_time
```

### Timing a Snippet
```python
>>> from timeit import timeit
>>> timeit("''.join(str(i) for i in range(100))",
...        number=10000, globals=globals(), setup='pass')
0.34986
```

### Profiling by Line
```python
# $ pip3 install line_profiler memory_profiler
@profile
def main():
    a = [*range(10000)]
    b = {*range(10000)}
main()
```

```text
$ kernprof -lv test.py
Line #   Hits     Time  Per Hit   % Time  Line Contents
=======================================================
     1                                    @profile
     2                                    def main():
     3      1    955.0    955.0     43.7      a = [*range(10000)]
     4      1   1231.0   1231.0     56.3      b = {*range(10000)}
```

```text
$ python3 -m memory_profiler test.py
Line #         Mem usage      Increment   Line Contents
=======================================================
     1        37.668 MiB     37.668 MiB   @profile
     2                                    def main():
     3        38.012 MiB      0.344 MiB       a = [*range(10000)]
     4        38.477 MiB      0.465 MiB       b = {*range(10000)}
```

### Call Graph
#### Generates a PNG image of a call graph with highlighted bottlenecks:
```python
# $ pip3 install pycallgraph
from pycallgraph import output, PyCallGraph
from datetime import datetime
time_str = datetime.now().strftime('%Y%m%d%H%M%S')
filename = f'profile-{time_str}.png'
drawer = output.GraphvizOutput(output_file=filename)
with PyCallGraph(drawer):
    <code_to_be_profiled>
```


NumPy
-----
**Array manipulation mini-language. It can run up to one hundred times faster than the equivalent Python code. An even faster alternative that runs on a GPU is called CuPy.**

```python
# $ pip3 install numpy
import numpy as np
```

```python
<array> = np.array(<list>)
<array> = np.arange(from_inclusive, to_exclusive, ±step_size)
<array> = np.ones(<shape>)
<array> = np.random.randint(from_inclusive, to_exclusive, <shape>)
```

```python
<array>.shape = <shape>
<view>  = <array>.reshape(<shape>)
<view>  = np.broadcast_to(<array>, <shape>)
```

```python
<array> = <array>.sum(axis)
indexes = <array>.argmin(axis)
```

* **Shape is a tuple of dimension sizes.**
* **Axis is an index of the dimension that gets collapsed. Leftmost dimension has index 0.**

### Indexing
```bash
<el>       = <2d_array>[row_index, column_index]
<1d_view>  = <2d_array>[row_index]
<1d_view>  = <2d_array>[:, column_index]
```

```bash
<1d_array> = <2d_array>[row_indexes, column_indexes]
<2d_array> = <2d_array>[row_indexes]
<2d_array> = <2d_array>[:, column_indexes]
```

```bash
<2d_bools> = <2d_array> ><== <el>
<1d_array> = <2d_array>[<2d_bools>]
```

### Broadcasting
**Broadcasting is a set of rules by which NumPy functions operate on arrays of different sizes and/or dimensions.**

```python
left  = [[0.1], [0.6], [0.8]]        # Shape: (3, 1)
right = [ 0.1 ,  0.6 ,  0.8 ]        # Shape: (3)
```

#### 1. If array shapes differ in length, left-pad the shorter shape with ones:
```python
left  = [[0.1], [0.6], [0.8]]        # Shape: (3, 1)
right = [[0.1 ,  0.6 ,  0.8]]        # Shape: (1, 3) <- !
```

#### 2. If any dimensions differ in size, expand the ones that have size 1 by duplicating their elements:
```python
left  = [[0.1, 0.1, 0.1], [0.6, 0.6, 0.6], [0.8, 0.8, 0.8]]  # Shape: (3, 3) <- !
right = [[0.1, 0.6, 0.8], [0.1, 0.6, 0.8], [0.1, 0.6, 0.8]]  # Shape: (3, 3) <- !
```

#### 3. If neither non-matching dimension has size 1, raise an error.


### Example
#### For each point returns index of its nearest point (`[0.1, 0.6, 0.8] => [1, 2, 1]`):

```python
>>> points = np.array([0.1, 0.6, 0.8])
 [ 0.1,  0.6,  0.8]
>>> wrapped_points = points.reshape(3, 1)
[[ 0.1],
 [ 0.6],
 [ 0.8]]
>>> distances = wrapped_points - points
[[ 0. , -0.5, -0.7],
 [ 0.5,  0. , -0.2],
 [ 0.7,  0.2,  0. ]]
>>> distances = np.abs(distances)
[[ 0. ,  0.5,  0.7],
 [ 0.5,  0. ,  0.2],
 [ 0.7,  0.2,  0. ]]
>>> i = np.arange(3)
[0, 1, 2]
>>> distances[i, i] = np.inf
[[ inf,  0.5,  0.7],
 [ 0.5,  inf,  0.2],
 [ 0.7,  0.2,  inf]]
>>> distances.argmin(1)
[1, 2, 1]
```


Image
-----
```python
# $ pip3 install pillow
from PIL import Image
```

```python
<Image> = Image.new('<mode>', (width, height))  # Also: `color=<int/tuple/str>`.
<Image> = Image.open(<path>)                    # Identifies format based on file contents.
<Image> = <Image>.convert('<mode>')             # Converts image to the new mode.
<Image>.save(<path>)                            # Selects format based on the path extension.
<Image>.show()                                  # Opens image in default preview app.
```

```python
<int/tuple> = <Image>.getpixel((x, y))          # Returns a pixel.
<Image>.putpixel((x, y), <int/tuple>)           # Writes a pixel to the image.
<ImagingCore> = <Image>.getdata()               # Returns a sequence of pixels.
<Image>.putdata(<list/ImagingCore>)             # Writes a sequence of pixels.
<Image>.paste(<Image>, (x, y))                  # Writes an image to the image.
```

```bash
<2d_array> = np.array(<Image_L>)                # Creates NumPy array from greyscale image.
<3d_array> = np.array(<Image_RGB>)              # Creates NumPy array from color image.
<Image>    = Image.fromarray(<array>)           # Creates image from NumPy array of floats.
```

### Modes
* **`'1'` - 1-bit pixels, black and white, stored with one pixel per byte.**
* **`'L'` - 8-bit pixels, greyscale.**
* **`'RGB'` - 3x8-bit pixels, true color.**
* **`'RGBA'` - 4x8-bit pixels, true color with transparency mask.**
* **`'HSV'` - 3x8-bit pixels, Hue, Saturation, Value color space.**

### Examples
#### Creates a PNG image of a rainbow gradient:
```python
WIDTH, HEIGHT = 100, 100
size = WIDTH * HEIGHT
hues = (255 * i/size for i in range(size))
img = Image.new('HSV', (WIDTH, HEIGHT))
img.putdata([(int(h), 255, 255) for h in hues])
img.convert('RGB').save('test.png')
```

#### Adds noise to a PNG image:
```python
from random import randint
add_noise = lambda value: max(0, min(255, value + randint(-20, 20)))
img = Image.open('test.png').convert('HSV')
img.putdata([(add_noise(h), s, v) for h, s, v in img.getdata()])
img.convert('RGB').save('test.png')
```

### Image Draw
```python
from PIL import ImageDraw
<ImageDraw> = ImageDraw.Draw(<Image>)
```

```python
<ImageDraw>.point((x, y), fill=None)
<ImageDraw>.line((x1, y1, x2, y2 [, ...]), fill=None, width=0, joint=None) 
<ImageDraw>.arc((x1, y1, x2, y2), from_deg, to_deg, fill=None, width=0)
<ImageDraw>.rectangle((x1, y1, x2, y2), fill=None, outline=None, width=0)
<ImageDraw>.polygon((x1, y1, x2, y2 [, ...]), fill=None, outline=None)
<ImageDraw>.ellipse((x1, y1, x2, y2), fill=None, outline=None, width=0)
```
* **Use `'fill=<color>'` to set the primary color.**
* **Use `'outline=<color>'` to set the secondary color.**
* **Color can be specified as an int, tuple, `'#rrggbb[aa]'` string or a color name.**


Animation
---------
#### Creates a GIF of a bouncing ball:
```python
# $ pip3 install imageio
from PIL import Image, ImageDraw
import imageio
WIDTH, R = 126, 10
frames = []
for velocity in range(1, 16):
    y = sum(range(velocity))
    frame = Image.new('L', (WIDTH, WIDTH))
    draw  = ImageDraw.Draw(frame)
    draw.ellipse((WIDTH/2-R, y, WIDTH/2+R, y+R*2), fill='white')
    frames.append(frame)
frames += reversed(frames[1:-1])
imageio.mimsave('test.gif', frames, duration=0.03)
```


Audio
-----
```python
import wave
```

```python
<Wave_read>  = wave.open('<path>', 'rb')        # Opens the WAV file.
framerate    = <Wave_read>.getframerate()       # Number of frames per second.
nchannels    = <Wave_read>.getnchannels()       # Number of samples per frame.
sampwidth    = <Wave_read>.getsampwidth()       # Sample size in bytes.
nframes      = <Wave_read>.getnframes()         # Number of frames.
<params>     = <Wave_read>.getparams()          # Immutable collection of above.
<bytes>      = <Wave_read>.readframes(nframes)  # Returns next 'nframes' frames.
```

```python
<Wave_write> = wave.open('<path>', 'wb')        # Truncates existing file.
<Wave_write>.setframerate(<int>)                # 44100 for CD, 48000 for video.
<Wave_write>.setnchannels(<int>)                # 1 for mono, 2 for stereo.
<Wave_write>.setsampwidth(<int>)                # 2 for CD quality sound.
<Wave_write>.setparams(<params>)                # Sets all parameters.
<Wave_write>.writeframes(<bytes>)               # Appends frames to the file.
```
* **Bytes object contains a sequence of frames, each consisting of one or more samples.**
* **In a stereo signal, the first sample of a frame belongs to the left channel.**
* **Each sample consists of one or more bytes that, when converted to an integer, indicate the displacement of a speaker membrane at a given moment.**
* **If sample width is one, then the integer should be encoded unsigned.**
* **For all other sizes, the integer should be encoded signed with little-endian byte order.**

### Sample Values
```text
+-----------+-------------+------+-------------+
| sampwidth |     min     | zero |     max     |
+-----------+-------------+------+-------------+
|     1     |           0 |  128 |         255 |
|     2     |      -32768 |    0 |       32767 |
|     3     |    -8388608 |    0 |     8388607 |
|     4     | -2147483648 |    0 |  2147483647 |
+-----------+-------------+------+-------------+
```

### Read Float Samples from WAV File
```python
def read_wav_file(filename):
    def get_int(bytes_obj):
        an_int = int.from_bytes(bytes_obj, 'little', signed=sampwidth!=1)
        return an_int - 128 * (sampwidth == 1)
    with wave.open(filename, 'rb') as file:
        sampwidth = file.getsampwidth()
        frames = file.readframes(-1)
    bytes_samples = (frames[i : i+sampwidth] for i in range(0, len(frames), sampwidth))
    return [get_int(b) / pow(2, sampwidth * 8 - 1) for b in bytes_samples]
```

### Write Float Samples to WAV File
```python
def write_to_wav_file(filename, float_samples, nchannels=1, sampwidth=2, framerate=44100):
    def get_bytes(a_float):
        a_float = max(-1, min(1 - 2e-16, a_float))
        a_float += sampwidth == 1
        a_float *= pow(2, sampwidth * 8 - 1)
        return int(a_float).to_bytes(sampwidth, 'little', signed=sampwidth!=1) 
    with wave.open(filename, 'wb') as file:
        file.setnchannels(nchannels)
        file.setsampwidth(sampwidth)
        file.setframerate(framerate)
        file.writeframes(b''.join(get_bytes(f) for f in float_samples))
```

### Examples
#### Saves a sine wave to a mono WAV file:
```python
from math import pi, sin
samples_f = (sin(i * 2 * pi * 440 / 44100) for i in range(100000))
write_to_wav_file('test.wav', samples_f)
```

#### Adds noise to a mono WAV file:
```python
from random import random
add_noise = lambda value: value + (random() - 0.5) * 0.03
samples_f = (add_noise(f) for f in read_wav_file('test.wav'))
write_to_wav_file('test.wav', samples_f)
```

#### Plays a WAV file:
```python
# $ pip3 install simpleaudio
from simpleaudio import play_buffer
with wave.open('test.wav', 'rb') as file:
    p = file.getparams()
    frames = file.readframes(-1)
    play_buffer(frames, p.nchannels, p.sampwidth, p.framerate)
```

### Text to Speech
```python
# $ pip3 install pyttsx3
import pyttsx3
engine = pyttsx3.init()
engine.say('Sally sells seashells by the seashore.')
engine.runAndWait()
```


Synthesizer
-----------
#### Plays Popcorn by Gershon Kingsley:
```python
# $ pip3 install simpleaudio
import math, struct, simpleaudio
from itertools import repeat, chain
F  = 44100
P1 = '71♩,69♪,,71♩,66♪,,62♩,66♪,,59♩,,,'
P2 = '71♩,73♪,,74♩,73♪,,74♪,,71♪,,73♩,71♪,,73♪,,69♪,,71♩,69♪,,71♪,,67♪,,71♩,,,'
get_pause   = lambda seconds: repeat(0, int(seconds * F))
sin_f       = lambda i, hz: math.sin(i * 2 * math.pi * hz / F)
get_wave    = lambda hz, seconds: (sin_f(i, hz) for i in range(int(seconds * F)))
get_hz      = lambda key: 8.176 * 2 ** (int(key) / 12)
parse_note  = lambda note: (get_hz(note[:2]), 1/4 if '♩' in note else 1/8)
get_samples = lambda note: get_wave(*parse_note(note)) if note else get_pause(1/8)
samples_f   = chain.from_iterable(get_samples(n) for n in f'{P1}{P1}{P2}'.split(','))
samples_b   = b''.join(struct.pack('<h', int(f * 30000)) for f in samples_f)
simpleaudio.play_buffer(samples_b, 1, 2, F)
```


Pygame
------
### Basic Example
```python
# $ pip3 install pygame
import pygame as pg
pg.init()
screen = pg.display.set_mode((500, 500))
rect = pg.Rect(240, 240, 20, 20)
while all(event.type != pg.QUIT for event in pg.event.get()):
    deltas = {pg.K_UP: (0, -3), pg.K_RIGHT: (3, 0), pg.K_DOWN: (0, 3), pg.K_LEFT: (-3, 0)}
    for key_code, is_pressed in enumerate(pg.key.get_pressed()):
        rect = rect.move(deltas[key_code]) if key_code in deltas and is_pressed else rect
    screen.fill((0, 0, 0))
    pg.draw.rect(screen, (255, 255, 255), rect)
    pg.display.flip()
```

### Rectangle
**Object for storing rectangular coordinates.**
```python
<Rect> = pg.Rect(x, y, width, height)           # Floats get truncated into ints.
<int>  = <Rect>.x/y/centerx/centery/…           # Top, right, bottom, left. Allows assignments.
<tup.> = <Rect>.topleft/center/…                # Topright, bottomright, bottomleft.
<Rect> = <Rect>.move((x, y))                    # Use move_ip() to move in place.
```

```python
<bool> = <Rect>.collidepoint((x, y))            # Checks if rectangle contains a point.
<bool> = <Rect>.colliderect(<Rect>)             # Checks if two rectangles overlap.
<int>  = <Rect>.collidelist(<list_of_Rect>)     # Returns index of first colliding Rect or -1.
<list> = <Rect>.collidelistall(<list_of_Rect>)  # Returns indexes of all colliding Rects.
```

### Surface
**Object for representing images.**
```python
<Surf> = pg.display.set_mode((width, height))   # Returns display surface.
<Surf> = pg.Surface((width, height), …)         # New RGB surface. Add `pg.SRCALPHA` for RGBA.
<Surf> = pg.image.load('<path>')                # Loads the image. Format depends on source.
<Surf> = <Surf>.subsurface(<Rect>)              # Returns a subsurface.
```

```python
<Surf>.fill(color)                              # Tuple, Color('#rrggbb[aa]') or Color(<name>).
<Surf>.set_at((x, y), color)                    # Updates pixel.
<Surf>.blit(<Surf>, (x, y))                     # Draws passed surface to the surface.
```

```python
from pygame.transform import scale, …
<Surf> = scale(<Surf>, (width, height))         # Returns scaled surface.
<Surf> = rotate(<Surf>, degrees)                # Returns rotated and scaled surface.
<Surf> = flip(<Surf>, x_bool, y_bool)           # Returns flipped surface.
```

```python
from pygame.draw import line, arc, rect
line(<Surf>, color, (x1, y1), (x2, y2), width)  # Draws a line to the surface.
arc(<Surf>, color, <Rect>, from_rad, to_rad)    # Also: ellipse(<Surf>, color, <Rect>)
rect(<Surf>, color, <Rect>)                     # Also: polygon(<Surf>, color, points)
```

### Font
```python
<Font> = pg.font.SysFont('<name>', size)        # Loads the system font or default if missing.
<Font> = pg.font.Font('<path>', size)           # Loads the TTF file. Pass None for default.
<Surf> = <Font>.render(text, antialias, color)  # Background color can be specified at the end.
```

### Sound
```python
<Sound> = pg.mixer.Sound('<path>')              # Loads the WAV file.
<Sound>.play()                                  # Starts playing the sound.
```

### Basic Mario Brothers Example
```python
import collections, dataclasses, enum, io, itertools as it, pygame as pg, urllib.request
from random import randint

P = collections.namedtuple('P', 'x y')          # Position
D = enum.Enum('D', 'n e s w')                   # Direction
SIZE, MAX_SPEED = 50, P(5, 10)                  # Screen size, Speed limit

def main():
    def get_screen():
        pg.init()
        return pg.display.set_mode(2 * [SIZE*16])
    def get_images():
        url = 'https://gto76.github.io/python-cheatsheet/web/mario_bros.png'
        img = pg.image.load(io.BytesIO(urllib.request.urlopen(url).read()))
        return [img.subsurface(get_rect(x, 0)) for x in range(img.get_width() // 16)]
    def get_mario():
        Mario = dataclasses.make_dataclass('Mario', 'rect spd facing_left frame_cycle'.split())
        return Mario(get_rect(1, 1), P(0, 0), False, it.cycle(range(3)))
    def get_tiles():
        positions = [p for p in it.product(range(SIZE), repeat=2) if {*p} & {0, SIZE-1}] + \
            [(randint(1, SIZE-2), randint(2, SIZE-2)) for _ in range(SIZE**2 // 10)]
        return [get_rect(*p) for p in positions]
    def get_rect(x, y):
        return pg.Rect(x*16, y*16, 16, 16)
    run(get_screen(), get_images(), get_mario(), get_tiles())

def run(screen, images, mario, tiles):
    clock = pg.time.Clock()
    while all(event.type != pg.QUIT for event in pg.event.get()):
        keys = {pg.K_UP: D.n, pg.K_RIGHT: D.e, pg.K_DOWN: D.s, pg.K_LEFT: D.w}
        pressed = {keys.get(i) for i, on in enumerate(pg.key.get_pressed()) if on}
        update_speed(mario, tiles, pressed)
        update_position(mario, tiles)
        draw(screen, images, mario, tiles, pressed)
        clock.tick(28)

def update_speed(mario, tiles, pressed):
    x, y = mario.spd
    x += 2 * ((D.e in pressed) - (D.w in pressed))
    x -= x // abs(x) if x else 0
    y += 1 if D.s not in get_boundaries(mario.rect, tiles) else (D.n in pressed) * -10
    mario.spd = P(*[max(-limit, min(limit, s)) for limit, s in zip(MAX_SPEED, P(x, y))])

def update_position(mario, tiles):
    x, y = mario.rect.topleft
    n_steps = max(abs(s) for s in mario.spd)
    for _ in range(n_steps):
        mario.spd = stop_on_collision(mario.spd, get_boundaries(mario.rect, tiles))
        x, y = x + mario.spd.x/n_steps, y + mario.spd.y/n_steps
        mario.rect.topleft = x, y

def get_boundaries(rect, tiles):
    deltas = {D.n: P(0, -1), D.e: P(1, 0), D.s: P(0, 1), D.w: P(-1, 0)}
    return {d for d, delta in deltas.items() if rect.move(delta).collidelist(tiles) != -1}

def stop_on_collision(spd, bounds):
    return P(x=0 if (D.w in bounds and spd.x < 0) or (D.e in bounds and spd.x > 0) else spd.x,
             y=0 if (D.n in bounds and spd.y < 0) or (D.s in bounds and spd.y > 0) else spd.y)

def draw(screen, images, mario, tiles, pressed):
    def get_frame_index():
        if D.s not in get_boundaries(mario.rect, tiles):
            return 4
        return next(mario.frame_cycle) if {D.w, D.e} & pressed else 6
    screen.fill((85, 168, 255))
    mario.facing_left = (D.w in pressed) if {D.w, D.e} & pressed else mario.facing_left
    screen.blit(images[get_frame_index() + mario.facing_left * 9], mario.rect)
    for rect in tiles:
        screen.blit(images[18 if {*rect.topleft} & {0, (SIZE-1)*16} else 19], rect)
    pg.display.flip()

if __name__ == '__main__':
    main()
```


Pandas
------
```python
# $ pip3 install pandas
import pandas as pd
from pandas import Series, DataFrame
```

### Series
**Ordered dictionary with a name.**

```python
>>> Series([1, 2], index=['x', 'y'], name='a')
x    1
y    2
Name: a, dtype: int64
```

```python
<Sr> = Series(<list>)                         # Assigns RangeIndex starting at 0.
<Sr> = Series(<dict>)                         # Takes dictionary's keys for index.
<Sr> = Series(<dict/Series>, index=<list>)    # Only keeps items with keys specified in index.
```

```python
<el> = <Sr>.loc[key]                          # Or: <Sr>.iloc[index]
<Sr> = <Sr>.loc[keys]                         # Or: <Sr>.iloc[indexes]
<Sr> = <Sr>.loc[from_key : to_key_inclusive]  # Or: <Sr>.iloc[from_i : to_i_exclusive]
```

```python
<el> = <Sr>[key/index]                        # Or: <Sr>.key
<Sr> = <Sr>[keys/indexes]                     # Or: <Sr>[<key_range/range>]
<Sr> = <Sr>[bools]                            # Or: <Sr>.i/loc[bools]
```

```python
<Sr> = <Sr> ><== <el/Sr>                      # Returns a Series of bools.
<Sr> = <Sr> +-*/ <el/Sr>                      # Items with non-matching keys get value NaN.
```

```python
<Sr> = <Sr>.append(<Sr>)                      # Or: pd.concat(<coll_of_Sr>)
<Sr> = <Sr>.combine_first(<Sr>)               # Adds items that are not yet present.
<Sr>.update(<Sr>)                             # Updates items that are already present.
```

#### Aggregate, Transform, Map:
```python
<el> = <Sr>.sum/max/mean/idxmax/all()         # Or: <Sr>.aggregate(<agg_func>)
<Sr> = <Sr>.rank/diff/cumsum/ffill/interpl()  # Or: <Sr>.agg/transform(<trans_func>)
<Sr> = <Sr>.fillna(<el>)                      # Or: <Sr>.apply/agg/transform/map(<map_func>)
```
* **The way `'aggregate()'` and `'transform()'` find out whether the passed function accepts an element or the whole Series is by passing it a single value at first and if it raises an error, then they pass it the whole Series.**

```python
>>> sr = Series([1, 2], index=['x', 'y'])
x    1
y    2
```

```text
+-------------+-------------+-------------+---------------+
|             |    'sum'    |   ['sum']   | {'s': 'sum'}  |
+-------------+-------------+-------------+---------------+
| sr.apply(…) |      3      |    sum  3   |     s  3      |
| sr.agg(…)   |             |             |               |
+-------------+-------------+-------------+---------------+
```

```text
+-------------+-------------+-------------+---------------+
|             |    'rank'   |   ['rank']  | {'r': 'rank'} |
+-------------+-------------+-------------+---------------+
| sr.apply(…) |             |      rank   |               |
| sr.agg(…)   |     x  1    |   x     1   |    r  x  1    |
| sr.trans(…) |     y  2    |   y     2   |       y  2    |
+-------------+-------------+-------------+---------------+
```
* **Last result has a hierarchical index. Use `'<Sr>[key_1, key_2]'` to get its values.**

### DataFrame
**Table with labeled rows and columns.**

```python
>>> DataFrame([[1, 2], [3, 4]], index=['a', 'b'], columns=['x', 'y'])
   x  y
a  1  2
b  3  4
```

```python
<DF>    = DataFrame(<list_of_rows>)           # Rows can be either lists, dicts or series.
<DF>    = DataFrame(<dict_of_columns>)        # Columns can be either lists, dicts or series.
```

```python
<el>    = <DF>.loc[row_key, column_key]       # Or: <DF>.iloc[row_index, column_index]
<Sr/DF> = <DF>.loc[row_key/s]                 # Or: <DF>.iloc[row_index/es]
<Sr/DF> = <DF>.loc[:, column_key/s]           # Or: <DF>.iloc[:, column_index/es]
<DF>    = <DF>.loc[row_bools, column_bools]   # Or: <DF>.iloc[row_bools, column_bools]
```

```python
<Sr/DF> = <DF>[column_key/s]                  # Or: <DF>.column_key
<DF>    = <DF>[row_bools]                     # Keeps rows as specified by bools.
<DF>    = <DF>[<DF_of_bools>]                 # Assigns NaN to False values.
```

```python
<DF>    = <DF> ><== <el/Sr/DF>                # Returns DataFrame of bools.
<DF>    = <DF> +-*/ <el/Sr/DF>                # Items with non-matching keys get value NaN.
```

```python
<DF>    = <DF>.set_index(column_key)          # Replaces row keys with values from a column.
<DF>    = <DF>.reset_index()                  # Moves row keys to a column named index.
<DF>    = <DF>.filter('<regex>', axis=1)      # Only keeps columns whose key matches the regex.
<DF>    = <DF>.melt(id_vars=column_key/s)     # Converts DataFrame from wide to long format.
```

#### Merge, Join, Concat:
```python
>>> l = DataFrame([[1, 2], [3, 4]], index=['a', 'b'], columns=['x', 'y'])
   x  y 
a  1  2 
b  3  4 
>>> r = DataFrame([[4, 5], [6, 7]], index=['b', 'c'], columns=['y', 'z'])
   y  z
b  4  5
c  6  7
```

```text
+------------------------+---------------+------------+------------+--------------------------+
|                        |    'outer'    |   'inner'  |   'left'   |       Description        |
+------------------------+---------------+------------+------------+--------------------------+
| l.merge(r, on='y',     |    x   y   z  | x   y   z  | x   y   z  | Joins/merges on column.  |
|            how=…)      | 0  1   2   .  | 3   4   5  | 1   2   .  | Also accepts left_on and |
|                        | 1  3   4   5  |            | 3   4   5  | right_on parameters.     |
|                        | 2  .   6   7  |            |            | Uses 'inner' by default. |
+------------------------+---------------+------------+------------+--------------------------+
| l.join(r, lsuffix='l', |    x yl yr  z |            | x yl yr  z | Joins/merges on row keys.|
|           rsuffix='r', | a  1  2  .  . | x yl yr  z | 1  2  .  . | Uses 'left' by default.  |
|           how=…)       | b  3  4  4  5 | 3  4  4  5 | 3  4  4  5 | If r is a series, it is  |
|                        | c  .  .  6  7 |            |            | treated as a column.     |
+------------------------+---------------+------------+------------+--------------------------+
| pd.concat([l, r],      |    x   y   z  |     y      |            | Adds rows at the bottom. |
|           axis=0,      | a  1   2   .  |     2      |            | Uses 'outer' by default. |
|           join=…)      | b  3   4   .  |     4      |            | A series is treated as a |
|                        | b  .   4   5  |     4      |            | column. Use l.append(r)  |
|                        | c  .   6   7  |     6      |            | to add a row instead.    |
+------------------------+---------------+------------+------------+--------------------------+
| pd.concat([l, r],      |    x  y  y  z |            |            | Adds columns at the      |
|           axis=1,      | a  1  2  .  . | x  y  y  z |            | right end. Uses 'outer'  |
|           join=…)      | b  3  4  4  5 | 3  4  4  5 |            | by default. A series is  |
|                        | c  .  .  6  7 |            |            | treated as a column.     |
+------------------------+---------------+------------+------------+--------------------------+
| l.combine_first(r)     |    x   y   z  |            |            | Adds missing rows and    |
|                        | a  1   2   .  |            |            | columns. Also updates    |
|                        | b  3   4   5  |            |            | items that contain NaN.  |
|                        | c  .   6   7  |            |            | R must be a DataFrame.   |
+------------------------+---------------+------------+------------+--------------------------+
```

#### Aggregate, Transform, Map:
```python
<Sr> = <DF>.sum/max/mean/idxmax/all()         # Or: <DF>.apply/agg/transform(<agg_func>)
<DF> = <DF>.rank/diff/cumsum/ffill/interpl()  # Or: <DF>.apply/agg/transform(<trans_func>)
<DF> = <DF>.fillna(<el>)                      # Or: <DF>.applymap(<map_func>)
```
* **All operations operate on columns by default. Use `'axis=1'` parameter to process the rows instead.** 

```python
>>> df = DataFrame([[1, 2], [3, 4]], index=['a', 'b'], columns=['x', 'y'])
   x  y
a  1  2
b  3  4
```

```text
+-------------+-------------+-------------+---------------+
|             |    'sum'    |   ['sum']   | {'x': 'sum'}  |
+-------------+-------------+-------------+---------------+
| df.apply(…) |             |       x  y  |               |
| df.agg(…)   |     x  4    |  sum  4  6  |     x  4      |
|             |     y  6    |             |               |
+-------------+-------------+-------------+---------------+
```

```text
+-------------+-------------+-------------+---------------+
|             |    'rank'   |   ['rank']  | {'x': 'rank'} |
+-------------+-------------+-------------+---------------+
| df.apply(…) |      x  y   |      x    y |        x      |
| df.agg(…)   |   a  1  1   |   rank rank |     a  1      |
| df.trans(…) |   b  2  2   | a    1    1 |     b  2      |
|             |             | b    2    2 |               |
+-------------+-------------+-------------+---------------+
```
* **Use `'<DF>[col_key_1, col_key_2][row_key]'` to get the fifth result's values.**

#### Encode, Decode:
```python
<DF> = pd.read_json/html('<str/path/url>')
<DF> = pd.read_csv/pickle/excel('<path/url>')
<DF> = pd.read_sql('<table_name/query>', <connection>)
<DF> = pd.read_clipboard()
```

```python
<dict> = <DF>.to_dict(['d/l/s/sp/r/i'])
<str>  = <DF>.to_json/html/csv/markdown/latex([<path>])
<DF>.to_pickle/excel(<path>)
<DF>.to_sql('<table_name>', <connection>)
```

### GroupBy
**Object that groups together rows of a dataframe based on the value of the passed column.**

```python
>>> df = DataFrame([[1, 2, 3], [4, 5, 6], [7, 8, 6]], index=list('abc'), columns=list('xyz'))
>>> df.groupby('z').get_group(3)
   x  y
a  1  2
>>> df.groupby('z').get_group(6)
   x  y
b  4  5
c  7  8
```

```python
<GB> = <DF>.groupby(column_key/s)             # DF is split into groups based on passed column.
<DF> = <GB>.get_group(group_key/s)            # Selects a group by value of grouping column.
```

#### Aggregate, Transform, Map:
```python
<DF> = <GB>.sum/max/mean/idxmax/all()         # Or: <GB>.apply/agg(<agg_func>)
<DF> = <GB>.rank/diff/cumsum/ffill()          # Or: <GB>.aggregate(<trans_func>)  
<DF> = <GB>.fillna(<el>)                      # Or: <GB>.transform(<map_func>)
```

```python
>>> gb = df.groupby('z')
      x  y  z
3: a  1  2  3
6: b  4  5  6
   c  7  8  6
```

```text
+-------------+-------------+-------------+-------------+---------------+
|             |    'sum'    |    'rank'   |   ['rank']  | {'x': 'rank'} |
+-------------+-------------+-------------+-------------+---------------+
| gb.agg(…)   |      x   y  |      x  y   |      x    y |        x      |
|             |  z          |   a  1  1   |   rank rank |     a  1      |
|             |  3   1   2  |   b  1  1   | a    1    1 |     b  1      |
|             |  6  11  13  |   c  2  2   | b    1    1 |     c  2      |
|             |             |             | c    2    2 |               |
+-------------+-------------+-------------+-------------+---------------+
| gb.trans(…) |      x   y  |      x  y   |             |               |
|             |  a   1   2  |   a  1  1   |             |               |
|             |  b  11  13  |   b  1  1   |             |               |
|             |  c  11  13  |   c  1  1   |             |               |
+-------------+-------------+-------------+-------------+---------------+
```

### Rolling
**Object for rolling window calculations.**

```python
<R_Sr/R_DF/R_GB> = <Sr/DF/GB>.rolling(window_size)  # Also: `min_periods=None, center=False`.
<R_Sr/R_DF>      = <R_DF/R_GB>[column_key/s]        # Or: <R>.column_key
<Sr/DF/DF>       = <R_Sr/R_DF/R_GB>.sum/max/mean()  # Or: <R>.apply/agg(<agg_func/str>)
```


Plotly
------
```python
# $ pip3 install plotly kaleido
from plotly.express import line
<Figure> = line(<DF>, x=<col_name>, y=<col_name>)        # Or: line(x=<list>, y=<list>)
<Figure>.update_layout(margin=dict(t=0, r=0, b=0, l=0))  # Or: paper_bgcolor='rgba(0, 0, 0, 0)'
<Figure>.write_html/json/image('<path>')                 # Also: <Figure>.show()
```


PySimpleGUI
-----------
```python
# $ pip3 install PySimpleGUI
import PySimpleGUI as sg
layout = [[sg.Text("What's your name?")], [sg.Input()], [sg.Button('Ok')]]
window = sg.Window('Window Title', layout)
event, values = window.read()
print(f'Hello {values[0]}!' if event == 'Ok' else '')
```


Appendix
--------
### Cython
**Library that compiles Python code into C.**

```python
# $ pip3 install cython
import pyximport; pyximport.install()
import <cython_script>
<cython_script>.main()
```

#### Definitions:
* **All `'cdef'` definitions are optional, but they contribute to the speed-up.**
* **Script needs to be saved with a `'pyx'` extension.**

```python
cdef <type> <var_name> = <el>
cdef <type>[n_elements] <var_name> = [<el_1>, <el_2>, ...]
cdef <type/void> <func_name>(<type> <arg_name_1>, ...):
```

```python
cdef class <class_name>:
    cdef public <type> <attr_name>
    def __init__(self, <type> <arg_name>):
        self.<attr_name> = <arg_name>
```

```python
cdef enum <enum_name>: <member_name_1>, <member_name_2>, ...
```

### PyInstaller
```bash
$ pip3 install pyinstaller
$ pyinstaller script.py                        # Compiles into './dist/script' directory.
$ pyinstaller script.py --onefile              # Compiles into './dist/script' console app.
$ pyinstaller script.py --windowed             # Compiles into './dist/script' windowed app.
$ pyinstaller script.py --add-data '<path>:.'  # Adds file to the root of the executable.
```
* **File paths need to be updated to `'os.path.join(sys._MEIPASS, <path>)'`.**

### Basic Script Template
```python
#!/usr/bin/env python3
#
# Usage: .py
#

from sys import argv, exit
from collections import defaultdict, namedtuple
from dataclasses import make_dataclass
from enum import Enum
import functools as ft, itertools as it, operator as op, re


def main():
    pass


###
##  UTIL
#

def read_file(filename):
    with open(filename, encoding='utf-8') as file:
        return file.readlines()


if __name__ == '__main__':
    main()
```

