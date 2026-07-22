# 发布检查表

## 1. 范围与命名

- [ ] 发布目录只包含开源 HTML、公开文档和脱敏示例。
- [ ] 不包含内部版 HTML、业务文档、浏览器配置、测试上传记录或本地环境文件。
- [ ] 文件名、页面 `<title>`、顶部品牌和 README 保持一致。
- [ ] CHANGELOG 已更新，版本号和发布日期已确认。

## 2. 许可证与版权

- [ ] 已获得代码开源所需的公司或作者授权。
- [x] 仓库根目录存在正式 MIT `LICENSE`。
- [x] README 已写明 MIT License。
- [ ] THIRD_PARTY_NOTICES 已与 HTML 中的依赖版本核对。

## 3. 敏感信息

- [ ] 人工检查所有 HTTP(S) URL、邮箱、用户名、Bucket 和文件库地址。
- [ ] 搜索 API Key、Token、Cookie、密码、Authorization、Access Key 等疑似凭据。
- [ ] 检查 Git 历史，而不只是当前工作区。
- [ ] 示例配置只使用 `example.com`、`YOUR_TOKEN` 等占位值。
- [ ] 截图不包含内部文档标题、文件路径、浏览器账号或上传地址。

辅助命令：

```bash
rg -ni "api[_-]?key|secret|token|password|authorization|access[_-]?key|cookie|bucket|usercode" .
rg -n -o "https?://[^\"'<> )]+" .
```

## 4. 静态检查

- [ ] 两个 HTML 中的所有内联脚本均可被 JavaScript 解析器解析。
- [ ] 所有 Markdown 链接指向存在的相对路径。
- [ ] `examples/share-service.example.json` 是合法 JSON。
- [ ] 页面只引用预期的公共 CDN 和示例域名。

## 5. 浏览器回归

- [ ] Chrome/Edge 最新稳定版，桌面 1440 x 1000。
- [ ] 手机视口 390 x 844，无横向溢出和控件重叠。
- [ ] 至少抽查 Firefox 或 Safari 的主要流程。
- [ ] 本地 HTTP 和 `file://` 两种打开方式均有基本验证。
- [ ] 首次和第二次选中文本都能自动带入查找框。

## 6. 文件与编辑

- [ ] 打开单文件、多文件和文件夹。
- [ ] URL 加载、GitHub blob 转换及错误提示。
- [ ] 新建、编辑、撤销、重做和保存。
- [ ] File System Access 原地保存及下载回退。
- [ ] 未保存改动和外部改动提示。

## 7. 渲染与导出

- [ ] 表格、任务列表、Callout、脚注、目录、代码、KaTeX 和 Mermaid 正常。
- [ ] HTML 导出可独立打开，不包含编辑脚手架和分享配置。
- [ ] Print/PDF 布局可读。
- [ ] DOCX 是有效 ZIP 包，能在 Word 中打开并继续编辑。
- [ ] 跨域图片失败时有明确占位或提示。

## 8. 分享服务

- [ ] 无配置时出现配置引导。
- [ ] 保存、刷新后恢复、Copy JSON、Import 和 Reset 均正常。
- [ ] multipart 文件字段、Bucket、用户、附加字段和请求头与配置一致。
- [ ] 自定义响应 URL 路径和相对 URL 正常。
- [ ] 未变化内容复用链接；账号、请求头或租户变化后不会复用旧缓存。
- [ ] 导出的 HTML 和 DOCX 不包含配置或 Token。

## 9. 发布产物

- [ ] 为两个 HTML 生成并公布 SHA-256。
- [ ] 从全新浏览器配置打开最终下载文件做一次冒烟测试。
- [ ] Git tag、Release 标题和附件版本一致。
- [ ] Release Notes 包含变更、已知限制和升级说明。
