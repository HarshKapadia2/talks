# ELF

## Talk Details

-   Abstract: Executable and Linkable Format (ELF) files are the primary executable files on Linux. This talk explores the format defined by ELF, the two views of an ELF file (the Execution View and the Linking View), and how one can inspect files on Linux. Additionally, we will see a rudimentary ELF parser that I built in C and explore all the compilation steps (Preprocessing, Compilation, Assembling and Linking) to generate an ELF file.
-   Duration: 1.5 hours, but can be modified depending on the available time.
-   ❗ [**Detailed article format of talk**](content.md)
    -   The [Table of Contents](content.md#table-of-contents) is the talk agenda.
-   [Slide deck](https://docs.google.com/presentation/d/1eg9I7QjBU7qpKB5SjjtL9XVD_ZLqB6XsTiiIGP2uZms/edit?usp=sharing)
-   [parse-elf](https://github.com/HarshKapadia2/parse-elf)
-   [compilation-examples](https://github.com/HarshKapadia2/compilation-examples)

## Motivation

Why should one learn about ELFs, process memory layout, compilation steps and file utilities?

-   ELF is the standard executable file format on Linux-based OSs.
-   Fundamentals
    -   Learning about files, process memory layout and compilation steps are fundamental Operating Systems concepts.
    -   Knowing more about the fundamentals makes it easier to build better software because of the additional context one gets.
-   Cybersecurity
    -   To carry out or defend against binary exploitation, it is important to know
        -   How information is structured. (ELF file format and its views.)
        -   How to examine files to check for issues and figure out details about the binary/executable. (File utilities)
-   Debugging
    -   Learning about the basics of Symbols helps understand how Debuggers (like `gdb`) are able to work with executables.

## Past Talks

-   [All About ELFs](https://www.youtube.com/watch?v=BM62xi4FE3c) at [OTC Talks #6](https://talks.ourtech.community/6) (September 28, 2024)
