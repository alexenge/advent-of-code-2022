Advent of Code 2022
================
Alexander Enge
2022-12-01

- <a href="#day-1-calorie-counting-pizza"
  id="toc-day-1-calorie-counting-pizza">Day 1: Calorie Counting
  :pizza:</a>
- <a href="#day-2-rock-paper-scissors-scissors"
  id="toc-day-2-rock-paper-scissors-scissors">Day 2: Rock Paper Scissors
  :scissors:</a>
- <a href="#day-3-rucksack-reorganization-school_satchel"
  id="toc-day-3-rucksack-reorganization-school_satchel">Day 3: Rucksack
  Reorganization :school_satchel:</a>

Hi! :wave:

This repository contains my solutions for the [2022
edition](https://adventofcode.com/2022) of [Advent of
Code](https://adventofcode.com).

From the Advent of Code website:

> **Advent of Code** is an [Advent
> calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small
> programming puzzles for a variety of skill sets and skill levels that
> can be solved in any programming language you like. People use them as
> interview prep, company training, university coursework, practice
> problems, a speed contest, or to challenge each other.

I’ll be using a mix of [Python](https://www.python.org), [Base
R](https://www.r-project.org), and [tidyverse-style
R](https://www.tidyverse.org).

## Day 1: Calorie Counting :pizza:

### Part one: Python

``` python
max_elf = 0
this_elf = 0

with open('data/day1.txt') as f:
    for line in f:

        if this_elf > max_elf:
            max_elf = this_elf

        if line == '\n':
            this_elf = 0
        else:
            this_elf += int(line.strip())

print(max_elf)
```

    64929

### Part one: Base R

``` r
lines <- readLines("data/day1.txt", warn = FALSE)

elf_indices <- cumsum(lines == "")

split(lines, elf_indices) |>
    lapply(as.numeric) |>
    lapply(sum, na.rm = TRUE) |>
    unlist() |>
    max()
```

    [1] 64929

### Part two: Python

``` python
max_elves = [0, 0, 0]
this_elf = 0

with open('data/day1.txt') as f:
    for line in f:

        max_elves.sort()
        if this_elf > max_elves[0]:
            max_elves[0] = this_elf

        if line == '\n':
            this_elf = 0
        else:
            this_elf += int(line.strip())

print(sum(max_elves))
```

    193697

### Part two: Base R

``` r
lines <- readLines("data/day1.txt", warn = FALSE)

elf_indices <- cumsum(lines == "")

split(lines, elf_indices) |>
    lapply(as.numeric) |>
    lapply(sum, na.rm = TRUE) |>
    unlist() |>
    sort() |>
    tail(3) |>
    sum()
```

    [1] 193697

## Day 2: Rock Paper Scissors :scissors:

### Part one: Tidyverse R

``` r
library(tidyverse)

read_table("data/day2.txt", col_names = c("other", "me")) %>%
    unite(col = "round", other, me, remove = FALSE) %>%
    mutate(
        game_score = case_when(
            round %in% c("A_Y", "B_Z", "C_X") ~ 6,
            round %in% c("A_X", "B_Y", "C_Z") ~ 3,
            round %in% c("A_Z", "B_X", "C_Y") ~ 0
        ),
        play_score = recode(me, "X" = 1, "Y" = 2, "Z" = 3),
        score = game_score + play_score
    ) %>%
    pull(score) %>%
    sum()
```

    [1] 14297

### Part two: Tidyverse R

``` r
read_table("data/day2.txt", col_names = c("other", "outcome")) %>%
    unite(col = "round", other, outcome, remove = FALSE) %>%
    mutate(
        me = case_when(
            round %in% c("A_X", "B_Z", "C_Y") ~ "Z",
            round %in% c("A_Y", "B_X", "C_Z") ~ "X",
            round %in% c("A_Z", "B_Y", "C_X") ~ "Y"
        ),
        game_score = recode(outcome, "X" = 0, "Y" = 3, "Z" = 6),
        play_score = recode(me, "X" = 1, "Y" = 2, "Z" = 3),
        score = game_score + play_score
    ) %>%
    pull(score) %>%
    sum()
```

    [1] 10498

## Day 3: Rucksack Reorganization :school_satchel:

### Part one: Python

``` python
from string import ascii_lowercase, ascii_uppercase

with open('data/day3.txt') as f:
    lines = [line.strip() for line in f.readlines()]

halves = [(line[:len(line) // 2], line[len(line) // 2:]) for line in lines]
items = [set(half_1).intersection(half_2).pop() for half_1, half_2 in halves]

letters = ascii_lowercase + ascii_uppercase
priorities = [letters.index(item) + 1 for item in items]
print(sum(priorities))
```

    7795

### Part two: Python

``` python
group_size = 3
groups = [lines[ix:ix + group_size] for ix in range(0, len(lines), group_size)]

badges = [set.intersection(*map(set, group)).pop() for group in groups]
priorities = [letters.index(badge) + 1 for badge in badges]
print(sum(priorities))
```

    2703
