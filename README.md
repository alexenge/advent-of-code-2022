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
- <a href="#day-4-camp-cleanup-broom"
  id="toc-day-4-camp-cleanup-broom">Day 4: Camp Cleanup :broom:</a>
- <a href="#day-5-supply-stacks-building_construction"
  id="toc-day-5-supply-stacks-building_construction">Day 5: Supply Stacks
  :building_construction:</a>
- <a href="#day-6-tuning-trouble-radio"
  id="toc-day-6-tuning-trouble-radio">Day 6: Tuning Trouble :radio:</a>

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

## Day 4: Camp Cleanup :broom:

### Part one: Python

``` python
def is_contained(min_1, max_1, min_2, max_2):
    return (min_1 >= min_2 and max_1 <= max_2 or
            min_2 >= min_1 and max_2 <= max_1)


contained = 0
with open('data/day4.txt') as f:
    for line in f:
        elf_1, elf_2 = line.strip().split(',')
        min_1, max_1 = [int(section) for section in elf_1.split('-')]
        min_2, max_2 = [int(section) for section in elf_2.split('-')]
        contained += is_contained(min_1, max_1, min_2, max_2)

print(contained)
```

    475

### Part one: Tidyverse R

``` r
read_csv("data/day4.txt", col_names = c("elf_1", "elf_2")) %>%
    transmute(across(.fns = str_split, pattern = "-")) %>%
    unnest_wider(c(elf_1, elf_2), names_sep = "_", transform = as.integer) %>%
    mutate(
        contained_1 = elf_1_1 >= elf_2_1 & elf_1_2 <= elf_2_2,
        contained_2 = elf_2_1 >= elf_1_1 & elf_2_2 <= elf_1_2,
        contained = contained_1 | contained_2
    ) %>%
    pull(contained) %>%
    sum()
```

    [1] 475

### Part two: Python

``` python
def is_overlap(min_1, max_1, min_2, max_2):
    return (max_1 >= min_2 and max_2 >= min_1)

overlaps = 0
with open('data/day4.txt') as f:
    for line in f:
        elf_1, elf_2 = line.strip().split(',')
        min_1, max_1 = [int(section) for section in elf_1.split('-')]
        min_2, max_2 = [int(section) for section in elf_2.split('-')]
        overlaps += is_overlap(min_1, max_1, min_2, max_2)

print(overlaps)
```

    825

### Part two: Tidyverse R

``` r
read_csv("data/day4.txt", col_names = c("elf_1", "elf_2")) %>%
    transmute(across(.fns = str_split, pattern = "-")) %>%
    unnest_wider(c(elf_1, elf_2), names_sep = "_", transform = as.integer) %>%
    mutate(overlap = elf_1_2 >= elf_2_1 & elf_2_2 >= elf_1_1) %>%
    pull(overlap) %>%
    sum()
```

    [1] 825

## Day 5: Supply Stacks :building_construction:

### Part one: Base R

``` r
transpose_list <- function(l) as.list(as.data.frame(t(as.data.frame(l))))
drop_empty <- function(x) x[x != " "]

input <- read.csv("data/day5.txt", header = FALSE)

input_1 <- subset(input, grepl("\\[", V1))$V1
stack_ixs <- seq(2, max(nchar(input_1)), by = 4)
stacks <- lapply(input_1, substring, first = stack_ixs, last = stack_ixs) |>
    rev() |>
    transpose_list() |>
    lapply(drop_empty)

input_2 <- subset(input, grepl("move", V1))$V1
moves <- strsplit(input_2, split = " ")
for (move in moves) {
    from = as.integer(move[4])
    to = as.integer(move[6])
    n = as.integer(move[2])
    stacks[[to]] <- c(stacks[[to]], rev(tail(stacks[[from]], n)))
    stacks[[from]] <- head(stacks[[from]], -n)
}

lapply(stacks, tail, n = 1) |>
    paste(collapse = "")
```

    [1] "QNNTGTPFN"

### Part two: Base R

``` r
stacks <- lapply(input_1, substring, first = stack_ixs, last = stack_ixs) |>
    rev() |>
    transpose_list() |>
    lapply(drop_empty) # Same as before

for (move in moves) {
    from = as.integer(move[4])
    to = as.integer(move[6])
    n = as.integer(move[2])
    stacks[[to]] <- c(stacks[[to]], tail(stacks[[from]], n)) # Don't reverse
    stacks[[from]] <- head(stacks[[from]], -n)
}

lapply(stacks, tail, n = 1) |>
    paste(collapse = "")
```

    [1] "GGNPJBTTR"

## Day 6: Tuning Trouble :radio:

### Part one: Python

``` python
def find_marker(input, n_letters):
    for ix in range(len(input)):
        letters = input[ix:ix + n_letters]
        if len(letters) == len(set(letters)):
            print(ix + n_letters) # Print index of *last* marker letter
            break


with open('data/day6.txt') as f:
    input = f.readlines()[0]

find_marker(input, n_letters=4)
```

    1802

### Part two: Python

``` python
find_marker(input, n_letters=14)
```

    3551
