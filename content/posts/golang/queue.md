---
title: "队列 Queue"
date: 2024-01-06 20:07:41 +08:00
draft: false
categories: [Golang]
tags: []
card: false
weight: 0
---

## 介绍

元素为`[]byte`的队列的golang实现（适用于多线程环境下，当然单线程也能用
如果想更改队列的元素类型，请自行将`queue [][]byte`中的`[]byte`替换为其他类型，同时修改函数中的相关代码

1. 应使用`NewQueue(size int) *Queue`来创建队列以初始化队列最大长度
2. 通过互斥锁和信号池保证了在队列非空非满的情况下能够同时`push`和`pop`，且在队列空和满的状态下`push`和`pop`不会冲突
3. 如果队列为空，则`pop`会阻塞，同理队列为满时，`push`会阻塞
4. 由于元素`[]byte`为slice，`deepCopy`参数决定`push`和`pop`出来的`[]byte`是深拷贝还是浅拷贝
5. 队列的数据存储使用的循环数组，避免频繁append导致的资源损耗

## queue.go

```go
package queue

import (
  "sync"
  "sync/atomic"
)

type Queue struct {
  poolStart chan bool
  poolEnd   chan bool
  pushLock  sync.Mutex
  popLock   sync.Mutex
  maxSize   int
  curSize   int32
  wIndex    int
  rIndex    int
  queue     [][]byte
}

func NewQueue(size int) *Queue {
  return &Queue{
    // Start和End信号池用于保证push和pop操作不会互相干扰
    // 每次Push和Pop操作后，两个信号池中的信号数量都会保持一致
    poolStart: make(chan bool, size),
    poolEnd:   make(chan bool, size),
    // 保证push操作完整性
    pushLock: sync.Mutex{},
    // 保证pop操作完整性
    popLock: sync.Mutex{},
    // 队列中元素最大数量
    maxSize: size,
    // 队列当前元素数量
    curSize: 0,
    // push指针
    wIndex: 0,
    // pop指针
    rIndex: 0,
    // 元素数组
    queue: make([][]byte, size),
  }
}

func (q *Queue) Push(item []byte, deepCopy bool) {
  q.pushLock.Lock()
  defer func() {
    // push成功后队列大小+1
    atomic.AddInt32(&q.curSize, 1)
    q.pushLock.Unlock()
    // 操作必定成功，向End信号池发送一个信号，表示完成此次push
    q.poolEnd <- true
  }()
  // 操作成功代表队列不满，向Start信号池发送一个信号，表示开始push
  q.poolStart <- true
  
  if deepCopy {
    q.queue[q.wIndex] = make([]byte, len(item))
    copy(q.queue[q.wIndex], item)
  } else {
    q.queue[q.wIndex] = item
  }
  
  q.wIndex++
  if q.wIndex >= q.maxSize {
    q.wIndex = 0
  }
}

func (q *Queue) Pop(deepCopy bool) (item []byte) {
  q.popLock.Lock()
  defer func() {
    // pop成功后队列大小-1
    atomic.AddInt32(&q.curSize, -1)
    q.popLock.Unlock()
    // 操作必定成功，当前元素已经成功取出，释放当前位置
    <-q.poolStart
  }()
  // 操作成功代表队列非空，只有End信号池中有信号，才能保证有完整的元素在队列中
  <-q.poolEnd
  
  if deepCopy {
    item = make([]byte, len(q.queue[q.rIndex]))
    copy(item, q.queue[q.rIndex])
  } else {
    item = q.queue[q.rIndex]
  }
  q.queue[q.rIndex] = nil
  
  q.rIndex++
  if q.rIndex >= q.maxSize {
    q.rIndex = 0
  }
  return
}

func (q *Queue) Size() int32 {
  return atomic.LoadInt32(&q.curSize)
}

func (q *Queue) IsEmpty() bool {
  return atomic.LoadInt32(&q.curSize) == 0
}
```

## queue_test.go

```go
package queue

import (
  "fmt"
  "testing"
  "time"
)

func TestQueue(t *testing.T) {
  queue := NewQueue(3)
  b1 := []byte{11, 11}
  b2 := []byte{22, 22}
  b3 := []byte{33, 33}
  b4 := []byte{44, 44}
  
  fmt.Printf("push   b1 curSize:%d\n", queue.Size())
  queue.Push(b1, true)
  fmt.Printf("pushed b1 curSize:%d\n", queue.Size())
  fmt.Printf("push   b2 curSize:%d\n", queue.Size())
  queue.Push(b2, false)
  fmt.Printf("pushed b2 curSize:%d\n", queue.Size())
  fmt.Printf("push   b3 curSize:%d\n", queue.Size())
  queue.Push(b3, false)
  fmt.Printf("pushed b3 curSize:%d\n", queue.Size())
  
  // test: push堵塞
  go func() {
    time.Sleep(3 * time.Second)
    fmt.Printf("pop    b10 curSize:%d\n", queue.Size())
    // test: 深push, 浅pop
    b1[0] = 22
    b10 := queue.Pop(false)
    fmt.Printf("poped  b10 [%d,%d] curSize:%d\n", b10[0], b10[1], queue.Size())
    if b10[0] != 11 || b10[1] != 11 {
      t.Errorf("b10 expected [11,11], but [%d,%d] got", b10[0], b10[1])
    }
  }()
  fmt.Printf("push   b4 curSize:%d\n", queue.Size())
  queue.Push(b4, true)
  fmt.Printf("pushed b4 curSize:%d\n", queue.Size())
  if queue.Size() != 3 {
    t.Errorf("queue expected 3, but %d got", queue.Size())
  }
  
  time.Sleep(5 * time.Second)
  fmt.Printf("pop    b11 curSize:%d\n", queue.Size())
  b11 := queue.Pop(true)
  // test: 浅push, 深pop
  b2[0] = 33
  fmt.Printf("poped  b11 [%d,%d] curSize:%d\n", b11[0], b11[1], queue.Size())
  if b11[0] != 22 || b11[1] != 22 {
    t.Errorf("b11 expected [22,22], but [%d,%d] got", b11[0], b11[1])
  }
  fmt.Printf("pop    b12 curSize:%d\n", queue.Size())
  b12 := queue.Pop(false)
  // test: 浅push, 浅pop
  b3[0] = 44
  fmt.Printf("poped  b12 [%d,%d] curSize:%d\n", b12[0], b12[1], queue.Size())
  if b12[0] != 44 || b12[1] != 33 {
    t.Errorf("b12 expected [44,33], but [%d,%d] got", b12[0], b12[1])
  }
  fmt.Printf("pop    b13 curSize:%d\n", queue.Size())
  b13 := queue.Pop(true)
  // test: 深push, 深pop
  b4[0] = 55
  fmt.Printf("poped  b13 [%d,%d] curSize:%d\n", b13[0], b13[1], queue.Size())
  if b13[0] != 44 || b13[1] != 44 {
    t.Errorf("b13 expected [44,44], but [%d,%d] got", b13[0], b13[1])
  }
  
  b5 := []byte{55, 55}
  // test: pop堵塞
  go func() {
    time.Sleep(3 * time.Second)
    fmt.Printf("push   b5 curSize:%d\n", queue.Size())
    queue.Push(b5, false)
    fmt.Printf("pushed b5 curSize:%d\n", queue.Size())
  }()
  fmt.Printf("pop    b14 curSize:%d\n", queue.Size())
  b14 := queue.Pop(false)
  // test: 浅push, 浅pop
  fmt.Printf("poped  b14 [%d,%d] curSize:%d\n", b14[0], b14[1], queue.Size())
  if b14[0] != 55 || b14[1] != 55 {
    t.Errorf("b14 expected [55,55], but [%d,%d] got", b14[0], b14[1])
  }
  
  if !queue.IsEmpty() {
    t.Errorf("queue expected empty, but %v got", queue.IsEmpty())
  }
}
```

