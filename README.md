# 史莱姆大冒险 PC 端（Tauri）

《史莱姆大冒险 13.1》单文件 HTML 游戏的桌面封装工程，使用 [Tauri v2](https://tauri.app)（Rust 内核 + 系统 WebView），打包为独立桌面应用。

## 目录结构

```
史莱姆大冒险PC/
├── frontend/            # 游戏本体（index.html，单文件）
├── src-tauri/           # Tauri 工程
│   ├── Cargo.toml
│   ├── tauri.conf.json  # 应用配置（窗口 430×820、打包目标、图标）
│   ├── capabilities/
│   ├── icons/           # 应用图标（含 Windows .ico）
│   └── src/             # Rust 入口
└── .github/workflows/   # Windows .exe 自动构建
```

## 本机构建

### Windows（生成 .exe 安装包）
需安装：Rust（MSVC 工具链）、Node.js、Visual Studio Build Tools（含 C++ 桌面开发）。

```bash
npm install -g @tauri-apps/cli
cd src-tauri
tauri build
```

产物位于 `src-tauri/target/release/bundle/`：
- `nsis/*.exe` —— 独立安装包（安装后桌面运行）
- `msi/*.msi` —— MSI 安装包

### Linux（Ubuntu 22.04+）
```bash
sudo apt install libwebkit2gtk-4.1-dev build-essential libssl-dev \
  libayatana-appindicator3-dev librsvg2-dev libxdo-dev
cd src-tauri
cargo build --release      # 仅二进制
cargo tauri build          # 二进制 + deb/AppImage
```

## 一键 CI 构建 .exe

工程已内置 GitHub Actions（`.github/workflows/build-windows.yml`）：
1. 把本目录推送到 GitHub 仓库；
2. Actions → 「构建 Windows exe」→ Run workflow（或 push 自动触发）；
3. 完成后在 Artifacts 下载 `史莱姆大冒险-Windows`，内含 setup.exe 与 .msi。

## 说明
- 游戏窗口 430×820（与网页版视口一致），可拉伸最小 430×700。
- 游戏为纯本地单文件（无外部网络资源），离线可玩，存档走浏览器本地存储（同网页版逻辑）。
- 换图标：替换 `src-tauri/icons/` 下同名文件后重新构建。
