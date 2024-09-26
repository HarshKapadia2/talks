# ELF

## Introduction

-   ELF files are the primary executable files on Linux. This talk explores the ELF format, how they are stored on disk and loaded in memory for execution, and how one can inspect files on Linux. We will also see a rudimentary ELF parser that I built in C.
-   Duration: TBD
-   Slide deck: https://docs.google.com/presentation/d/1eg9I7QjBU7qpKB5SjjtL9XVD_ZLqB6XsTiiIGP2uZms/edit?usp=sharing

## Motivation

Why should one learn about ELFs?

-   Most commonly used executable format on Linux-based OSs
    -   What are some other executable formats on Linux?
-   Helps in getting comfortable with working with files.
-   Helps in understanding how programs are loaded into memory and executed.

## Goal

-   Understand the ELF format.
-   Get comfortable with examining files.
-   Get more comfortable with C.
-   Understand the memory model of files and how they are loaded into memory.
-   Learn how the compiler understands the dynamic libraries to be linked.

## High-Level Agenda

-   ELF format
-   ELF parser
-   Examining ELF files
-   Building an ELF file

## ELF Format

> We will only talk about the 64-bit ELF format and not the 32-bit ELF format.

-   Section vs Segment
    -   Sections describe files from a linking/static perspective for the Linker, while segments describe the file from a runtime/execution view for the OS.
    -   Each byte in a file can belong to zero or one section.
    -   A segment can contain zero or more sections.
-   File Header
    -   Optional?
-   Segment (Program) Header
    -   Optional?
-   Section Header
    -   Optional?
-   Dynamic linking
    -   Archive (`.a`) vs Shared Object (`.so`) files

### Sections and Segments

#### Need for Sections in ELFs

They describe different types of areas in an ELF that are used for different purposes. Each section contains data for a particular function. They help the linker understand

#### Need for Segments in ELFs

Memory is divided into segments. (Why?)

A program is divided into segments, so that they can fit into memory segments. (Are these physical memory segments or virtual memory segments?)

## ELF Demos

-   ELF parser: [github.com/HarshKapadia2/parse-elf](https://github.com/HarshKapadia2/parse-elf)
-   Read-only data string literal demo
-   Extracting passwords from binaries

## Examining ELF Files

-   `size -d`

## Building an ELF File

## Resources

-   [Becoming an Elf-Lord](https://cpu.land/becoming-an-elf-lord)
-   [In-depth: ELF - The Extensible & Linkable Format](https://www.youtube.com/watch?v=nC1U1LJQL8o)
-   [Wikipedia: Executable and Linkable Format](https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)
-   Section Header vs Segment (Program) Header
    -   [What's the difference of section and segment in ELF file format](https://stackoverflow.com/questions/14361248/whats-the-difference-of-section-and-segment-in-elf-file-format)
    -   [Difference between Program header and Section Header in ELF](https://stackoverflow.com/questions/23379880/difference-between-program-header-and-section-header-in-elf)
    -   [Inside Specs: ELF Segments and Sections](https://dvdhrm.github.io/2020/04/26/inside-specs-elf-segments-and-sections)
    -   [System V ABI for AMD64 v0.99.6](https://refspecs.linuxbase.org/elf/x86_64-abi-0.99.pdf)
        -   Section 5.1 (Program Loading) first paragraph
-   Background
    -   [Memory Layout and Compilers](https://linux.harshkapadia.me/#memory-layout-and-compilers)
-   ELF utilities
    -   [How to Inspect Compiled Binaries (`binutils`, `objdump`)](https://www.youtube.com/watch?v=bWMIpHVRFUo)
