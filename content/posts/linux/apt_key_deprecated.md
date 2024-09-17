---
title: "APT key deprecated"
date: 2023-02-15 11:17:01 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

```shell
curl -s URL | sudo gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/NAME.gpg --import
sudo chmod 644 /etc/apt/trusted.gpg.d/NAME.gpg
```

## google-chrome

```shell
curl -s https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/google_linux_signing_key.gpg --import
sudo chmod 644 /etc/apt/trusted.gpg.d/google_linux_signing_key.gpg
```


