---
title: "SVG 结构"
date: 2024-02-08 01:17:50 +08:00
draft: false
categories: [SVG]
tags: []
card: false
weight: 0
---

- `M = moveto(M X,Y)`：起始 将画笔移动到指定的坐标位置
- `L = lineto(L X,Y)`：连线 画直线到指定的坐标位置
- `H = horizontal lineto(H X)`：水平线 画水平线到指定的X坐标位置
- `V = vertical lineto(V Y)`：垂直线 画垂直线到指定的Y坐标位置
- `C = curveto(C X1,Y1,X2,Y2,ENDX,ENDY)`：三次贝塞尔曲线
- `S = smooth curveto(S X2,Y2,ENDX,ENDY)`：三次贝塞尔曲线
- `Q = quadratic Belzier curve(Q X,Y,ENDX,ENDY)`：二次贝塞尔曲线
- `T = smooth quadratic Belzier curveto(T ENDX,ENDY)`：二次贝塞尔曲线 映射
- `A = elliptical Arc(A RX,RY,XROTATION,FLAG1,FLAG2,X,Y)`：椭圆弧 弧线
- `Z = closepath()`：闭合（从最后一个点连直线到起始点）关闭路径

使用大写字母表示绝对位置，小写字母表示相对位置（相对于起点的位置，向右向下为正）。
