# Markdown Workspace Tools

两个无需安装、无需构建的单文件 Markdown 工具。直接在浏览器中打开，即可完成 Markdown 阅读、编辑、查找替换、变更审阅和文档导出。

## 工具

| 文件 | 主要用途 | 导出格式 |
| --- | --- | --- |
| `md-html-workspace.html` | Markdown 阅读、编辑与网页交付 | HTML、打印/PDF、Markdown |
| `md-word-workspace.html` | Markdown 阅读、编辑与 Word 交付 | DOCX、打印/PDF、Markdown |

两份 HTML 均为独立应用，没有后端依赖。分享功能是可选项，只有主动配置并点击分享后才会上传文档。

## 主要功能

- 打开单个或多个 Markdown 文件，也可以打开整个文件夹。
- 从 URL 加载 Markdown，GitHub blob 地址会自动转换为 raw 地址。
- 阅读界面直接编辑，支持块、表格行列、列表和常见行内格式。
- 完整源码模式、撤销/重做、保存和未保存改动提示。
- 查找与替换；选中文本后触发查找会自动带入选中内容。
- 本次未保存改动与外部文件变动的可视化审阅。
- GFM 表格、任务列表、脚注、缩写、目录、Callout、代码高亮、KaTeX 公式和 Mermaid 图表。
- 深色/浅色/自动主题、强调色、字体、字号、阅读宽度和自定义 CSS。
- HTML Workspace 导出独立 HTML，Word Workspace 导出可编辑 DOCX。
- 通用 multipart 分享服务配置，支持请求头、附加表单字段、响应路径和浏览器记忆。

## 快速开始

### 直接打开

下载任意一个 HTML 文件并双击打开。然后把 `examples/feature-demo.md` 拖进页面，即可查看主要渲染能力。

### 使用本地 HTTP 服务

推荐通过同一 Origin 打开两个工具，这样主题和分享服务配置可以自动复用。

```bash
python3 -m http.server 8000
```

Windows 也可以使用：

```powershell
py -m http.server 8000
```

浏览器访问：

- `http://127.0.0.1:8000/md-html-workspace.html`
- `http://127.0.0.1:8000/md-word-workspace.html`

本地 HTTP 服务只是静态文件服务，不会接收或处理文档内容。

## 基本使用

1. 点击 `Open` 打开 Markdown 文件，或使用下拉菜单打开文件夹、URL、新建文档。
2. 点击 `Edit` 在阅读界面直接编辑；再次点击退出编辑。
3. 点击查找图标，或按 `Ctrl/Command + F` 查找；按 `Ctrl/Command + H` 展开替换。
4. 点击 `Save` 保存 Markdown。
5. 在 HTML Workspace 中点击 `Export` 导出独立 HTML。
6. 在 Word Workspace 中点击 `Export Word` 导出 `.docx`。

在支持 File System Access API 的 Chromium 浏览器中，通过文件选择器打开的文件可以原地保存。其他浏览器会退化为下载保存。

## 常用快捷键

| 操作 | Windows/Linux | macOS |
| --- | --- | --- |
| 打开文件 | `Ctrl + O` | `Command + O` |
| 打开文件夹 | `Ctrl + Shift + O` | `Command + Shift + O` |
| 保存 | `Ctrl + S` | `Command + S` |
| 切换阅读编辑 | `Ctrl + E` | `Command + E` |
| 切换完整源码 | `Ctrl + Shift + E` | `Command + Shift + E` |
| 查找 | `Ctrl + F` | `Command + F` |
| 查找替换 | `Ctrl + H` | `Command + H` |
| 下一处匹配 | `F3` 或 `Ctrl + G` | `F3` 或 `Command + G` |
| 撤销/重做 | `Ctrl + Z` / `Ctrl + Shift + Z` | `Command + Z` / `Command + Shift + Z` |
| 显示改动清单 | `Ctrl + Shift + D` | `Command + Shift + D` |
| 上一处/下一处改动 | `Alt + Up/Down` | `Option + Up/Down` |

## 分享服务

开源文件不包含任何上传地址、Bucket、账号或 Token。进入 `Aa -> Share service` 手工配置，或导入 `examples/share-service.example.json` 后修改。

完整字段和服务端响应要求见 [分享服务配置说明](docs/share-service-config.md)。

配置保存在浏览器 `localStorage`。请求头中的 Token 也是明文保存，只应在可信设备上使用。导出的 HTML 和 Word 文档不会携带分享服务配置。

## 数据与网络边界

| 操作 | 是否发起网络请求 |
| --- | --- |
| 打开、编辑、保存本地 Markdown | 否 |
| 页面首次加载 | 是，从公共 CDN 加载字体和渲染依赖 |
| 从 URL 加载 Markdown | 是，请求目标 URL |
| 开启 CORS proxy 后加载 URL | 是，请求 `corsproxy.io` |
| 加载远程图片 | 是，请求图片地址 |
| 导出本地 HTML、PDF 或 Word | 不上传文档；远程图片可能在导出时被读取 |
| 点击 Share | 是，上传到用户配置的服务 |

如果需要完全离线运行，需要将 CDN 依赖本地化并修改 HTML 引用；当前发布包未内置第三方库。

## 浏览器支持

- 推荐最新版 Chrome、Edge 或其他 Chromium 浏览器。
- Firefox 和 Safari 可以完成大部分阅读、编辑与导出流程，但原地文件保存等能力可能退化。
- Word 导出依赖现代浏览器的 Blob、Canvas、Fetch 和 Promise 支持。

## 已知限制

- 复杂 MDX/JSX 不会作为 React 组件执行，只会按 Markdown/HTML 能力处理。
- Word 导出关注可编辑结构，不保证与浏览器 CSS 像素级一致。
- 跨域图片缺少 CORS 权限时，Word 中会使用文字占位。
- `file://` 下不同 HTML 的浏览器存储可能互相隔离；可使用 `Copy JSON` 和 `Import` 迁移分享配置。
- 页面运行依赖公共 CDN，网络不可用时部分增强渲染和 Word 导出会不可用。

## 仓库结构

```text
.
├── .github/                  # Issue 与 Pull Request 模板
├── md-html-workspace.html
├── md-word-workspace.html
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── SECURITY.md
├── THIRD_PARTY_NOTICES.md
├── LICENSE
├── SHA256SUMS
├── docs/
│   ├── RELEASE_CHECKLIST.md
│   └── share-service-config.md
└── examples/
    ├── feature-demo.md
    └── share-service.example.json
```

## 参与贡献

提交修改前请阅读 [CONTRIBUTING.md](CONTRIBUTING.md)。安全问题请按 [SECURITY.md](SECURITY.md) 中的方式报告。

## 第三方依赖

运行时依赖及许可证见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。

两个可执行 HTML 的发布哈希见 `SHA256SUMS`。

## 许可证

本项目采用 [MIT License](LICENSE)。你可以使用、复制、修改、合并、发布、分发、再许可或销售软件副本，但必须保留原版权及许可声明。
