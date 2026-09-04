# 轻之国度下载器 (Lightnovel Downloader)

一个用于轻之国度（[lightnovel.fun](https://www.lightnovel.fun/)）的 Tampermonkey / Violentmonkey 脚本，支持将书籍章节提取并打包为规范的 EPUB 或 TXT 电子书。

## 安装说明

1. 确保浏览器已安装 [Tampermonkey](https://www.tampermonkey.net/) 或 Violentmonkey 扩展程序。
2. 选择以下任一来源安装脚本：

| Greasy Fork | GitHub 源 | jsDelivr 源 |
| --- | --- | --- |
| [安装](https://greasyfork.org/zh-CN/scripts/594307-%E8%BD%BB%E4%B9%8B%E5%9B%BD%E5%BA%A6%E4%B8%8B%E8%BD%BD%E5%99%A8) | [安装](https://raw.githubusercontent.com/RRRRUDDDD/Lightnovel_Downloader/main/lightnovel_downloader.user.js) | [安装](https://cdn.jsdelivr.net/gh/RRRRUDDDD/Lightnovel_Downloader@main/lightnovel_downloader.user.js) |

## 使用说明

### 下载书籍

1. 进入轻之国度的书籍详情页。
2. 在页面原生操作区“下载APP”右侧点击 **“扒书”**，打开下载设置弹窗。
3. 选择下载格式（EPUB / TXT / EPUB + TXT）；书籍有多个版本时，先切换到要下载的版本。
4. 选择章节，可按需关闭 **“EPUB 内嵌插图”**，并调整请求间隔。
5. 点击 **“开始扒书”**，等待抓取和打包完成，文件会自动保存至浏览器下载目录。

### 目录读取

脚本会优先使用书籍详情页返回的完整目录。目录分页或暂未完整载入时，会从首章开始沿章节的 `nextChapterId` 自动遍历；目录为空时也会尝试使用站点首章重定向作为入口。

### 输出文件

- EPUB 文件包含封面（若书籍提供）、书名页、可视目录页、正文和 NCX 兼容目录。
- TXT 文件包含书籍元数据、简介、卷名、章节标题和正文；锁定或无权限读取的章节不会被绕过。
- 若部分章节最终抓取失败，TXT 末尾会列出失败章节及错误信息，完成提示也会汇总失败数量。

## 注意事项

- **权限限制** — 脚本只保存当前账号实际能够读取的内容，不会绕过锁定、付费或登录权限；请先保持站点登录状态。
- **请求间隔** — 长书会产生较多请求，默认每章间隔 600 ms，可在下载弹窗中调大间隔以降低对站点的压力。
- **图片下载** — EPUB 图片受图片域名、浏览器跨域策略和链接有效性影响；无法嵌入时会保留原图片 URL 或跳过该图片。
- **失败重试** — 站点返回 Cloudflare 524、502 等临时错误时会自动重试；持续失败的章节会记录在 TXT 输出末尾。

## 技术栈

- **打包引擎** — [fflate](https://github.com/101arrowz/fflate) (ZIP/DEFLATE)
- **运行环境** — Tampermonkey / Violentmonkey
- **数据解析** — 轻之国度 Nuxt `__NUXT_DATA__` 与站点章节接口
- **网络请求** — Fetch、`GM_xmlhttpRequest`
