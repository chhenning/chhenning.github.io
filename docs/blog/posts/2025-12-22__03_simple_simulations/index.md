---
date:
  created: 2025-12-22
  updated: 2024-12-22

draft: False

categories:
  - Simulation

tags:
  - python
---


# A collection of simple simulations in Python

Python makes it very easy to create simple simulations to simulate complex processes. There is usually a math solution but that can be hard to remember.

In the spirit of [Raymond Hettinger's](https://github.com/rhettinger){: target="_blank" } [Modern Python LiveLessons: Big Ideas and Little Code in Python](https://www.oreilly.com/videos/modern-python-livelessons/9780134743400/){: target="_blank" } course. We will explore some basic simulation.

<!-- more -->

## Streak Length History of an American Roulette Wheel

The American Roulette has 18 black and red pockets plus TWO green pockets. Making it a total of 38 pockets.

To simulate this whell we just need one line of Python code. In the following code the roulette wheel is spun 1,000 times

```py
from random import *
from collections import *

Counter(choices(['black', 'red', 'green'], weights=[18,18,2], k=1000))
```

The hit counts are below. Red was hit 476 times (~48%). The two greens were only hit 53 times (5.3%) which makes sense since 2/38 = 5.2%

```py
Counter({'red': 476, 'black': 472, 'green': 52})
```

Now, how often do we hit a streak of any color? The following code does just that:

```py
# spin the wheel 100,000 times
spins = choices(['black', 'red', 'green'], weights=[18,18,2], k=100_000)

def streak_lengths(spins):
    streaks = []
    current = spins[0]
    length = 1

    for s in spins[1:]:
        if s == current:
            length += 1
        else:
            streaks.append((current, length))
            current = s
            length = 1

    streaks.append((current, length))
    return streaks


streaks = streak_lengths(spins)

# we are only interested in streaks of longer than 4 spins
streaks_counter = Counter([s[1] for s in streaks if s[1] > 4])
print(streaks_counter)
```

The output is:

```py
Counter({5: 1296, 6: 641, 7: 280, 8: 150, 9: 61, 10: 27, 11: 14, 12: 10, 13: 2, 15: 1, 14: 1})
```

As you can see very long streaks are indeed possible albeit very rarely.


