## Quicker Bridge 
[![Download Zip](https://img.shields.io/badge/Download-QuickerBridge__v2.zip-brightgreen?style=for-the-badge&logo=github)](https://github.com/RainskyCG/Blender-Quicker-Bridge/releases/download/v2.0/QuickerBridge_v2.zip)
这是一个专为 Blender 开发的通信插件。它的核心功能是打破 Blender 与外部工作流的隔离，允许外部工具（如 Quicker、AutoHotkey等）直接控制 Blender。
测试版本：Blender 5.0
插件提供了两大核心工作模式：**剪贴板模式** 和 **HTTP 模式**。

---
### 一、 剪贴板模式（快捷运行）
这是最简单、最直观的使用方式。它允许你把系统剪贴板当作代码传递的介质。
- **工作原理：** 复制或编写一段 Blender Python 代码到剪贴板。在 Blender 界面中按下快捷键，插件会立即读取剪贴板内容并在 Blender 后台执行。
- **默认快捷键：** `Ctrl + Shift + Alt + V`（可在偏好设置中修改）。
- **适用场景：** 适合快速的代码片段测试，验证运行效果。

---
### 二、 HTTP 接口模式（深度自动化）
插件开启后，会在 Blender 内部启动一个 HTTP 服务器（默认监听 `127.0.0.1:8765`），外部程序可通过发送网络请求与 Blender 进行无缝的数据交互，无需任何窗口切换或按键模拟。
此模式提供两个主要的 API 接口：
###### 1. 执行外部代码 (POST 请求)
你可以让外部程序通过 POST 请求，将纯文本的 Python 代码发送给 Blender。
- **执行方式：** Blender 收到请求后会在后台静默执行传入的代码。
- **反馈机制：** 如果运行报错服务器会返回附带具体的错误信息的响应。
###### 2. 获取当前状态 (GET 请求)
这是用于实现“智能判断”的核心接口。外部程序发送 GET 请求后，Blender 会实时侦测当前鼠标所在区域的具体状态，并返回一个状态标识符（Status 字符串）。
- **状态识别能力：** 可精准识别出当前鼠标停留的窗口类别（3D 视图、大纲视图、节点编辑器等），处于什么模式（物体模式、编辑模式），以及选中元素（点、线、面、灯光、相机、曲线、物体等）。
- **返回值示例：** * 鼠标在 3D 视图，编辑模式选中面：返回 `EDIT_FACE`
    - 鼠标在 3D 视图，物体模式未选中：返回 `OBJECT_NONE`
    - 鼠标在节点编辑器：返回 `NODE`
- **适用场景：** 外部工具（如 Quicker 动作）可以先通过 GET 请求获取当前鼠标位置的窗口类别、选中状态等信息，再通过条件判断，针对不同的工作场景发送不同的 POST 代码，从而打造极具智能的上下文专属菜单。
