# OptiMix - 最优配比计算器

<p align="center">
  <img src="./public/icons/png/256x256.png" alt="OptiMix Icon" width="128" height="128" />
</p>

<p align="center">
  <strong>基于线性规划的智能成本优化工具</strong>
</p>

<p align="center">
  <a href="#-功能特性">功能特性</a> •
  <a href="#-快速开始">快速开始</a> •
  <a href="#-构建指南">构建指南</a> •
  <a href="#-技术架构">技术架构</a> •
  <a href="#-项目结构">项目结构</a>
</p>

---

## 📖 项目简介

**OptiMix** 是一款跨平台的配比优化计算器，专为需要进行原料混合并控制成本的场景设计（如饲料加工、化工配料等）。它利用 **线性规划算法 (Linear Programming)**，在满足各项质量指标（如最小值、最大值）约束的前提下，自动计算出**成本最低**的配比方案。

## ✨ 功能特性

- **🧠 智能求解**：内置线性规划求解器，毫秒级计算满足所有约束的最优成本方案。
- **🌍 多语言支持**：内置中英文界面，支持一键切换。
- **📊 动态与静态**：支持“自动计算最优解”和“手动滑块调整”两种模式。
- **📱 全平台支持**：
  - **Web**: 响应式设计，适配桌面与移动浏览器。
  - **Desktop (Electron)**: macOS (.dmg), Windows (.exe) 原生体验。
  - **Mobile (Capacitor)**: Android, iOS 原生应用体验。
- **🎨 现代 UI**：基于 Tailwind CSS 设计，支持**深色模式**（自动跟随系统）。
- **� 数据管理**：
  - 多方案管理（创建、重命名、删除）。
  - 支持 Excel 导入/导出，方便数据迁移。
  - 本地持久化存储，无需联网。

## 🚀 快速开始

### 环境准备

- **Node.js**: v18.0.0 或更高版本
- **包管理器**: npm 或 yarn

### 开发环境运行

```bash
# 1. 克隆仓库
git clone https://github.com/JVxie/Optmix.git
cd Optmix

# 2. 安装依赖
npm install

# 3. 启动 Web 开发服务器 (http://localhost:3000)
npm run dev

# 4. 启动 Electron 开发环境 (桌面端)
npm run electron:dev
```

## 📦 构建指南

本项目支持构建为多种平台的应用。构建产物将位于 `dist/` (Web) 或 `release/` (Desktop) 目录。

### 快速构建命令

| 目标平台 | 构建命令 | 说明 |
| :--- | :--- | :--- |
| **Web** | `npm run build` | 生成静态文件至 `dist/` |
| **macOS** | `npm run electron:build:mac` | 生成 .dmg (支持 Intel & Apple Silicon) |
| **Windows** | `npm run electron:build:win` | 生成 .exe 安装包 |
| **Android** | 见下方详细说明 | 生成 .apk 安装包 |

> **注意**: 构建 macOS 应用需要 macOS 环境；构建 Windows 应用建议在 Windows 环境下进行以避免兼容性问题。

### 🤖 Android APK 构建

#### 环境要求

- **Android Studio**: 需要安装 [Android Studio](https://developer.android.com/studio)，并配置好 Android SDK
- **JDK**: 需要安装 JDK 21 或更高版本

#### 构建步骤

```bash
# 1. 构建 Web 资源
npm run build

# 2. 同步到 Android 项目
npx cap sync android

# 3. 构建 Debug APK (无需签名)
cd android && ./gradlew assembleDebug
# APK 位置: android/app/build/outputs/apk/debug/app-debug.apk

# 4. 或构建 Release APK (需要签名配置)
cd android && ./gradlew assembleRelease
# APK 位置: android/app/build/outputs/apk/release/app-release.apk
```

#### 签名配置 (Release 版本)

如需构建签名的 Release 版本，请在 `android/app/build.gradle` 中配置签名信息，或创建 `android/keystore.properties` 文件：

```properties
storeFile=your-keystore.keystore
storePassword=your-store-password
keyAlias=your-key-alias
keyPassword=your-key-password
```

#### 使用 Android Studio

也可以通过 Android Studio 进行构建和调试：

```bash
# 打开 Android Studio
npx cap open android
```

在 Android Studio 中可以直接点击 **Build > Build Bundle(s) / APK(s) > Build APK(s)** 进行构建。

## 🏗️ 技术架构

- **核心框架**: React 19, TypeScript, Vite 6
- **样式**: Tailwind CSS, PostCSS
- **跨平台**: 
  - **Electron 40**: 桌面端封装
  - **Capacitor 8**: 移动端封装
- **算法**: `javascript-lp-solver` (单纯形法求解器)
- **图表**: Recharts
- **图标**: Lucide React

## 📂 项目结构

源代码已重构至 `src/` 目录，结构更加清晰：

```text
optimix/
├── android/                # Android 原生项目 (Capacitor)
│   ├── app/
│   │   ├── src/main/res/   # Android 资源 (图标等)
│   │   └── build.gradle    # 应用构建配置
│   └── keystore.properties # 签名配置 (需自行创建，不上传)
├── electron/               # Electron 主进程代码
├── public/                 # 静态资源 (图标等)
├── src/                    # 前端源代码
│   ├── components/         # React 组件
│   │   ├── common/         # 通用组件 (Modal 等)
│   │   ├── layout/         # 布局组件 (Sidebar)
│   │   └── views/          # 核心视图
│   │       ├── CalculationView.tsx   # 计算与优化视图
│   │       ├── ConfigurationView.tsx # 基础数据配置
│   │       ├── Optimizer.tsx         # 求解器结果展示
│   │       └── ManualAdjuster.tsx    # 手动调整面板
│   ├── services/           # 业务服务
│   │   ├── solver.ts       # 线性规划求解封装
│   │   └── excelService.ts # Excel 导入导出逻辑
│   ├── types/              # TypeScript 类型定义
│   ├── App.tsx             # 应用根组件
│   └── main.tsx            # 应用入口
├── capacitor.config.ts     # Capacitor 配置 (移动端)
├── index.html              # Web 入口模板
├── package.json            # 项目配置与脚本
├── tailwind.config.js      # 样式配置
└── vite.config.ts          # 构建配置
```

## 🤝 贡献与反馈

如果您发现任何 Bug 或有新功能建议，欢迎提交 Issue 或 Pull Request。

---

<p align="center">
  Made with ❤️ by JVxie
</p>
