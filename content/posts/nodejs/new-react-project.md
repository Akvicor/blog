---
title: "New React Project"
date: 2024-07-27 17:21:29 +08:00
draft: false
categories: [Node.js]
tags: []
card: false
weight: 0
---

创建新 React 项目

### 使用create-react-app创建项目

```shell
npx create-react-app
```

### 设置yarn版本

前置条件

```shell
corepack enable # 安装完nodejs后只需要执行一次
```

设置版本

```shell
yarn policies set-version 4.3.0
```

### 修改`.yarnrc.yml`配置

```shell
nodeLinker: node-modules

yarnPath: .yarn/releases/yarn-4.3.0.cjs
```

### 安装依赖

```shell
yarn install
```

