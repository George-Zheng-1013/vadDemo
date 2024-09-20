# vadDemo

## 项目简介

`vadDemo` 是一个基于 Vue 的 Web 应用，主要功能包括用户注册、实时分辨说话人角色、实时检测声音输入、录音上传到后端，并且在前端显示后端返回的响应数据，并通过 TTS (Text-To-Speech) 朗读回复内容。

本项目集成了基于 VAD（Voice Activity Detection）的功能，使用 [ricky0123/vad](https://github.com/ricky0123/vad) 实现音频的语音活动检测。

### 项目演示

你可以访问 [项目的 GitHub 仓库](https://github.com/George-Zheng-1013/vadDemo) 以获取更多信息和代码细节。

## 主要功能

- **用户注册**：允许用户注册并记录登录信息。
- **分辨说话人角色**：实时检测并分辨说话人角色。
- **实时检测声音输入**：通过 VAD 功能检测语音活动，应用会自动监听用户的声音输入，并根据条件上传录音数据到后端处理。
- **录音上传及后端处理**：将录音上传至后端进行处理，并获取处理后的响应。
- **TTS 显示回复**：通过 TTS 引擎将后端返回的回复通过语音播报，并显示在对话框中。

## VAD 功能集成

VAD（语音活动检测，Voice Activity Detection）是一种用来识别音频流中是否包含语音的技术。该项目使用 [ricky0123/vad](https://github.com/ricky0123/vad) 库来实现实时的语音活动检测，以便在有语音输入时触发事件进行录音和处理。当检测到语音时，触发录音并将数据上传至后端，避免上传静音或无关的背景噪音。

## 技术栈

- **前端框架**：Vue 3
- **组件库**：Element Plus
- **HTTP 客户端**：Axios
- **事件总线**：Mitt
- **TTS 引擎**：speak-tts
- **音频处理**：vue-audio-visual
- **VAD 实现**：[vad](https://github.com/ricky0123/vad)

## 安装与使用

### 前提条件

- Node.js (推荐使用 LTS 版本)
- npm 或 Yarn 包管理器

### 克隆仓库

```bash
git clone https://github.com/George-Zheng-1013/vadDemo.git
cd vadDemo
```

### 安装依赖

使用 npm：

```bash
npm install
```

或者使用 Yarn：

```bash
yarn install
```

### 运行开发服务器

```bash
npm run serve
```

然后在浏览器中打开 `http://localhost:8080` 即可预览项目。

### 打包项目

```bash
npm run build
```

## 依赖项

- **[@element-plus/icons-vue](https://www.npmjs.com/package/@element-plus/icons-vue)**: Vue 图标库。
- **[axios](https://www.npmjs.com/package/axios)**: 基于 Promise 的 HTTP 客户端，用于与后端通信。
- **[element-plus](https://element-plus.org/)**: 一个基于 Vue 3 的桌面端组件库。
- **[mitt](https://www.npmjs.com/package/mitt)**: 一款轻量级的事件总线库，用于组件间事件传递。
- **[speak-tts](https://www.npmjs.com/package/speak-tts)**: TTS (Text-To-Speech) 引擎，用于将文本转换为语音。
- **[vue-audio-visual](https://www.npmjs.com/package/vue-audio-visual)**: Vue 组件库，用于处理和可视化音频数据。
- **[vad](https://github.com/ricky0123/vad)**: 轻量级的 VAD (Voice Activity Detection) 库，用于语音活动检测。

## 项目结构

```bash
vadDemo/
│
├── public/              # 公共文件目录
├── src/
│   ├── assets/          # 静态资源文件夹
│   ├── components/      # Vue 组件
│   ├── views/           # 视图组件
│   ├── App.vue          # 根组件
│   ├── main.js          # 项目入口文件 
|   ├── vad.vue          # vad组件
├── package.json         # 项目依赖和脚本
└── README.md            # 项目说明文档
```
