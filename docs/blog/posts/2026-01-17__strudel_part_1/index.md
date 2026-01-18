---
date:
  created: 2026-01-17
  updated: 2026-01-17

draft: False

tags:
  - strudel

links:
  - Strudel Home: https://strudel.cc/
---

# Strudel Tutorial - Part 1

Coding music is fun!

<!-- more -->

[Strudel](https://strudel.cc/) is a tool to make music using JavaScript. You can use to to explore melodies or drum pattern in a coding sort of environment called [REPL](https://strudel.cc/). But that is just the beginning. Many people use it in live-coding settings called [algorave](https://en.wikipedia.org/wiki/Algorave).

Here is a very good example [Strudeldudel](https://strudel.cc/?qVv8Cr0OD6cc){: target="_blank" }. Just hit the play button on the upper right. 


## A basic drum pattern

Let's take [Abelton's making music tutorial](https://learningmusic.ableton.com/make-beats/what-are-these-sounds.html) and try to replicate the drum pattern.

There are four different sequences in this pattern. Each has a total of 16 notes.

A very first and verbose strudel version would look like this:

```
setcpm(85/4) // make 85 beats per minute (bpm)

let drum = "RolandTR808"

$: s("- - - -  - - - -  - - - -  - - oh -").bank(drum)
$: s("hh hh hh  hh hh hh hh  hh hh hh  hh hh hh hh - -").bank(drum)
$: s("- - - -  cp - - -  - - - -  cp - - -").bank(drum)
$: s("bd bd - -  - - bd -  bd bd - bd  - - - -").bank(drum)
```
Each of a the `$` functions will run in parallel.

The two letter acronyms refer to pieces in a drum set. Here is a list:

```
bd = bass drum
sd = snare drum
rim = rimshot
hh = hihat
oh = open hihat
lt = low tom
mt = middle tom
ht = high tom
rd = ride cymbal
cr = crash cymbal
cp = clap
```

A more compact version would just `stack` up the sequences. In addition, instead of writing out each beat we can just us the `beat` function to tell strudel what when to play.
```
setcpm(85/4) // make 85 beats per minute (bpm)

let drum = "RolandTR808"

$: stack(s("oh").beat("14",16)
      , s("hh").beat("0,1,2,3,4,5,6,7,8,9,10,11,12,13",16)
      , s("cp").beat("4, 12",16)
      , s("bd").beat("0,1,6,8,9,11",16)
     ).bank(drum)
```

Now, I guess, the "official" strudel version would rather look like this. It is pretty cryptic and harder to read in the beginning. I hope this will ease up over time.

```
setcpm(85/4) // make 85 beats per minute (bpm)

let drum = "RolandTR808"

$: stack(
  s("~@14 oh ~"),                // Open hi-hat (delayed to step 15)
  s("hh!14 ~ ~"),                // Hi-hats (14 hits, 2 rests)
  s("~ cp ~ cp"),                // Claps (on beats 2 and 4)
  s("[bd!2 ~ ~] [~ ~ bd ~] [bd!2 ~ bd] ~") // Kick pattern in 4 subsequences.
).bank(drum)
```

- `~@14` - Extends the rest to last for 14 steps.
- `hh!14` - Repeat hh 14 times


## Keyboard shortcuts

It took me forever to figure out how to use the keyboard to `start`, `update`, and `stop` the music. Many website say on MacOS:

- `Cmd` + `Enter` to start and update
- `Cmd` + `.` to stop

But it actually is:

- `Ctrl` + `Enter` to start and update
- `Ctrl` + `.` to stop

At least on my macbook.
