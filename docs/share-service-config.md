# 分享服务配置说明

本文适用于以下两个开源工具：

- `md-html-workspace.html`
- `md-word-workspace.html`

两个工具不会内置任何上传地址、账号或 Token。分享功能使用你配置的 HTTP multipart 上传接口，配置只保存在当前浏览器的 `localStorage` 中。

## 快速配置

1. 打开任意一个工具。
2. 点击右上角 `Aa`，展开 `Share service`。
3. 手工填写配置，或者点击 `Import` 并粘贴完整 JSON。
4. 保存后打开 Markdown 文档，点击 `Share`，选择分享 HTML 或 Markdown。

可以先用下面的通用模板，再替换成自己的服务信息：

```json
{
  "apiUrl": "https://your-domain.example/api/upload",
  "fileField": "file",
  "responseUrlPath": "data.url",
  "bucket": "",
  "bucketField": "bucket",
  "userCode": "",
  "userField": "userCode",
  "headers": {},
  "extraFields": {},
  "fileLibraryUrl": "",
  "shareFormat": "name_url",
  "autoCopy": true,
  "withCredentials": false
}
```

## 配置字段

| 界面字段 | JSON 字段 | 是否必填 | 说明 |
| --- | --- | --- | --- |
| Upload API URL | `apiUrl` | 是 | 接收 multipart POST 请求的完整 HTTP(S) 地址 |
| File field | `fileField` | 是 | 文件在 multipart 表单中的字段名，常见值为 `file` |
| Response URL path | `responseUrlPath` | 是 | 从 JSON 响应中读取文件链接的点路径，例如 `data.url` |
| Bucket value | `bucket` | 否 | Bucket、空间或容器值；接口不需要时留空 |
| Bucket field | `bucketField` | Bucket 有值时必填 | Bucket 在 multipart 表单中的字段名 |
| User / tenant value | `userCode` | 否 | 用户、租户或项目值；接口不需要时留空 |
| User field | `userField` | User 有值时必填 | 用户或租户在 multipart 表单中的字段名 |
| Request headers JSON | `headers` | 否 | 请求头对象，常用于 Token 鉴权 |
| Additional form fields JSON | `extraFields` | 否 | 除文件、Bucket、用户以外的 multipart 字段 |
| Optional file library URL | `fileLibraryUrl` | 否 | 上传成功后可跳转的文件管理页面 |
| Copied link format | `shareFormat` | 否 | 复制到剪贴板的链接格式 |
| Auto-copy link | `autoCopy` | 否 | 上传完成后是否自动复制分享内容 |
| Send cookies | `withCredentials` | 否 | 跨域请求时是否携带浏览器 Cookie |

## 请求格式

工具始终发送 `POST multipart/form-data`。假设配置如下：

```json
{
  "fileField": "document",
  "bucket": "public-files",
  "bucketField": "space",
  "userCode": "tenant-01",
  "userField": "tenant",
  "extraFields": {
    "visibility": "public",
    "source": "markdown-tool"
  }
}
```

服务端将收到这些表单字段：

| 字段 | 内容 |
| --- | --- |
| `document` | 当前导出的 `.html` 或 `.md` 文件 |
| `space` | `public-files` |
| `tenant` | `tenant-01` |
| `visibility` | `public` |
| `source` | `markdown-tool` |

不要在 `headers` 中手工设置 `Content-Type`。浏览器会自动生成 multipart boundary；手工覆盖通常会导致服务端无法解析文件。

## 响应链接

`responseUrlPath` 使用点号读取 JSON 字段。

响应为：

```json
{
  "data": {
    "url": "https://cdn.example.com/files/report.html"
  }
}
```

对应配置：

```text
data.url
```

以下常见路径即使没有显式配置也会作为后备尝试：

- `url`
- `data.url`
- `downloadUrl`
- `data.downloadUrl`

服务端可以返回相对地址，例如 `/files/report.html`，工具会根据 `apiUrl` 转换成完整 HTTP(S) 地址。

## 请求头和鉴权

Bearer Token 示例：

```json
{
  "Authorization": "Bearer YOUR_TOKEN",
  "X-Tenant-Id": "tenant-01"
}
```

如果服务使用浏览器登录 Cookie，将 `Send cookies` 打开。服务端同时需要正确配置 CORS，例如允许工具所在的 Origin、所需请求头及凭证。

## 链接复制格式

| `shareFormat` | 复制结果 |
| --- | --- |
| `name_url` | 第一行文件名，第二行 URL |
| `markdown` | `[文件名](URL)` |
| `inline` | `文件名 - URL` |
| `url` | 仅 URL |

## 浏览器记忆与复用

- 配置保存在 `localStorage` 的 `md-workspace:share-service-v1` 中。
- 两个工具通过同一 HTTP(S) Origin 打开时，会自动共享同一份配置。
- 直接双击并以 `file://` 打开时，不同浏览器可能按文件隔离存储。此时在第一个工具点击 `Copy JSON`，再到第二个工具点击 `Import` 即可。
- `Reset` 会清除分享配置、本地分享历史和分享去重缓存。

## 安全说明

- 请求头中的 Token 会以明文保存在当前浏览器的 `localStorage`，只应在可信设备上使用。
- `Copy JSON` 会连同请求头一起复制，分享配置 JSON 前必须检查其中是否包含 Token。
- 配置不会写入 HTML 源文件，也不会进入导出的 HTML 或 Word 文档。
- 不要把真实公司的 API、Bucket、账号或 Token 写入准备公开的 HTML、README 或示例配置。

## 排错

### 点击 Share 后提示未配置

确认 `Upload API URL` 已填写并点击 `Save`。导入 JSON 后会自动保存。

### 浏览器提示网络错误或 CORS

确认上传接口允许当前页面的 Origin。携带自定义请求头时，服务端还必须正确响应浏览器的 OPTIONS 预检请求。

### HTTP 成功但提示找不到文件链接

查看接口实际 JSON 响应，把 `Response URL path` 设置为文件 URL 所在路径。例如响应字段是 `result.file.href`，配置值就填写 `result.file.href`。

### 服务端收不到文件

确认 `File field` 与后端接收文件的字段名完全一致，并确认没有手工设置 `Content-Type`。

### 修改账号后仍出现旧链接

当前版本会将上传地址、请求头、Bucket、用户和附加表单字段纳入去重指纹。修改并保存配置后，同一文档会重新上传，不会复用旧账号下的缓存链接。
