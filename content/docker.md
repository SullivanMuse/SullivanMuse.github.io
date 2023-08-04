---
title: "Docker"
date: 2023-08-04T01:03:27-05:00
draft: false
---

# Docker

Docker is a container system. The basic idea is that it creates a reproducible environment in which to run an app. Containers will have other required packages to run your application, environment variables, and may remap things like ports and IP addresses. They are similar to VMs, but much more lightweight.

## Images

A Docker image is a blueprint for a container. They store the following:

- Runtime environment
- Application code
- Any dependencies
- Extra configuration (environment variables)
- Commands

Images are read-only. If you need to change something about the image, you must build a brand new image. This is desireable because it means that the environment is reproducible.

## Containers

Running an image creates a container, which is a process that can run our application exactly in the environment defined in the image. Containers are an isolated process.
