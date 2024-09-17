---
title: "修改图片大小"
date: 2019-09-29 17:08:24 +08:00
draft: false
categories: [PHP]
tags: []
card: false
weight: 0
---

叙述使用以下代码修改图片大小或创建缩略图。

参数说明：

`$filename`：文件名。
`$tmpname`：文件路径，如上传中的临时目录。
`$xmax`：修改后最大宽度。
`$ymax`：修改后最大高度。

<!--more-->

```php
function resize_image($filename, $tmpname, $xmax, $ymax)
{
    $ext = explode(".", $filename);
    $ext = $ext[count($ext)-1];
 
    if($ext == "jpg" || $ext == "jpeg")
        $im = imagecreatefromjpeg($tmpname);
    elseif($ext == "png")
        $im = imagecreatefrompng($tmpname);
    elseif($ext == "gif")
        $im = imagecreatefromgif($tmpname);
 
    $x = imagesx($im);
    $y = imagesy($im);
 
    if($x <= $xmax && $y <= $ymax)
        return $im;
 
    if($x >= $y) {
        $newx = $xmax;
        $newy = $newx * $y / $x;
    }
    else {
        $newy = $ymax;
        $newx = $x / $y * $newy;
    }
 
    $im2 = imagecreatetruecolor($newx, $newy);
    imagecopyresized($im2, $im, 0, 0, 0, 0, floor($newx), floor($newy), $x, $y);
    return $im2; 
}
```

