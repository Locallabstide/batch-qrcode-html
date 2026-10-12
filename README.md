# batch-qrcode-html

**批量二维码生成工具** —— 单文件、零依赖、纯前端。

一张表格进去，一堆带样式的二维码出来。**全程在你自己的浏览器里完成，不联网、不上传。**

[![在线使用](https://img.shields.io/badge/🚀_在线使用-立即打开-2f6bff?style=for-the-badge)](https://locallabstide.github.io/batch-qrcode-html/)
[![License](https://img.shields.io/badge/license-MIT-16a34a?style=for-the-badge)](LICENSE)

> 🚀 **在线直接用**：https://locallabstide.github.io/batch-qrcode-html/

---

## 这是什么

你手上有 200 行数据（门店名 + 链接），要为每行做一张二维码图片，还得按门店名命名。
一个个去在线网站生成、下载、改名，能折腾一下午 —— 而且数据要上传到别人服务器。

**这个工具**：把表格丢进去，一次生成全部二维码（带颜色、LOGO、门店名文字），
自动命名、打包成 ZIP 下载。几秒钟完成，数据一步都没离开你的电脑。

## 工作原理

![工作原理](docs/workflow.svg)

## 快速开始

### 方式一：在线直接用（推荐）

点这个链接即可，无需下载：

**https://locallabstide.github.io/batch-qrcode-html/**

### 方式二：下载到本地双击

下载 [`app.html`](app.html)，用 **Chrome / Edge** 双击打开。断网也能用。

### 方式三：嵌入自己的网页

把 `app.html` 放到任意静态目录，或用 iframe 嵌入：

```html
<iframe src="app.html" width="100%" height="800"></iframe>
```

## 功能

- **两列数据** —— 第一列「图片名称」（也是文件名与可选显示文字），第二列「二维码内容」；名称为空时自动编号
- **多种导入** —— 手动粘贴，或导入 `.xlsx` / `.csv` / `.txt`；提供规范 CSV 模板一键下载
- **数据整理** —— 查找替换、去首尾空格、去除重复行、删除空行；实时统计重复 / 超长 / 缺内容
- **样式可调** —— 尺寸、静区边距、前景/背景色、四档容错率（L/M/Q/H）、方形 / 圆点 / 圆角样式
- **LOGO 支持** —— 上传 LOGO，占比 10%–25%，可选白色留白衬底
- **展示文字** —— 二维码上方或下方印文字，来源可选名称 / 内容 / 固定文案
- **多格式导出** —— PNG（可透明）/ JPG / SVG（纯矢量，不含 LOGO 与文字）
- **批量打包** —— 一键导出为 ZIP；自研 PKZIP 实现，零第三方库

## 数据格式

```
图片名称, 二维码内容
门店A-桌贴, https://example.com/a
门店B-海报, https://example.com/b
物料M001, 商品编号 A-001
https://example.com/只有内容也行
```

- 分隔符用**英文逗号**或 **Tab**（从 Excel 直接复制粘贴过来就是 Tab）
- 只写一列时，该列视为「内容」，名称自动编号
- 名称列会用作输出文件名（自动清洗 `\ / : * ? " < > |`）
- 内容含逗号 / 引号 / 换行时，用英文双引号包起来即可

## 技术要点

- **二维码内核** —— 纯 JS 实现的 QR 生成器，版本 1–40、UTF-8 中文、四档容错率、8 种掩码自动择优。容量表取自 ISO/IEC 18004 官方数据并做过公式自校验。
- **xlsx 解析** —— 自研 ZIP 中央目录解析 + 浏览器原生 `DecompressionStream` + sharedStrings / sheet XML 抽取。**不依赖 SheetJS**。
- **ZIP 打包** —— 手写 PKZIP（CRC32 + `CompressionStream('deflate-raw')`，不支持时回退存储模式）。
- **预览清晰度** —— 按 `min(4, max(2, devicePixelRatio))` 高倍率渲染再等比缩小，放大也锐利。
- **单文件 IIFE 架构**，`"use strict"`，无外链、无 CDN。

## 文件说明

| 文件 | 说明 |
|---|---|
| `index.html` | 项目首页（GitHub Pages 入口） |
| `app.html` | 批量二维码生成工具（单文件，可直接使用） |
| `user-guide.html` | 用户手册（面向使用者） |
| `dev-guide.html` | 开发者手册（源码级技术文档） |
| `docs/workflow.svg` | 工作原理示意图 |

## 已验证

- 二维码内核经 ZXing 解码器实测：覆盖版本 1–34、L/M/Q/H 全部容错等级、中英文与混合内容，**全部可正常扫描**。
- CSV 往返无损：名称含逗号、内容含逗号、名称含引号、内容含换行、中文混合，**全部正确**。
- ZIP 打包经 Python `zipfile` 校验可正常解压，中文文件名与内容无损。
- 含展示文字的 SVG 还原为位图后仍可正常解码。

## 浏览器支持

| 浏览器 | 支持情况 |
|---|---|
| Chrome 103+ | ✅ 完整支持 |
| Edge 103+ | ✅ 完整支持 |
| Safari 16.4+ | ✅ 完整支持 |
| Firefox 113+ | ✅ 支持（xlsx 导入需 113+） |

> 依赖 `CompressionStream` / `DecompressionStream`。老版本浏览器下 ZIP 会回退为存储模式，xlsx 导入不可用。

## 隐私说明

所有处理均在你的浏览器本地完成，**不存在任何网络请求**，数据不会离开你的电脑。可断网使用验证。

## 作者

**Yulai**

- E-mail：liyulai@qq.com
- GitHub：[@Locallabstide](https://github.com/Locallabstide)

## License

MIT License — 详见 [LICENSE](LICENSE)

---

*batch-qrcode-html · Powered by Yulai @2026*
