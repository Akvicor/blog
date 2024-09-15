---
title: "BOOT: 从FAT32分区搜索并读取loader"
date: 2022-08-22 21:52:00 +08:00
draft: false
categories: [AOS]
tags: []
card: false
weight: 0
---

## 初始化寄存器

我将物理地址`0x050000-0x070008`之间的内存分配给了堆栈。栈顶设置为`0x070008`的原因是，kernel函数在编译后会在函数开头加上这样一行汇编代码`sub    $0x8,%rsp`，将栈顶设置为`0x070008`可以保证在经过此操作后，栈顶为`0x070000`。

### 段寄存器

- `CS`: 指向存放程序的内存段，`IP`用来存放下条待执行的指令在该段的偏移量。`CS:IP`所指向的指令就是下次要执行的指令。
- `SS`: 指向用于堆栈的内存段，`SP`用来指向该堆栈的栈顶。
- `DS`: 指向数据段
- `ES`: 指向附加段 (辅助段寄存器)
- `FS`: 指向标志段 (辅助段寄存器)
- `GS`: 指向全局段 (辅助段寄存器)

### 缺省段寄存器

- 当偏移量用到了指针寄存器`BP`，则其缺省的段寄存器也是`SS`，并且用BP可访问整个堆栈，不仅仅是只访问栈顶。
- 通常，缺省的数据段寄存器是`DS`
- 在进行串操作时，其目的地址的段寄存器规定为`ES`


## 代码

Use this command to check your USB Stick FAT32 Fields

```shell
sudo hexdump -C -n 512 /dev/sdb
```

Write the bootloader to first 512 bytes of the usb stick

```shell
sudo dd if=boot.bin of=/dev/sdb conv=notrunc
```

boot.asm

```asm
[BITS 16]

[ORG 0x7c00]

;----------------------------------------------- [Jump Instruction; offset: 0x00; length: 3  Bytes]

    jmp short Label_Start                           ; 2 Bytes
    nop                                             ; 1 Bytes

;----------------------------------------------- [FAT32; offset: 0x03; length: 87 Bytes]

%include "fat32.asm"

;----------------------------------------------- [Bootstrap Code; offset: 0x5A; length: 420 Bytes]

Label_Start:
    call Print_StartBoot

    mov sp, BaseOfStack

    push    cs
    pop     ds

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Search and Read "loader.bin" ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

    and     byte [BS_RootDirectoryStart+3], 0x0F ; mask cluster value
    mov     esi, [BS_RootDirectoryStart]         ; esi=cluster # of root dir

RootDirReadContinue:
    push    BaseOfLoader
    pop     es
    xor     bx, bx
    call    ReadCluster     ; read one cluster of root dir
    push    esi             ; save esi=next cluster # of root dir
    pushf                   ; save carry="not last cluster" flag

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Look for the loader file to load and run ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

    push    BaseOfLoader
    pop     es
    xor     di, di                  ; es:di -> root entries array
    mov     si, Filename_Loader     ; ds:si -> program name

    mov     al, [BPB_SectorsPerCluster]
    cbw
    mul     word [BPB_BytesPerSector]   ; ax = bytes per cluster
    shr     ax, 5
    mov     dx, ax                      ; dx = # of dir entries to search in

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Looks for a file/dir by its name      ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Input:  DS:SI -> file name (11 chars) ;;
;;         ES:DI -> root directory array ;;
;;         DX = number of root entries   ;;
;; Output: ESI = cluster number          ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

    mov     cx, 11     ; the length of loader filename

FindNameCycle:
    cmp     byte [es:di], 0xC
    jne     FindNameNotEnd
    jmp     Print_Error_NoLoader                 ; end of root directory (NULL entry found)
FindNameNotEnd:
    pusha
    repe    cmpsb
    popa
    je      FindNameFound
    add     di, 32
    dec     dx
    jnz     FindNameCycle           ; next root entry
    popf                            ; restore carry="not last cluster" flag
    pop     esi                     ; restore esi=next cluster # of root dir
    jc      RootDirReadContinue     ; continue to the next root dir cluster
    jmp     Print_Error_NoLoader    ; end of root directory (dir end reached)
FindNameFound:
    push    word [es:di+0x14]
    push    word [es:di+0x1A]
    pop     esi                     ; si = cluster no.

;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Load the entire file ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;

    push    BaseOfLoader
    pop     es
    xor     bx, bx
FileReadContinue:
    call    ReadCluster             ; read one cluster of root dir

    pushf
    pusha
    inc byte [CLU]
    mov ax, 0xB800
    mov gs, ax
    mov ah, 0x0F       ; 0000: 黑底    1111: 白字
    mov al, [CLU]
    mov [gs:((80 * 0 + 39) * 2)], ax  ; 屏幕第 0 行, 第 39 列。
    popa
    popf

    jc      FileReadContinue

EnterLoader:
    mov bl, byte [LINE]              ; save line number
    jmp BaseOfLoader:OffsetOfLoader

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Reads a FAT32 cluster        ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Inout:  ES:BX -> buffer      ;;
;;           ESI = cluster no   ;;
;; Output:   ESI = next cluster ;;
;;         ES:BX -> next addr   ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

ReadCluster:
    mov     ax, [BPB_BytesPerSector]
    shr     ax, 2                           ; ax=# of FAT32 entries per sector
    cwde
    mov     ebp, esi                        ; ebp=esi=cluster #
    xchg    eax, esi
    cdq
    div     esi                             ; eax=FAT sector #, edx=entry # in sector
    movzx   edi, word [BPB_ReservedSectors]
    add     edi, [BPB_HiddenSectors]
    add     eax, edi

    push    dx                              ; save dx=entry # in sector on stack
    mov     cx, 1
    call    ReadSectorLBA                   ; read 1 FAT32 sector

    pop     si                              ; si=entry # in sector
    add     si, si
    add     si, si
    and     byte [es:si+3], 0x0F            ; mask cluster value
    mov     esi, [es:si]                    ; esi=next cluster #

    lea     eax, [ebp-2]
    movzx   ecx, byte [BPB_SectorsPerCluster]
    mul     ecx
    mov     ebp, eax

    movzx   eax, byte [BPB_TotalFATs]
    mul     dword [BS_BigSectorsPerFAT]

    add     eax, ebp
    add     eax, edi

    call    ReadSectorLBA

    mov     ax, [BPB_BytesPerSector]
    shr     ax, 4                   ; ax = paragraphs per sector
    mul     cx                      ; ax = paragraphs read

    mov     cx, es
    add     cx, ax
    mov     es, cx                  ; es:bx updated

    cmp     esi, 0x0FFFFFF8         ; carry=0 if last cluster, and carry=1 otherwise
    ret

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Reads a sector using BIOS Int 13h fn 42h ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; Input:  EAX = LBA                        ;;
;;         CX    = sector count             ;;
;;         ES:BX -> buffer address          ;;
;; Output: CF = 1 if error                  ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

ReadSectorLBA:
    pushad

ReadSectorLBANext:
    pusha

    push    byte 0
    push    byte 0 ; 32-bit LBA only: up to 2TB disks
    push    eax
    push    es
    push    bx
    push    byte 1 ; sector count word = 1
    push    byte 16 ; packet size byte = 16, reserved byte = 0

    mov     ah, 42h
    mov     dl, [BS_DriveNumber]
    mov     si, sp
    push    ss
    pop     ds
    int     13h
    push    cs
    pop     ds

    jc      short Print_Error_Read
    add     sp, 16 ; the two instructions are swapped so as not to overwrite carry flag

    popa
    dec     cx
    jz      ReadSectorLBADone2      ; last sector

    add     bx, [BPB_BytesPerSector] ; adjust offset for next sector
    add     eax, byte 1             ; adjust LBA for next sector
    jmp     short ReadSectorLBANext

ReadSectorLBADone2:
    popad
    ret

Print_StartBoot:
    pusha
    mov cx, 10
    mov bp, Message_StartBoot
    mov bx, 0x000f
    call Message_Print_End
    popa
    ret

Print_Error_Read:
    mov cx, 11
    mov bp, Message_ReadError
    mov bx, 0x000f
    call Message_Print_End
    jmp $

Print_Error_NoLoader:
    mov cx, 15
    mov bp, Message_NoLoader
    mov bx, 0x000c
    call Message_Print_End
    jmp $

Message_Print_End:
    push es
    push ds
    mov ax, 0
    mov ds, ax
    mov es, ax
    mov ax, 0x1301
    mov dl, 0
    mov dh, [LINE]
    inc byte [LINE]
    int 10h
    pop ds
    pop es
    ret


BaseOfLoader:         EQU   0x1000
OffsetOfLoader:       EQU   0x0000
BaseOfStack:          EQU   0x7C00


Message_StartBoot:    DB    "Start Boot"      ; 10
Message_ReadError:    DB    "ERROR: Read"     ; 11
Message_NoLoader:     DB    "No Loader Found" ; 15
Filename_Loader:      DB    "LOADER  BIN"     ; 11

LINE:                 DB    0x00
CLU:                  DB    0x41

    TIMES 510 - ($ - $$)  DB  0

;----------------------------------------------- [End of Sector Marker; offset: 0x1FE; length: 2 Bytes]

    DW  0xAA55
```


