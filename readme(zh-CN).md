## 概述
感谢您使用 YesCool! 导播管家插件😄。

本插件运行于Windows x64平台，依赖OBS Studio运行，兼容OBS v30及以上版本。支持相机控制（变焦、聚焦、预置位）、云台控制（360°方向转动）、MIDI导播键盘（普通键、摇杆、T杆、旋钮、滑动杆，支持普通键的按下、抬起、长按、点亮、熄灭控制）、自动化功能（触发条件、触发动作）、全场景预览、API服务等功能。

## 授权
YesCool!导播插件占用了作者大量的精力开发与维护，因此要收取少量的授权费用才可使用。在软件界面点击“授权”按屏幕提示完成授权即可使用。

## PTZ控制
包含相机控制与云台控制。
当您切换OBS场景源时，YesCool!导播插件自动识别源类型，如果是视频/相机时，显示PTZ控制界面。对于新添加或首次识别的场景源，需要您做一些简单的设置，这包括相机的IP、端口、协议（VISCA、PELCO、网络），然后就可以控制相机与云台了。

## MIDI导播键盘
支持MIDI协议的专业导播键盘，当您通过USB接口连接到电脑后，可以创建个性化的键盘命令。
YesCool!导播管家允许您创建多个键盘方案，并自由切换方案。每一个按键设置好指令动作与参数即可。

## 全场景预览
显示全部场景的预览画面，实时检测各场景内的媒体声音，并醒目显示声音状态。

## 图片幻灯片预览/切换
显示当前预览场景的内的所有图片幻灯片，双击直接切换幻灯片图像。

## 音频增益
YesCool!导播插件内置音频淡入淡出、音量增益等全局功能。在自动化节目里选择“使用背景音乐”后，可自动启用此功能。

## 自动化节目
如果您希望在导播时实现一些自动化操作，比如切换场景、播放媒体、调取预置位、布局调整等，可以用YesCool!导播插件的自动化节目功能。
一个节目允许添加多个不同组合的触发条件与触发动作，设置多个不同的节目，形成节目组，能实现更强大灵活的自动化导播。

### 触发条件
- 支持逻辑与、逻辑或的组合关系。
- 支持等于、小于、大于、不等于、大于或等于、小于或等于等比较运算。
- 支持直接触发。
- 支持对所选条件项的可见、大小、位置、角度、媒体状态、场景等项目的判断。

### 触发动作
- 当设置的触发条件按逻辑关系判断成功时，触发实际的控制动作。
- 允许设置多个动作一起执行。
- 支持控制场景、输入源、预置位、媒体等操作。
- 当控制项目的位置、大小、角度时，支持连续性的动作，即可按增量值变化，又可以按绝对值变化，这样能形成简单的动作。

## API 服务
YesCool!导播插件内置了API服务，同时支持TCP和UDP两种协议。您通过API服务可实现更高级的扩展功能，API服务有以下特点：
1. API 指令格式简洁；
2. API 指令可与MIDI键盘绑定；
3. 自由控制API服务是否启用；
4. 服务请求/响应均使用UTF8编码。

### 服务请求
与API服务通讯时，将指令与参数生成JSON格式的字符串，JSON格式为{"cmd":"指令名称","param1":"参数1","param2":参数2}，然后将JSON串按UTF8编码转为字节序列，再进行Base64编码放入括号内，即可得到API服务的传输内容，即JSON=>UTF8=>(base64)。
举个例子：假如您想弹出一个消息框，内容是“Hello,World!”，那么您按以下步骤生成传输内容：
1. 生成JSON串：{"cmd":"msgbox","param1":"Hello,World!"}
2. 将上面的JSON串进行UTF8编码得到字节数组，再转为base64码：eyJjbWQiOiJtc2dib3giLCJwYXJhbTEiOiJIZWxsbyxXb3JsZCEifQ==
3. 将得到的base64码放入括号内：(eyJjbWQiOiJtc2dib3giLCJwYXJhbTEiOiJIZWxsbyxXb3JsZCEifQ==)
4. 向API服务发送第3步得到的结果即可。

### 服务响应
API服务收到请求后，解析指令内容并执行，返回执行结果。执行结果为纯文本，如果成功将返回OK，否则返回错误信息。

### 服务指令
以下是当前版本支持的服务指令及指令参数要求，格式：指令名称(参数1,[参数2])。如果参数用方括号标注，说明它是一个可选的参数，如果某指令没有参数清单说明无参数要求。

### 系统全局指令
- OpenUrl(param1)：打开param1指定的URL地址。
- GetUrl(param1)：向param1指定的URL地址发送HTTP GET请求。
- PostUrl(param1)：向param1指定的URL地址发送HTTP POST请求。
- PutUrl(param1)：向param1指定的URL地址发送HTTP PUT请求。
- DeleteUrl(param1)：向param1指定的URL地址发送HTTP DELETE请求。
- OpenFile(param1)：打开param1指定的文件。
- OpenFolder(param1)：打开param1指定的目录。
- Hotkey(param1)：触发param1指定的热键组合（本指令需授权后有效），每个键之间以+相连，多个组合之间以逗号分开。
	- 以下例子都是有效的
	- Ctrl+A,Ctrl+Alt+F1 (两份组合键)、
	- Ctrl+A (一个组合键)、
	- Escape (单个键)
- MsgBox(param1)：以对话框的方式显示param1指定的消息内容。
- Toast(param1)：以提示条的方式显示param1指定的消息内容。
- Exit：退出OBS Studio。

### PTZ控制指令 
仅控制当前选中场景源相机和云台，PTZ的所有指令需授权后有效！！
- ZoomStop：停止变焦动作。
- ZoomIn：放大焦距（使目标拉近）。
- ZoomOut：缩小焦距（使目标推远）。
- FocusStop：停止聚焦动作。
- FocusFar：远聚焦。
- FocusNear：近聚焦。
- FocusAuto：自动聚焦。

- PTZLeft(param1,[param2])、PTZLeftUp(param1,[param2])、PTZRight(param1,[param2])、PTZRightUp(param1,[param2])、PTZUp(param1,[param2])、PTZDown(param1,[param2])、PTZLeftDown(param1,[param2])、PTZRightDown(param1,[param2])：使云台向指定的方向转动。param1要求为数字，它决定转动速度；param2要求为数字，它指定允许的最大速度值。
- PTZStop：使云台停止转动。
- GotoHome：使云台回到初始位。
- CallPreset(param1)：param1要求为数字，从0开始计数，调取指定序号的预置位。
- SetPreset(param1)：param1要求为数字，从0开始计数，设置指定序号的预置位。

### OBS指令 
- ToggleRecording：开始/停止录制（本指令需授权后有效）。
- StartRecord：开始录制（本指令需授权后有效）。
- StopRecord：停止录制（本指令需授权后有效）。

- ToggleLiving：开始/停止直播推流（本指令需授权后有效）。
- StartLive：开始直播推流（本指令需授权后有效）。
- StopLive：停止直播推流（本指令需授权后有效）。

- ToggleStudioMode：打开/关闭工作室模式（本指令需授权后有效）。
- OpenStudioMode：打开工作室模式（本指令需授权后有效）。
- CloseStudioMode：关闭工作室模式（本指令需授权后有效）。

- ToggleVirtualCam：打开/关闭虚拟摄像机（本指令需授权后有效）。
- OpenVirtualCam：打开虚拟摄像机（本指令需授权后有效）。
- CloseVirtualCam：关闭虚拟摄像机（本指令需授权后有效）。

- ToggleFullscreen：打开/关闭全屏视图（本指令需授权后有效）。
- OpenFullscreen：打开全屏视图（本指令需授权后有效）。
- CloseFullscreen：关闭全屏视图（本指令需授权后有效）。

- Preview(param1)：使预览画面切换到param1指定的场景（本指令需授权后有效），param1可以是数字序号，也可以是场景名称。
- Output(param1)：使输出画面切换到param1指定的场景（本指令需授权后有效），param1可以是数字序号，也可以是场景名称。
- CutDirect：直接将当前正在预览的场景切换到输出画面（本指令需授权后有效）。
- AutoDirect：根据最后一次使用的转场效果将当前正在预览的场景切换到输出画面（本指令需授权后有效）。
- Transaction(param1)：使用param1指定的转场效果将当前正在预览的场景切换到输出画面（本指令需授权后有效），param1指定转场效果的名称。
- T-Bar(param1)：控制OBS场景切换的变化幅度（本指令需授权后有效），param1要求为数字。

- Select(param1)：选择当前预览场景内的输入源（本指令需授权后有效），param1可以是数字序号，也可以是输入源的名称。
- Volume(param1,[param2])：调整当前预览场景选中源的音量（本指令需授权后有效），param1表示音量值，param2表示最大音量	值。如果当前预览场景没有选中的源，则执行失败。
- ToggleMuted：使当前预览场景选中源的媒体静音/不静音（本指令需授权后有效）。
- Muted：使当前预览场景选中源的媒体静音（本指令需授权后有效）。
- UnMuted：使当前预览场景选中源的媒体不静音（本指令需授权后有效）。

- PlayMedia：播放当前预览场景选中源的媒体（本指令需授权后有效）。
- PauseMedia：暂停播放当前预览场景选中源的媒体（本指令需授权后有效）。
- PlayPauseMedia：暂停/继续播放当前预览场景选中源的媒体（本指令需授权后有效）。
- RestartMedia：强制重新播放当前预览场景选中源的媒体（本指令需授权后有效）。
- StopMedia：停止播放当前预览场景选中源的媒体（本指令需授权后有效）。
- PreviousMedia：如果当前预览场景选中源的媒体是播放列表，则切换到上一个媒体播放（本指令需授权后有效）。
- NextMedia：如果当前预览场景选中源的媒体是播放列表，则切换到下一个媒体播放（本指令需授权后有效）。

- ViewRecords：查看OBS Studio录制的媒体文件。
- Screenshot：截屏OBS Studio的当前输出画面。

## 结语
感谢您的选择。
YesCool!导播管家会持续的加入新的功能，在授权有效期内，免费使用。