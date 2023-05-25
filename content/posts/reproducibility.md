---
title: "Reproducibility"
date: 2023-05-25T02:21:32-05:00
draft: false
---

"Containers" are a super popular buzzword in silicon valley these days. So wtf is a container? Well, it is yet another answer to the long-standing question of how to provide a reproducible environment for a program to run in. "It works on my machine" is a symptom of a deeper problem. The problem is that of dependency management, or *configuration management* as it is known more generally.

Configuration management is older than computers. For a long time, engineers have had to deal with the fact that many engineering processes are extremely sensitive to their environment, so the configuration of this environment has to be meticulously tracked. Tools can become miscalibrated over time, so each tool has a unique ID, and the tool that is used to perform a certain task is noted down at the time that task is done! This way any peculiarities in the finished product can be tracked back to that tool. This same issue plagues software.

Sometimes software tools get out of alignment too: their configurations can become mismatched with the specification, which causes an error. Trying to reproduce this outside of the original environment will not cause the same error, because this configuration difference does not exist. The following tools are attempts to solve this problem.

Of course, the ultimate solution to this problem is to make computers less stateful, but that won't happen until the sun dies, so we are going to have to muddle along with various partial solutions if we want to get our jobs done.

## Manual Dependency Management

This is the *lack* of a formal system for tracking and maintaining the dependencies of your programs. It is basically what Windows users do. The basic implications are that graphs of programs that depend on each other are very fragile, and the ecosystem tends to gravitate towards monolithic software that maintains its own independent set of dependencies which are essentially inseparable from itself.

## Package Manager

Use this to automatically install the dependencies of a package when you install. This doesn't answer many of the problems of configuration management. Maybe a and b both depend on c but a requires c to be configured one way and b requires it to be configured another way. The package manager can't resolve this conflict on its own (usually).

## Virtual Environments

Virtual environments like those in the Python ecosystem simply maintain a different set of installed programs based on the project that you are running, so you can have numpy 1.24.3 in one project, and numpy 1.1.1 in another. Because the environment is separate for each project, they also allows these programs to be configured differently. This solves one of the main problems that packaged managers are not capable of solving. These are way lighter weight than any of the next set because they only change the installed packages for a program, rather than the entire OS.

## Containers

File diffs from lower-level containers in a hierarchy. The file diffs are applied at startup, so basically a VM is being constructed at startup by the host. So these are basically VMs but way lighter.

## VMs

Complete file system for a virtual operating system. The hypervisor tricks the operating system into thinking it is running as the main host of the system.

## Separate OS

Of course, the ultimate solution is just to run the software on its own machine or at least its own partition, but these days there isn't much benefit to using this over a VM (because of the hypervisor architecture of modern servers).
