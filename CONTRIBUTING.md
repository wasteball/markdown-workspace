# 贡献指南

感谢你改进 Markdown Workspace Tools。本项目由两个独立 HTML 应用组成，没有构建步骤，修改应尽量保持单文件可运行和向后兼容。

## 开始之前

1. 阅读 `README.md` 和相关配置文档。
2. 确认修改属于 Markdown 阅读、编辑、导出、分享或可访问性范围。
3. 不要提交公司内部域名、账号、Bucket、Token、Cookie、业务文档或本地绝对路径。

## 本地运行

在仓库根目录启动静态服务：

```bash
python3 -m http.server 8000
```

然后分别打开两个 HTML 文件。直接使用 `file://` 也可以，但浏览器可能隔离本地存储，且部分文件系统能力行为不同。

## 代码约定

- 保持 HTML 单文件分发，不引入必须安装的构建工具。
- 优先复用已有样式、工具函数和交互模式。
- 新增外部依赖时固定版本，并更新 `THIRD_PARTY_NOTICES.md`。
- 新增浏览器存储项时使用 `md-workspace:` 命名空间，并记录用途和迁移策略。
- 对 Markdown/HTML 输入使用结构化解析和现有清理流程，不绕过 DOMPurify。
- 不手工绘制已有图标库可以表达的复杂图标；本项目当前图标以轻量文本符号为主，应保持风格一致。
- 注释只解释不明显的约束、兼容性或算法原因。

## 必测流程

两个工具都需要验证：

- 打开单文件、多文件和文件夹。
- 新建、编辑、撤销、重做、保存。
- 阅读视图与完整源码视图切换。
- 第一次和第二次选中文本后触发查找，输入框均更新为最新选择。
- 单处替换、全部替换及撤销。
- 表格、任务列表、Callout、脚注、KaTeX、Mermaid 和代码块。
- 深色、浅色及 390px 左右的手机视口。
- 未配置分享服务时的引导。
- 配置保存、重载、复制、导入和重置。
- multipart 上传字段、请求头、响应 URL 路径、历史和去重。

还需要分别验证：

- HTML Workspace 导出的文件以 `<!DOCTYPE html>` 开头，且不包含分享配置或 Token。
- Word Workspace 导出的文件是有效 DOCX ZIP 包，并能由 Microsoft Word 或兼容软件打开。

可使用 `examples/feature-demo.md` 作为基础测试文档。

## 安全检查

提交前检查所有硬编码 URL、邮箱、账号和疑似凭据。下面的命令只用于辅助，结果需要人工确认：

```bash
rg -ni "api[_-]?key|secret|token|password|authorization|access[_-]?key|cookie|bucket|usercode" .
rg -n -o "https?://[^\"'<> )]+" .
```

示例中的 `YOUR_TOKEN`、`example.com` 等占位值是允许的，真实值不允许进入仓库。

## Pull Request

- 一个 Pull Request 聚焦一个问题。
- 说明用户可观察到的变化和兼容性影响。
- 列出已经验证的浏览器、视口和导出格式。
- 界面改动附桌面和手机截图。
- 新功能同步更新 README、示例和变更记录。
