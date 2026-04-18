# OS Jackfruit – Mini Container Runtime

## Overview

This project implements a lightweight container runtime system inspired by Docker, built using core Operating Systems concepts. It allows users to create and manage isolated containers where processes run independently with controlled resources.

The system includes a supervisor process, logging pipeline, CLI interface, and a kernel module for memory monitoring and enforcement.

---

## Features

* Multi-container supervision using a central supervisor
* Process isolation using Linux namespaces and chroot
* CLI interface for container management (`start`, `run`, `ps`, `logs`, `stop`)
* Logging system using producer–consumer model with bounded buffer
* Inter-process communication using UNIX domain sockets
* Kernel module for monitoring memory usage
* Soft and hard memory limit enforcement
* Clean container lifecycle management

---

## Architecture

* **engine.c** → User-space runtime and supervisor
* **monitor.c** → Kernel module for memory tracking
* **monitor_ioctl.h** → Shared interface between user and kernel
* **Workloads**:

  * cpu_hog → CPU-intensive workload
  * memory_hog → Memory-intensive workload
  * io_pulse → I/O workload

---

## Build Instructions

```bash
make
```

---

## Running the System

### Start supervisor

```bash
sudo ./engine supervisor ../rootfs-base
```

### Start containers

```bash
sudo ./engine start cpu ../rootfs-alpha ./cpu_hog
sudo ./engine start io ../rootfs-beta ./io_pulse
```

### List containers

```bash
sudo ./engine ps
```

### View logs

```bash
sudo ./engine logs cpu
```

### Run one-time container

```bash
sudo ./engine run test ../rootfs-alpha /bin/ls
```

---

## Memory Limit Enforcement

```bash
sudo dmesg -C
sudo ./engine start mem ../rootfs-alpha ./memory_hog
sleep 2
sudo dmesg | tail
```

* Soft limit → Warning
* Hard limit → Process killed

---

## Demonstrated OS Concepts

* Process isolation
* Scheduling (CPU vs I/O workloads)
* Inter-process communication
* Synchronization (producer–consumer)
* Kernel-user interaction
* Resource management

---

## Clean Teardown

Stop supervisor using:

```bash
CTRL + C
```

Verify:

```bash
ps aux | grep engine
```

---

## Project Structure

```
engine.c
monitor.c
monitor_ioctl.h
Makefile
cpu_hog.c
memory_hog.c
io_pulse.c
rootfs-alpha/
rootfs-beta/
rootfs-base/
```

---

## Authors

Vismay Chandra Dev - PES2UG24CS597
Vaishnavi Goyal - PES2UG24CS571

Operating Systems Mini Project
