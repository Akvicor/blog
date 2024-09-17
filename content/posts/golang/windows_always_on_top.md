---
title: "窗口置顶"
date: 2023-06-11 13:33:37 +08:00
draft: false
categories: [Golang]
tags: []
card: false
weight: 0
---

将窗口标题中带有指定字符串的窗口置顶，使其显示在所有窗口之上

```go
package main

import (
	"golang.org/x/sys/windows"
	"strings"
	"syscall"
	"unsafe"
)

func AlwaysOnTop() {
	SetWindowAlwaysOnTop(GetWindowHandleByWindowName("Title"))
}

const SWP_NOSIZE = uintptr(0x0001)
const SWP_NOMOVE = uintptr(0x0002)

// This is dumb but Go doesn't like the inline conversion (see above image).
func IntToUintptr(value int) uintptr {
	return uintptr(value)
}

func SetWindowAlwaysOnTop(hwnd uintptr) {
	user32dll := windows.MustLoadDLL("user32.dll")
	setwindowpos := user32dll.MustFindProc("SetWindowPos")
	setwindowpos.Call(hwnd, IntToUintptr(-1), 0, 0, 100, 100, SWP_NOSIZE|SWP_NOMOVE)
}

func GetWindowHandleByWindowName(window_name string) uintptr {
	user32dll := windows.MustLoadDLL("user32.dll")
	enumwindows := user32dll.MustFindProc("EnumWindows")

	var the_handle uintptr
	window_byte_name := []byte(window_name)

	// Windows will loop over this function for each window.
	wndenumproc_function := syscall.NewCallback(func(hwnd uintptr, lparam uintptr) uintptr {
		// Allocate 100 characters so that it has something to write to.
		var filename_data [100]uint16
		max_chars := uintptr(100)

		getwindowtextw := user32dll.MustFindProc("GetWindowTextW")
		getwindowtextw.Call(hwnd, uintptr(unsafe.Pointer(&filename_data)), max_chars)

		// If there's a match, save the value and return 0 to stop the iteration.
		if strings.Contains(string(windows.UTF16ToString([]uint16(filename_data[:]))), string(window_byte_name)) {
			the_handle = hwnd
			return 0
		}

		return 1
	})

	// Call the above looping function.
	enumwindows.Call(wndenumproc_function, uintptr(0))

	return the_handle
}

```


