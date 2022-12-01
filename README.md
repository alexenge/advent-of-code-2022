Advent of Code 2022
================
Alexander Enge
2022-12-01

- <a href="#hi" id="toc-hi">Hi! 👋</a>
- <a href="#day-1-calorie-counting" id="toc-day-1-calorie-counting">Day 1:
  Calorie Counting 🍕</a>

## Hi! 👋

This repository contains my solutions for the [2022
edition](https://adventofcode.com/2022) of [Advent of
Code](https://adventofcode.com). From the Advent of Code website:

> **Advent of Code** is an [Advent
> calendar](https://en.wikipedia.org/wiki/Advent_calendar) of small
> programming puzzles for a variety of skill sets and skill levels that
> can be solved in any programming language you like. People use them as
> interview prep, company training, university coursework, practice
> problems, a speed contest, or to challenge each other.

I’ll be using a mix of [Python](https://www.python.org), [Base
R](https://www.r-project.org), and [tidyverse-style
R](https://www.tidyverse.org).

## Day 1: Calorie Counting 🍕

### Part one

#### Python

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

#### Base R

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

### Part two

#### Python

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

#### Base R

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
