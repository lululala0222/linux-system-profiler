# Linux System Profiler

this is a side project for exploring Linux system programming, improving my ability of interact with embedded system or hardware.

## Goals

- Practice C programming in a Linux environment
- Learn how to read system information from `/proc`
- Practice Makefile, gdb, and basic debugging workflow
- Build a small but maintainable system programming project

## Non-goals

This project does not aim to implement a production-grade monitoring tool.
It also avoids kernel modules, root-only operations, and heavy stress testing.

## Planned Features

- [ ] Read system load average from `/proc/loadavg`
- [ ] Read memory information from `/proc/meminfo`
- [ ] Display basic process information
- [ ] Log selected metrics to CSV
- [ ] Handle graceful termination with signals

## Build

```bash
make
```