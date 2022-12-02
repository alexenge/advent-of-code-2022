Advent of Code 2022
================
Alexander Enge
2022-12-01

- <a href="#day-1-calorie-counting" id="toc-day-1-calorie-counting">Day 1:
  Calorie Counting 🍕</a>
- <a href="#day-2-rock-paper-scissors"
  id="toc-day-2-rock-paper-scissors">Day 2: Rock Paper Scissors ✂️</a>

Hi! 👋 This repository contains my solutions for the [2022
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

## Day 2: Rock Paper Scissors ✂️

### Part one: Tidyverse Rttin

``` r
library(tidyverse)
```

    ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ✔ tidyr   1.2.0      ✔ stringr 1.4.1 
    ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()

``` r
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


    ── Column specification ────────────────────────────────────────────────────────
    cols(
      other = col_character(),
      me = col_character()
    )

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


    ── Column specification ────────────────────────────────────────────────────────
    cols(
      other = col_character(),
      outcome = col_character()
    )

    [1] 10498
