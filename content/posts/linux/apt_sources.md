---
title: "Debian APT源"
date: 2022-07-14 16:07:36 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

编辑

```bash
apt install apt-transport-https ca-certificates
vim /etc/apt/sources.list
```

## Debian 11

```text
# Official & Contrib & Non-free
deb https://deb.debian.org/debian bullseye main contrib non-free
deb-src https://deb.debian.org/debian bullseye main contrib non-free

deb https://deb.debian.org/debian-security bullseye-security main contrib non-free
deb-src https://deb.debian.org/debian-security bullseye-security main contrib non-free

deb https://deb.debian.org/debian bullseye-updates main contrib non-free
deb-src https://deb.debian.org/debian bullseye-updates main contrib non-free

# Backports
deb https://deb.debian.org/debian bullseye-backports main contrib non-free
deb-src https://deb.debian.org/debian bullseye-backports main contrib non-free
```

## Debian 12

```text
# apt install apt-transport-https ca-certificates

deb https://deb.debian.org/debian/ bookworm contrib main non-free non-free-firmware
deb-src https://deb.debian.org/debian/ bookworm contrib main non-free non-free-firmware

deb https://deb.debian.org/debian/ bookworm-updates contrib main non-free non-free-firmware
deb-src https://deb.debian.org/debian/ bookworm-updates contrib main non-free non-free-firmware

deb https://deb.debian.org/debian/ bookworm-backports contrib main non-free non-free-firmware
deb-src https://deb.debian.org/debian/ bookworm-backports contrib main non-free non-free-firmware

deb https://deb.debian.org/debian-security/ bookworm-security contrib main non-free non-free-firmware
deb-src https://deb.debian.org/debian-security/ bookworm-security contrib main non-free non-free-firmware
```


