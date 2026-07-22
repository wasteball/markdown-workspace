# Changelog

本项目采用 Keep a Changelog 的组织方式。正式发布版本后建议使用语义化版本编号。

## Unreleased

### Added

- 独立开源发布目录及维护文档。
- 通用分享服务配置示例和功能演示 Markdown。
- 贡献、安全、第三方依赖和发布检查说明。
- MIT License。

### Changed

- HTML 工具的公开名称统一为 MD HTML Workspace。
- 发布文件改用不含内部版本后缀的稳定文件名。

### Security

- Markdown 和 Mermaid 渲染改为清理失败时安全降级，不再插入未清理 HTML。
- Mermaid 使用严格安全模式，并在挂载图表前再次清理 SVG。
- 嵌入式文件保存和剪贴板消息仅允许同源父页面，阻止跨源页面接收文档内容。
- 导出 HTML 会拦截自定义 CSS 的标签逃逸，阅读编辑器会拒绝危险链接协议。

## Public baseline - 2026-07-21

### Added

- Markdown 文件、文件夹、URL和新建文档工作流。
- 阅读界面编辑、源码编辑、查找替换、撤销重做和变更审阅。
- 表格、任务列表、Callout、脚注、目录、代码高亮、公式和 Mermaid 支持。
- HTML、打印/PDF 和可编辑 DOCX 导出。
- 可配置的 multipart 分享服务及浏览器配置记忆。
- 分享历史、内容去重和完整配置身份隔离。

### Security

- 移除内部 API、Bucket、账号、文件库地址和旧公司命名空间。
- 分享配置不写入应用 HTML，也不进入导出的 HTML 或 Word 文档。
- 服务端返回链接限制为 HTTP(S)。
