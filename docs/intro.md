---
sidebar_position: 1
---

# Intro

Let's see how you can build a simple `qcow2` image from a bootable container and boot it with QEMU

## Getting Started

Make sure that you have `podman` installed, and you can run it rootful (using `sudo`).

## Build your image

Build your image using the following command:

```bash
mkdir -p output
sudo podman run --rm \
  -it \
  --privileged \
  --security-opt label=type:unconfined_t \
  -v $(pwd)/output:/output \
  ghcr.io/osbuild/osbuild-deploy-container \
  -imageref quay.io/centos-boot/fedora-tier-1:eln
```

## Run your image

Use QEMU to run the result:

```bash
qemu-system-x86_64 \
  -M accel=kvm \
  -cpu host \
  -smp 2 \
  -m 4096 \
  -snapshot output/qcow2/disk.qcow2
```

Congratulations, your container is now booted!
