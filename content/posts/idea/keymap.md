---
title: "IntelliJ IDEA Keymap"
date: 2019-09-29T19:07:57Z
draft: false
categories: [IDEA]
tags: []
card: false
weight: 0
---

此快捷键说明我是翻译官方的快捷键说明的，方便查看，基于PHPStorm的官方Help来翻译的，其他系列的JetBrains软件应该都是一样的道理，其中如有错误，欢迎斧正。

<!--more-->

## MAC

### Mac键盘符号和修饰键说明

- `⌘` Command
- `⇧` Shift
- `⌥` Option
- `⌃` Control
- `↩︎` Return/Enter
- `⌫` Delete
- `⌦` 向前删除键（Fn+Delete）
- `↑` 上箭头
- `↓` 下箭头
- `←` 左箭头
- `→` 右箭头
- `⇞` Page Up（Fn+↑）
- `⇟` Page Down（Fn+↓）
- `Home` Fn + ←
- `End` Fn + →
- `⇥` 右制表符（Tab键）
- `⇤` 左制表符（Shift+Tab）
- `⎋` Escape (Esc)

### 一、Editing（编辑）

- `Control + Space` 基本的代码补全（补全任何类、方法、变量）
- `Control + Shift + Space` 智能代码补全（过滤器方法列表和变量的预期类型）
- `Command + Shift + Enter` 自动结束代码，行末自动添加分号
- `Command + P` 显示方法的参数信息
- `Control + J` 快速查看文档
- `Shift + F1` 查看外部文档（在某些代码上会触发打开浏览器显示相关文档）
- `Command + 鼠标放在代码上` 显示代码简要信息
- `Command + F1` 在错误或警告处显示具体描述信息
- `Command + N, Control + Enter, Control + N` 生成代码（getter、setter、构造函数、hashCode/equals,toString）
- `Control + O` 覆盖方法（重写父类方法）
- `Control + I` 实现方法（实现接口中的方法）
- `Command + Option + T` 包围代码（使用if..else, try..catch, for, synchronized等包围选中的代码）
- `Command + /` 注释/取消注释与行注释
- `Command + Option + /` 注释/取消注释与块注释
- `Option + 方向键上` 连续选中代码块
- `Option + 方向键下` 减少当前选中的代码块
- `Control + Shift + Q` 显示上下文信息
- `Option + Enter` 显示意向动作和快速修复代码
- `Command + Option + L` 格式化代码
- `Control + Option + O` 优化import
- `Control + Option + I` 自动缩进线
- `Tab / Shift + Tab` 缩进代码 / 反缩进代码
- `Command + X` 剪切当前行或选定的块到剪贴板
- `Command + C` 复制当前行或选定的块到剪贴板
- `Command + V` 从剪贴板粘贴
- `Command + Shift + V` 从最近的缓冲区粘贴
- `Command + D` 复制当前行或选定的块
- `Command + Delete` 删除当前行或选定的块的行
- `Control + Shift + J` 智能的将代码拼接成一行
- `Command + Enter` 智能的拆分拼接的行
- `Shift + Enter` 开始新的一行
- `Command + Shift + U` 大小写切换
- `Command + Shift + ] / Command + Shift + [` 选择直到代码块结束/开始
- `Option + Fn + Delete` 删除到单词的末尾
- `Option + Delete` 删除到单词的开头
- `Command + 加号 / Command + 减号` 展开 / 折叠代码块
- `Command + Shift + 加号` 展开所以代码块
- `Command + Shift + 减号` 折叠所有代码块
- `Command + W` 关闭活动的编辑器选项卡

### 二、Search/Replace（查询/替换）

- `Double Shift` 查询任何东西
- `Command + F` 文件内查找
- `Command + G` 查找模式下，向下查找
- `Command + Shift + G` 查找模式下，向上查找
- `Command + R` 文件内替换
- `Command + Shift + F` 全局查找（根据路径）
- `Command + Shift + R` 全局替换（根据路径）
- `Command + Shift + S` 查询结构（Ultimate Edition 版专用，需要在Keymap中设置）
- `Command + Shift + M` 替换结构（Ultimate Edition 版专用，需要在Keymap中设置）

### 三、Usage Search（使用查询）

- `Option + F7 / Command + F7` 在文件中查找用法 / 在类中查找用法
- `Command + Shift + F7` 在文件中突出显示的用法
- `Command + Option + F7` 显示用法

### 四、Compile and Run（编译和运行）

- `Command + F9` 编译Project
- `Command + Shift + F9` 编译选择的文件、包或模块
- `Control + Option + R` 弹出 Run 的可选择菜单
- `Control + Option + D` 弹出 Debug 的可选择菜单
- `Control + R` 运行
- `Control + D` 调试
- `Control + Shift + R, Control + Shift + D` 从编辑器运行上下文环境配置

### 五、Debugging（调试）

- `F8` 进入下一步，如果当前行断点是一个方法，则不进入当前方法体内
- `F7` 进入下一步，如果当前行断点是一个方法，则进入当前方法体内，如果该方法体还有方法，则不会进入该内嵌的方法中
- `Shift + F7` 智能步入，断点所在行上有多个方法调用，会弹出进入哪个方法
- `Shift + F8` 跳出
- `Option + F9` 运行到光标处，如果光标前有其他断点会进入到该断点
- `Option + F8` 计算表达式（可以更改变量值使其生效）
- `Command + Option + R` 恢复程序运行，如果该断点下面代码还有断点则停在下一个断点上
- `Command + F8` 切换断点（若光标当前行有断点则取消断点，没有则加上断点）
- `Command + Shift + F8` 查看断点信息

### 六、Navigation（导航）

- `Command + O` 查找类文件
- `Command + Shift + O` 查找所有类型文件、打开文件、打开目录，打开目录需要在输入的内容前面或后面加一个反斜杠`/`
- `Command + Option + O` 前往指定的变量 / 方法
- `Control + 方向键左 / Control + 方向键右` 左右切换打开的编辑tab页
- `F12` 返回到前一个工具窗口
- `Esc` 从工具窗口进入代码文件窗口
- `Shift + Esc` 隐藏当前或最后一个活动的窗口，且光标进入代码文件窗口
- `Command + Shift + F4` 关闭活动run/messages/find/... tab
- `Command + L` 在当前文件跳转到某一行的指定处
- `Command + E` 显示最近打开的文件记录列表
- `Option + 方向键左 / Option + 方向键右` 光标跳转到当前单词 / 中文句的左 / 右侧开头位置
- `Command + Option + 方向键左 / Command + Option + 方向键右` 退回 / 前进到上一个操作的地方
- `Command + Shift + Delete` 跳转到最后一个编辑的地方
- `Option + F1` 显示当前文件选择目标弹出层，弹出层中有很多目标可以进行选择(如在代码编辑窗口可以选择显示该文件的Finder)
- `Command + B / Command + 鼠标点击` 进入光标所在的方法/变量的接口或是定义处
- `Command + Option + B` 跳转到实现处，在某个调用的方法名上使用会跳到具体的实现处，可以跳过接口
- `Option + Space, Command + Y` 快速打开光标所在方法、类的定义
- `Control + Shift + B` 跳转到类型声明处
- `Command + U` 前往当前光标所在方法的父类的方法 / 接口定义
- `Control + 方向键下 / Control + 方向键上` 当前光标跳转到当前文件的前一个/后一个方法名位置
- `Command + ] / Command + [` 移动光标到当前所在代码的花括号开始/结束位置
- `Command + F12` 弹出当前文件结构层，可以在弹出的层上直接输入进行筛选（可用于搜索类中的方法）
- `Control + H` 显示当前类的层次结构
- `Command + Shift + H` 显示方法层次结构
- `Control + Option + H` 显示调用层次结构
- `F2 / Shift + F2` 跳转到下一个/上一个突出错误或警告的位置
- `F4 / Command + 方向键下` 编辑/查看代码源
- `Option + Home` 显示到当前文件的导航条
- `F3`选中文件/文件夹/代码行，添加/取消书签
- `Option + F3` 选中文件/文件夹/代码行，使用助记符添加/取消书签
- `Control + 0...Control + 9` 定位到对应数值的书签位置
- `Command + F3` 显示所有书签

### 七、Refactoring（重构）

- `F5` 复制文件到指定目录
- `F6` 移动文件到指定目录
- `Command + Delete` 在文件上为安全删除文件，弹出确认框
- `Shift + F6` 重命名文件
- `Command + F6` 更改签名
- `Command + Option + N` 一致性
- `Command + Option + M` 将选中的代码提取为方法
- `Command + Option + V` 提取变量
- `Command + Option + F` 提取字段
- `Command + Option + C` 提取常量
- `Command + Option + P` 提取参数

### 八、VCS/Local History（版本控制/本地历史记录）

- `Command + K` 提交代码到版本控制器
- `Command + T` 从版本控制器更新代码
- `Option + Shift + C` 查看最近的变更记录
- `Control + C` 快速弹出版本控制器操作面板

### 九、Live Templates（动态代码模板）

- `Command + Option + J` 弹出模板选择窗口，将选定的代码使用动态模板包住
- `Command + J` 插入自定义动态代码模板

### 十、General（通用）

- `Command + 1...Command + 9` 打开相应编号的工具窗口
- `Command + S` 保存所有
- `Command + Option + Y` 同步、刷新
- `Control + Command + F` 切换全屏模式
- `Command + Shift + F12` 切换最大化编辑器
- `Option + Shift + F` 添加到收藏夹
- `Option + Shift + I` 检查当前文件与当前的配置文件
- Control + ` 快速切换当前的scheme（切换主题、代码样式等）
- `Command + ,` 打开IDEA系统设置
- `Command + ;` 打开项目结构对话框
- `Shift + Command + A` 查找动作（可设置相关选项）
- `Control + Shift + Tab` 编辑窗口标签和工具窗口之间切换（如果在切换的过程加按上delete，则是关闭对应选中的窗口）

### 十一、Other（一些官方文档上没有体现的快捷键）

- `Command + Shift +8` 竖编辑模式

## Windows

### 编辑

------

| 快捷键组合            | 说明                                                         |
| :-------------------- | :----------------------------------------------------------- |
| Ctrl + Space          | 代码自动完成提示（选择）                                     |
| Alt + Enter           | 显示意图动作和快速修复                                       |
| Ctrl + P              | 参数信息（在调用方法参数忘记的时候，提示）                   |
| Ctrl + Q              | 快速查找文件，可以查找当前类定义的文件等                     |
| Ctrl + 鼠标滑过       | 基本信息                                                     |
| Alt + Insert          | 生成代码...(细节需要多次操作会发现很有意思)                  |
| Ctrl + O              | 重写方法（在PHPStorm中是重写父类方法，会有选择框）           |
| Ctrl + I              | 实现方法（一般是指实现接口类或抽象类方法）                   |
| Ctrl + Alt + T        | 环绕代码块 (if..else, try..catch, for, 等)                   |
| Ctrl + /              | 单行注释(//)                                                 |
| Ctrl + Shift + /      | 块注释 (/**/)                                                |
| Ctrl + W              | 选择依次递增的代码块，具体使用目前来看比较少                 |
| Ctrl + Shift + W      | 去掉当前选择返回上一个选择，类似于撤销选择，与上面的相反     |
| Ctrl + Alt + L        | 格式化代码，一般来说，写的代码格式不整齐统一，这个很有用     |
| Ctrl + Alt + I        | 自啮合线，这个解释不太好解释，测试结果就是会自动根据代码来进行对齐 |
| Ctrl + D              | 复制当前行或选定的块                                         |
| Ctrl + Y              | 删除插入符号所在行                                           |
| Ctrl + Shift + J      | 智能线连接（HTML和JavaScript才有用）                         |
| Ctrl + Enter          | 智能分割线 (HTML 和 JavaScript 才有用)                       |
| Shift + Enter         | 开始新行，比如光标在当前行，不需要切换到行尾按Enter，直接按这个组合键即可 |
| Ctrl + Shift + U      | 切换选中的英文文字的大小写，此处其实用到挺多的               |
| Ctrl + Shift + ] 或 [ | 选择直到代码块的开始或结束，我之前不知道这个，其实很有用     |
| Ctrl + Delete         | 删除从当前光标到当前单词结尾                                 |
| Ctrl + Backspace      | 从光标位置删除到当前单词的开始                               |
| Ctrl + + 或 -         | 这里是ctrl和加号或者减号产生的组合，可以折叠或展开当前代码块 |
| Ctrl + F4             | 关闭活动中的tab                                              |
| Ctrl + Shift + V      | 从历史粘贴                                                   |

### 调试

------

*此处我是用得很少*

| 快捷键组合    | 说明       |
| :------------ | :--------- |
| F8            | 跳过       |
| F7            | 步进       |
| Shift + F8    | 跳出       |
| Alt + F8      | 表达式求值 |
| F9            | 恢复程序   |
| Ctrl + F8     | 切断断点   |
| Ctrl+Shift+F8 | 查看断点   |

### 运行

------

| 快捷键组合         | 说明                                                         |
| :----------------- | :----------------------------------------------------------- |
| Shift + F10        | 运行                                                         |
| Shift + F9         | 调试                                                         |
| Ctrl + Shift + F10 | 从编辑器运行上下文配置（Run context configuration from editor），此处可能翻译不够准确 |
| Ctrl + Shift + X   | 在命令行运行                                                 |

### 搜索/替换

| 快捷键组合         | 说明              |
| :----------------- | :---------------- |
| Ctrl + F/R         | 查找/替换         |
| F3/Shift + F3      | 查找下一个/上一个 |
| Ctrl + Shift + F/R | 在目录中查找/替换 |

### 查找哪些地方使用

| 快捷键组合           | 说明                                        |
| :------------------- | :------------------------------------------ |
| Alt + F7 / Ctrl + F7 | 当前文件查找被使用/在文件中查找哪些地方使用 |
| Ctrl + Shift + F7    | 文件中搜索并在使用的地方高亮显示            |
| Ctrl + Alt + F7      | 显示哪些地方被使用                          |

### 导航

------

| 快捷键组合               | 说明                                                         |
| :----------------------- | :----------------------------------------------------------- |
| Ctrl + N                 | 跳转到指定类                                                 |
| Ctrl + Shift + N         | 跳转到文件                                                   |
| Ctrl + Alt + Shift + N   | 跳转到符号                                                   |
| Ctrl + G                 | 跳转到第几行                                                 |
| Alt + Right/Left         | 切换编辑器活动窗                                             |
| Esc                      | Go to editor (from tool window)                              |
| Ctrl + E                 | 弹出最近编辑文件，我也是在写这文档才知道，太方便了           |
| Ctrl + Alt + Left/Right  | 导航前进/后退                                                |
| Ctrl + Shift + Backspace | 跳转到最近编辑的代码位置                                     |
| Alt + F1                 | 在任何视图中选择当前文件或符号                               |
| Ctrl + B 或 Ctrl + Click | 跳到申明（如跳转到当前函数声明的地方，这个很常用，可以实操一下） |
| Ctrl + Alt + B           | 与上面相反，跳到执行位置                                     |
| Ctrl + Shift + I         | 打开快速定义查找                                             |
| Ctrl + Shift + B         | 跳转到类型声明                                               |
| Ctrl + U                 | 跳到超级方法(super-method)/超类 (super-class)                |
| Alt + Up/Down            | 跳转到上一个或者下一个方法，在编辑一个类的时候，方便一个一个的方法进行查看 |
| Ctrl + ] / [             | 跳转到代码块的开始或结束                                     |
| F2 / Shift + F2          | 跳转到上一个或下一个高亮错误地方，这个检查代码语法错误很有用 |
| F4 / Ctrl + Enter        | 编辑源代码/查看源代码                                        |

### 重构

------

| 快捷键组合             | 说明                                                         |
| :--------------------- | :----------------------------------------------------------- |
| F5/F6                  | 复制/移动                                                    |
| Alt + Delete           | 安全删除                                                     |
| Shift + F6             | 重命名                                                       |
| Ctrl + Alt + N         | 内联变量                                                     |
| Ctrl + Alt + M/V/F/C   | 提取方法/变量/字段/常数(Method/Variable/Field/Constant)      |
| Ctrl + Alt + Shift + T | 重构这段代码（显示所有可用的重构），比如if else if 这种语句转switch语句 |

### VCS/本地历史

------

| 快捷键组合       | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| Alt + 反引号 (`) | ‘VCS’ 快速弹出，此处需要注意这个反引号在最左上角，和那个~符号在一起的，ESC键下面 |
| Ctrl + K         | 提交项目到VCS                                                |
| Ctrl + T         | 从 VCS 更新项目                                              |
| Alt + Shift + C  | 显示最近更改                                                 |

### 常用操作

------

| 快捷键组合         | 说明                                                       |
| :----------------- | :--------------------------------------------------------- |
| 快速按两次 Shift   | 搜索任何一个地方                                           |
| Ctrl + Shift + A   | 查找方法(Action)                                           |
| Alt + #[0-9]       | 打开相应的工具窗口（这个我也没搞明白）                     |
| Ctrl + Alt + F11   | 开启或关闭全屏模式                                         |
| Ctrl + Shift + F12 | 开启或关闭最大化编辑                                       |
| Alt + Shift + F    | 添加到收藏列表（我觉得这个功能很神奇，不知道为啥要这么做） |
| Alt + Shift + I    | 检查当前文件以及当前配置文件                               |
| Ctrl + Alt + S     | 打开设置对话框（表示会与QQ默认快捷键冲突）                 |
| Ctrl + Tab         | 在 tabs 和工具窗口间切换                                   |

### 插入模板/片段(针对PHPstorm)

------

| 快捷键组合 | 说明                                         |
| :--------- | :------------------------------------------- |
| Alt + J    | 插入模板                                     |
| eco        | ‘echo’ 语句                                  |
| fore       | foreach(iterable_expr as $value) {…}         |
| forek      | foreach(iterable_expr as $key => $value) {…} |
| inc/inco   | ‘include’/‘include_once’ 语句                |
| prif       | private function                             |
| prof       | protected function                           |
| pubf       | public function                              |
| rqr/rqro   | ‘require’/‘require_once’ 语句                |
| 更多...    | 其他自己尝试                                 |

