---
title: "Notepad Compile"
date: 2023-02-15 12:05:02 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

## Environment

Add MinGW environment to Path: `C:\MinGW\bin`

## Open Notepad++

Plug->Plugin Manager->Show Plugin Manager

at Available, double click 'downloading list'

Search 'Nppexec', install and restart

## Open Console

Plug->NppExec->Show Console

## Compile

Plug->NppExec->Execute

### Compile

- input: `g++ $(FULL_CURRENT_PATH) -g -o $(CURRENT_DIRECTORY)\$(NAME_PART).exe`
- click save, input: `Compile`

### Run

- input: `$(CURRENT_DIRECTORY)\$(NAME_PART).exe`
- click save, input: `Run`

### GDB

- input: `gdb $(CURRENT_DIRECTORY)\$(NAME_PART).exe`
- click save, input: `GDB`

## Add to Macros submenu

Plug->NppExec->Advanced Options

at Associated script:

- Add-> Compile and Run and GBD
- Menu items -> Place to the Macros submenu
