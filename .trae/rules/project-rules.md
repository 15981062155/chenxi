# 项目规则 — 陈曦的网页课堂实践

## 适用范围

本规则适用于本项目中所有**新建或修改的** HTML、CSS、JavaScript 文件。

---

## 一、命名规范

所有用户自定义名称必须以 `cx_` 为前缀（`cx` 代表"陈曦"）。

| 类型 | 前缀 | 示例 |
|------|------|------|
| JavaScript 变量 | `cx_` | `cx_count`, `cx_userList`, `cx_isLoading` |
| JavaScript 函数 | `cx_` | `cx_fetchData()`, `cx_handleClick()` |
| JavaScript 常量 | `CX_`（全大写） | `CX_API_BASE`, `CX_MAX_COUNT` |
| CSS 类名 | `cx_` | `.cx_container`, `.cx_button`, `.cx_card-title` |
| HTML id | `cx_` | `cx_header`, `cx_main-content`, `cx_nav` |
| CSS 自定义属性 | `--cx-` | `--cx-primary-color`, `--cx-font-size` |

**不使用前缀的情况**：HTML 标签名（`div`、`span` 等）、CSS 属性名、HTML 全局属性（`class`、`id`、`href` 等）、标准 DOM API 名称。

---

## 二、页面注释规范

每个 HTML 页面必须在 `<head>` 内（`<meta charset="UTF-8">` 之后）添加以下格式的注释：

```html
<!-- 姓名：陈曦，日期：YYYY-MM-DD，功能：页面功能简述 -->
```

**示例**：
```html
<head>
    <meta charset="UTF-8">
    <!-- 姓名：陈曦，日期：2026-10-22，功能：海洋生物图册展示页面 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
```

---

## 三、技术栈规范

### 1. 原生实现
仅使用原生 HTML5、CSS3、原生 JavaScript（ES6+）。不引入任何框架或库（如 React、Vue、jQuery、Bootstrap 等）。

### 2. 图标：使用 iconfont
统一使用项目 `font/` 目录下的 iconfont 图标（`iconfont.css` + `iconfont.ttf` 等字体文件）。

引入方式：
```html
<link rel="stylesheet" href="font/iconfont.css">
```

使用方式：
```html
<i class="iconfont icon-search"></i>
```

### 3. 不使用 CDN
所有资源必须使用**本地文件**。禁止使用外部 CDN 链接（如 `cdn.jsdelivr.net`、`cdnjs.cloudflare.com`、`fonts.googleapis.com`、`unpkg.com` 等）。

包括但不限于：图标库、字体库、CSS 框架、JavaScript 库。

### 4. 数据请求：使用 fetch
与后端服务器交互统一使用原生 `fetch` API，不使用 `XMLHttpRequest`、`axios` 等。

示例：
```javascript
async function cx_fetchData(url) {
    const response = await fetch(url);
    if (!response.ok) {
        throw new Error(`请求失败: ${response.status}`);
    }
    return response.json();
}
```
