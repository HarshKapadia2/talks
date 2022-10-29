# Git Internals Agenda

In this session, we will be exploring the internals of Git and understanding how it goes about efficiently versioning software.

## Agenda

Why do millions of people use Git to version their code? It's because of its features, convenience, structure and design! This talk explores what we can learn from the structure and design of Git to write better software and use these principles in practice to efficiently manage, store, transfer and query data. We will broadly talk about Packfiles and efficiently handling CRUD operations on files.

The following questions will be answered in this session

-   The `.git` directory
    -   Which files and directories does the `.git` directory contain?
    -   What is the use and structure of those files and directories?
-   Git Objects
    -   What are Blob, Tree and Commit objects in Git?
    -   What is the use and structure of these objects?
-   How does Git efficiently manage the following?
    -   Different versions of files
    -   Renaming of files
    -   Directories
    -   Changes to large files (Packfiles)

> NOTE
>
> -   This session assumes a basic understanding of Git.
> -   Concepts and tools such as Rebasing and Git LFS will not be covered.

## Details

### Introduction

-   Who TF is HK?
-   What is going to be done?
    -   No need to write or note anything.
-   Why is it imp to know the internals?
    -   Understanding of what is going on improves usage.
    -   Improves logic and system design understanding. (Not like CP.)

### The `.git` Directory

#### Summary

-   Explore the most important files and directories in the `.git` directory.
-   Give reasons for the need of each file and directory and explain the structure of their contents.

#### Details

-   Init a repo in dir A and explore the new `.git` dir.
-   Init a repo in dir B and add a file to show changes.
    -   `index` file
    -   `objects` dir
        -   Explain bucketing, but don't show contents.
-   Commit the added file in repo B and show changes.
    -   `objects` dir
    -   `COMMIT_EDITMSG` file
    -   `HEAD` file
    -   `refs/heads` dir
-   Show remaining files using ['The `.git` Directory Contents' section](https://git.harshkapadia.me/#_the_git_directory_contents) of Git Internals as ref, from the `git_basics` local repo or any other repo.
-   QnA

### Git Objects

#### Summary

-   Show and explain the contents of Blob, Commit and Tree Objects.
-   Through an example, create a Directed Acyclic graph to show how Git connects these three objects together to manage different versions of files.
-   Showcase how efficient Git is
    -   At handling different versions of files and renaming of files.
    -   In storing versions of huge files.

#### Details

-   Introduce Git Objects.
-   In the dir A repo, add a file to show changes.
    -   Get into the `objects` dir and then show the contents of the added `blob` file.
-   Add content to the added file.
-   Create graph.
    -   Add the two blob objects.
-   Commit file and show changes.
    -   Get into the `objects` dir and then show the contents of the added `commit` and `tree` files.
    -   Show the `HEAD` file and the `refs/heads/main` file contents.
-   Update graph.
    -   Add HEAD, commit and tree.
-   QnA
-   Add another file and commit it to showcase the parent commit.
    -   Showcase the blob, commit and tree objects.
    -   In the commit object, showcase the parent commit line.
-   Update graph
    -   Add commit, tree and blob
    -   Update HEAD.
-   QnA
-   Create a dir, add a file in that dir and commit it.
    -   Showcase the blob, commit and tree objects.
    -   Note the tree object in the tree object and print the contents of that tree as well. Get into the blob in the new tree as well.
-   Construct tree graph.
-   Update main graph.
    -   Add commit, trees and blobs
    -   Update HEAD.
-   QnA
-   Rename the first file that was created, add it and commit it to showcase that Git is smart enough to understand that the file has only been renamed.
    -   Showcase the commit and tree objects.
-   Update graph.
    -   Add commit and tree.
    -   Update blob file name and HEAD.
-   QnA
-   Add a pic and commit it.
    -   Showcase the blob, commit and tree objects.
-   Make a small change to the contents using Vim, add and commit.
    -   Showcase the blob, commit and tree objects.
-   Run `du -c` to check the size of the two large image blob objects and raise space efficiency question.
-   Pack file
    -   Delta compression
        -   Takes place when code is pushed/pulled to/from GitHub or when aggressive garbage collection is carried out.
    -   Run `git gc --aggressive` and then run `du -c` to showcase the state of the repo.
    -   Run `git verify-pack -v *.pack` in `.git/objects/pack` and showcase the contents of the pack file.
        -   Point out the older img blob obj having the newer one as the parent.
        -   Efficient storage!
    -   What is the `.idx` index file.
        -   Print contents using `git verify-pack -v *.idx` in `.git/objects/pack`.
-   Show log of final repo.
-   Summarise everything done in the sub-section
-   QnA

### Conclusion

-   Git is amazing!
-   Go over all the topics covered.
-   Give a concise round up of all the things learnt and takeaways.
-   QnA

## The Short Version

> This version should take around 1 hour to complete.

-   What is Git?
-   What is going to be done?
    -   No need to write or note anything.
-   Why is it imp to know the internals?
    -   Understanding of what is going on improves usage.
    -   Improves logic and system design understanding.
-   Prerequisite basic commands/concepts (with picture)
    -   Adding/Staging
    -   Committing
    -   Branching
-   Init repo
-   Show the contents of the `.git/objects` directory.
-   Create two files, add both files and make a single commit.
-   Use Git Graph on this repo.
-   Show the contents of the `.git/objects` directory again.
-   Show raw file contents of Commit (using `zlib` and then Git Graph), Tree and Blob objects.
-   Talk about `HEAD`.
-   Modify one of the two file and see how the graph changes.
-   Create a directory, create a file in it, add it and then commit it.
-   Use Git Graph on this repo.
-   \[OPTIONAL\] Branching
    -   Create a new branch and make a few commits on it. Switch on Git Graph and show how head is working.
-   \[OPTIONAL\] Concept of Packfiles
    -   All objs in one file
    -   Only new stored and old diffed
-   Learnings
    -   Some understanding of how Git works to improve usage.
    -   Understanding of how software is designed with efficiency in mind, to optimize for storage and bandwidth.
-   Urge people to look more into software and even Git.
    -   Show the `.git` directory files using ['The `.git` Directory Contents' section](https://git.harshkapadia.me/#_the_git_directory_contents) of Git Internals.
