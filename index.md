---
title: Home
---


# Various topics of interest in bioinformatics  <!-- omit in toc -->

## Table of contents  <!-- omit in toc -->

- [1. Manipulating and analysing DNA sequences](#1-manipulating-and-analysing-dna-sequences)
  - [1.1. Extracting sequences from multifasta files with SeqTk](#11-extracting-sequences-from-multifasta-files-with-seqtk)
  - [1.2. Using BLAST with the command line.](#12-using-blast-with-the-command-line)
  - [1.3. Multiple alignment with MAFFT](#13-multiple-alignment-with-mafft)
- [2. Exploring, editing and subsetting large files in linux](#2-exploring-editing-and-subsetting-large-files-in-linux)
  - [2.1. navigating a file with less](#21-navigating-a-file-with-less)
  - [2.2. Editing: use nano in unix or editor such as vscode](#22-editing-use-nano-in-unix-or-editor-such-as-vscode)
  - [2.3. Selecting lines and columns and choosing field separators with awk](#23-selecting-lines-and-columns-and-choosing-field-separators-with-awk)
  - [3. Notes on BASH](#3-notes-on-bash)


<div style="page-break-after: always;"></div>

## 1. Manipulating and analysing DNA sequences
* We will deal here mostly with sequences in fasta format

### 1.1. Extracting sequences from multifasta files with [SeqTk](notes/tuto_seqtk.md)
* Typically
    * Make a list of the sequence names, edit the list and extract the sequences from the edited list
    * Mxtract specific portions of sequence using lists (bed files) or directly one single portion ta the command line

### 1.2. Using [BLAST](notes/tuto_blast.md) with the command line.
* Make a blast database and use it, to obtain an output in tabular format

### 1.3. Multiple alignment with [MAFFT](notes/tuto_mafft.md)

## 2. Exploring, editing and subsetting large files in linux
### 2.1. navigating a file with [less](notes/Unix_Less_Commands.md)
* The less command in unix allows to navigate large files
* Includes search functions
* Does not allow modifications of the files

### 2.2. Editing: use nano in unix or editor such as vscode
* Windows and others cause compatibility problems. Is Notepad still useable?

### 2.3. Selecting lines and columns and choosing field separators with [awk](notes/AWK.md)

### 3. Notes on [BASH](notes/BASHnotes.md)


Can also include a complete link: [test](https://avignal5.github.io/bioinfo_notes/notes/tuto_mafft.html#documentation)

