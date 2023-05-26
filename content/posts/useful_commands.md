---
title: "Useful Commands"
date: 2023-05-20T07:42:17-05:00
draft: false
---

This is a non-exhaustive list of useful commands in Ubuntu. I include useful notes on their behavior that are often glossed over. I only list commands that I find are actually useful in practice, so I exclude commands like `ls -a`. This post already assumes you understand the basics of shell usage, like `sudo`.

## Documentation

- `cmd --help` print help file
- `cmd --help | less` paginate help file
- `man cmd` read man page
- `which cmd` print file path of `cmd`\; **prints nothing for aliases**

## Running stuff

Each of these commands has **subtle differences** that should be understood.

- `exec > file; ...; exit;` redirect all stdout in `...` into `file`
- `exec file` execute `file` replacing current shell process
- `source script` execute `script` in current shell
- `bash script` execute `script` in child shell process

## Useful aliases and functions

- `function hl { $1 --help | less; }` read the help of the supplied command interactively
- `alias la="ls -AF"` I almost never use `ls` by itself because I usually WANT to see the dotfiles.
- `alias ll="ls -lAF"` If I'm using list format, I want to see everything.

## Filesize Units

Because engineers hate each other, file sizes are often reported in powers of 1024 instead of powers of 1000, causing unnecessary confusion. That is, `1KB` is often taken to mean 1 kibibyte (1024 bytes), `1MB` *sometimes* means 1 mebibyte (1024^2 bytes), etc. The best part? The relative error is worse for larger file sizes, meaning the confusion caused by this is *getting worse over time*.

| Decimal Unit | Binary Unit | Relative Error in Binary Unit |
| --- | --- | --- | --- |
| 1 kB = 1000 bytes    | 1 KiB = 1 K = 1024 bytes    | 2.4%
| 1 MB = 1000^2 bytes  | 1 MiB = 1 M = 1024^2 bytes  | 4.9%
| 1 GB = 1000^3 bytes  | 1 GiB = 1 G = 1024^3 bytes  | 7.4%
| 1 TB = 1000^4 bytes  | 1 TiB = 1 T = 1024^4 bytes  | 10.0%
| 1 PB = 1000^5 bytes  | 1 PiB = 1 P = 1024^5 bytes  | 12.6%
| 1 EB = 1000^6 bytes  | 1 EiB = 1 E = 1024^6 bytes  | 15.3%
| 1 ZB = 1000^7 bytes  | 1 ZiB = 1 Z = 1024^7 bytes  | 18.1%
| 1 YB = 1000^8 bytes  | 1 YiB = 1 Y = 1024^8 bytes  | 20.9%
| 1 RB = 1000^9 bytes  | 1 RiB = 1 R = 1024^9 bytes  | 23.8%
| 1 QB = 1000^10 bytes | 1 QiB = 1 Q = 1024^10 bytes | 26.8%

Luckily, many linux utilities give you the option of reporting file sizes in either system.

- `du -h` will report file sizes in human-readable units (powers of 1024).
- `du --si` will report file sizes in human-readable units (powers of 1000).
- `df -h` will report file sizes in human-readable units (powers of 1024).
- `df -H` will report file sizes in human-readable units (powers of 1000).
- `ls -h` will report file sizes

In general, engineers should prefer to be explicit but efficient, so if I were re-designing software from the ground up, I would use the following guidelines:

- By default, report data sizes in human-readable powers of 1000, using kB, MB, GB, etc.
- Use `-b`, or `file_size_format="binary"` to report file sizes in human-readable powers of 1024, using KiB, MiB, GiB, etc.
- Use `--bytes` or `file_size_format="bytes"` to report file sizes in bytes, with no grouping, for ease of parsing. This is not default because the default should be human-readability. This is because commands intended for machines only need to be written once, in a script file, whereas commands intended for humans are written often.

As it stands, one should probably implement `-h`, `-H`, and `--si` in command-line applications.

## File Manipulation

- `pwd` Print Working Directory
- `cd dir` Change Directory
- `ls` List directory contents
    - `ls -A` almost all directory contents, excluding implied `.` and `..`
    - `ls -l` list format, with more information
    - `ls -F` append type indicator to files
    - `ls -l file` show details of file
    - `ls dir` show contents of dir
    - `ls -d dir` list directory itself, instead of contents
    - `ls -h` human-readable file sizes in powers of two (K, M, G)
- `tree` helpfully display a tree of the current file structure
    - `tree -d` display only directories
    - `tree -L 3` display a tree of three levels maximum
- `exa` is a really great replacement for `ls` and `tree` that has sensible defaults and some useful options
    - `exa --group-directories-first -h` group directories first, and show header for list view. I alias this as `exa`.
    - By default `exa` lists file sizes with human-readable si prefixes
        - `-b` for binary prefixes
        - `-B` for bytes
    - `exa -DT` show directory tree
    - `exa -DTL3` show directory tree of depth 3
- `mv src dest` attempt to move `src` to `dest/src` if `dest` is a directory, otherwise rename `src` to `dir`, **removing `dir`**
    - `mv file dir/.` attempt to move `src` to `dir/src`, failing if `dir` is not a directory
- `cp src dest` attempt to copy `src` to `dest/src` if `dest` is a directory, otherwise overwrite `dest` with `src`
    - `cp src dir/.` attempt to copy `src` to `dir/src`, failing if `dir` is not a directory
- `rm file` remove `file`
    - `rmdir empty_dir` remove an empty directory
    - `rm -r dir` remove `dir` and all sub-directories\; in general, don't use `-f`

## Permissions

Permissions are kinda weird. There are a lot of concepts to keep straight. If you `ls -l file`, you will see the following:

```
-rw-rw-r-- 1 owner group size modified file
```

The first dash represents the type of file it is. It may take on any of the following:

- `-` regular file
- `d` directory
- `l` symlink
- There are others

The next nine characters are the permissions of the file. They are split into groups of three. Each group can be `rwx`, or dashes. `r--` means that group has only read access. `r-x` means that group has read and execute permissions. The first group of three is what the owner can do. The second group is what the group can do. The third group of three is what anyone other than the owner and group can do.

Permissions may also be specified in binary notation. `111` means the user has read, write, and execute permission. This is more commonly specified in octal as `7`.

| String | Octal |
| --- | --- |
| `---` | 0 |
| `--x` | 1 |
| `-w-` | 2 |
| `-wx` | 3 |
| `r--` | 4 |
| `r-x` | 5 |
| `rw-` | 6 |
| `rwx` | 7 |

So `777` means the owner, group, and others can all read, write, and execute the file.

This is not a complete explanation of linux filesystem permissions.

A better explanation than mine can [be had](https://www.hackinglinuxexposed.com/articles/20030424.html).

`stat` can also be used to see file permissions.

- `chmod`
- `chage`
- `chown` change ownership of file

## Aliases

- `alias` list aliases and their definitions
    - `alias alias` show definition
    - `alias alias="cmd arg arg"` define alias
- `unalias alias` remove `alias`

## Archives

`tar [options] archive_name contents...` compress `file` into `file.tar.gz` with gzip compression
    - `f` required if you want to specify file of archive
    - action
        - `c` create
        - `x` extract
    - optional compression
        - `z` with gzip compression
        - `j` with bzip2 compression
        - `a` automatically detect compression from file extension when extracting
    - common
        - `cf` create
        - `czf` create with gzip compression
        - `xf` extract
        - `xzf` extract with gzip decompression

## System Information

- `lshw -short` will show you basic information about what hardware you are running. You should run it with `sudo` if you want the maximum amount of information.
- `uname -m` will show your instruction set
- `ps -aux` print information about running processes
- `tty` print file name of terminal connected to stdout
