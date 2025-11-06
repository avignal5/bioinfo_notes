
[Home](../index.md)

# AWK  <!-- omit in toc -->
- [Basic AWK: select lines, columns and choose field separator](#basic-awk-select-lines-columns-and-choose-field-separator)
  - [Select one or several columns](#select-one-or-several-columns)
  - [Select columns and lines on the basis of values](#select-columns-and-lines-on-the-basis-of-values)
  - [Output separated by tabulations: "\\t"](#output-separated-by-tabulations-t)
  - [Add a column with some text](#add-a-column-with-some-text)
  - [Print all columns](#print-all-columns)
  - [lines where column 7 is not "NO"](#lines-where-column-7-is-not-no)
  - [Multiple column selection](#multiple-column-selection)
  - [Selection while keeping a header](#selection-while-keeping-a-header)
  - [Selection while keeping a header with sorting](#selection-while-keeping-a-header-with-sorting)
  - [Using different field separators](#using-different-field-separators)

# Basic AWK: select lines, columns and choose field separator

* Created a few tables, such as "table1.txt for the examples

```bash
more /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    1       3       6       3       8       4       OK
    5       6       7       1       9       0       NO
    4       5       2       8       9       3       OK

## Select one or several columns
```bash
awk '{print $2, $4, $7}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    3 3 OK
    6 1 NO
    5 8 OK


## Select columns and lines on the basis of values

```bash
# Columns 1, 2 and 6 if column 5 equals to 5
awk '$5 == 9{print $1, $2, $6}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    5 6 0
    4 5 3

## Output separated by tabulations: "\t"
```bash
# Same, but output OFS separated by tabulations: "\t"
awk 'BEGIN{OFS="\t"}$5 == 9{print$1,$2,$6}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    5	6	0
    4	5	3

## Add a column with some text
```bash
# Add a column with some text
awk 'BEGIN{OFS="\t"} $5 == 9{print$1,$2,"text",$6}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    5	6	text	0
    4	5	text	3

## Print all columns

```bash
# print all columns
awk '$5 == 9{print}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    5	6	7	1	9	0	NO
    4	5	2	8	9	3	OK


## lines where column 7 is not "NO"

```bash
# lines where column 7 is not "NO"
awk 'BEGIN{OFS="\t"} $7 != "NO" {print $1,$2}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    1	3
    4	5


## Multiple column selection
```bash
# Multiple column selection
awk 'BEGIN{OFS="\t"} $7 != "NO" && $2 == 3 {print $1,$2}' /Users/avignal/Documents/Documentations/BASH/Directory/table1.txt
```

    1	3


## Selection while keeping a header


```bash
more /Users/avignal/Documents/Documentations/BASH/Directory/table2.txt
```

    One     Two     Three   Four    Five    Six     Seven
    1       3       6       3       8       4       OK
    5       6       7       1       9       0       NO
    4       5       2       8       9       3       OK
    [K[?1l>/avignal/Documents/Documentations/BASH/Directory/table2.txt (END)[27m[K


```bash
awk 'BEGIN{OFS="\t"} NR == 1;NR > 1 {if($7 == "OK") {print}}' /Users/avignal/Documents/Documentations/BASH/Directory/table2.txt
```

    One	Two	Three	Four	Five	Six	Seven
    1	3	6	3	8	4	OK
    4	5	2	8	9	3	OK


## Selection while keeping a header with sorting


```bash
awk 'BEGIN{OFS="\t"} NR == 1;NR > 1 {if($7 == "OK") {print | "sort -k 3"}}' /Users/avignal/Documents/Documentations/BASH/Directory/table2.txt
```

    One	Two	Three	Four	Five	Six	Seven
    4	5	2	8	9	3	OK
    1	3	6	3	8	4	OK


## Using different field separators
* FS = input file separator
* OFS = output file separator


```bash
more /Users/avignal/Documents/Documentations/BASH/Directory/list.txt

```

    One_Two_Three_Four
    Five_Six_Seven_Eigh
    Nine_Ten_Eleven_Twelve


```bash
awk 'BEGIN{FS="_";OFS=";"}{print $2,$4}' /Users/avignal/Documents/Documentations/BASH/Directory/list.txt
```

    Two;Four
    Six;Eigh
    Ten;Twelve



```bash
awk 'BEGIN{FS="_";OFS="\t"}{print $2,$4}' /Users/avignal/Documents/Documentations/BASH/Directory/list.txt
```

    Two	Four
    Six	Eigh
    Ten	Twelve


