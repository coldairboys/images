# images
# 将 GitHub 仓库变为图床：最小可用方案（含自查清单）

## TL;DR

- 用 GitHub 仓库 + jsDelivr（或 GitHub Raw）即可把图片当静态资源外链使用。
- 最小实践：新建 repo → 建 `images/` 目录 → 上传图片 → 生成外链 → 在 Markdown 里引用。
- 关键点：**仓库公开**（否则外链访问受限）、**路径稳定**、**避免频繁改文件名**。

---

## 适用场景

- 写 Markdown 笔记/博客，需要稳定的图片外链
- 不想购买/维护独立图床服务

## 方案 A：jsDelivr（推荐）

jsDelivr 是一个 CDN，可以把 GitHub 上的静态文件加速后外链。

### 1）准备仓库结构

建议结构：

- `images/`：放所有图片
- `README.md`：仓库说明（可选）

命名建议：

- 用日期或主题做前缀，保证唯一性，例如：`2026-04-14-github-cdn-demo.png`

### 2）上传图片

- 直接在 GitHub 网页端上传，或用 Git 推送

### 3）生成 jsDelivr 外链

格式：

- `https://cdn.jsdelivr.net/gh/<用户名>/<仓库名>@<分支或tag>/<文件路径>`

示例：

- `https://cdn.jsdelivr.net/gh/username/repo@main/images/demo.png`

> 小提示：如果你后续会“持续更新图片”，用 `@main` 更省事；如果你追求强稳定性，建议用 `@tag`（发布一个 tag 后引用）。
> 

### 4）在 Markdown 中引用

```markdown
![说明文字](https://cdn.jsdelivr.net/gh/username/repo@main/images/demo.png)
```

---

## 方案 B：GitHub Raw（不经 CDN）

如果不想用 CDN，也可以直接用 Raw 链接。

### 获取方式

在 GitHub 打开图片文件 → 右上角 `Download` / `Raw` → 复制链接。

Raw 链接通常类似：

- `https://raw.githubusercontent.com/<用户名>/<仓库名>/<分支>/<文件路径>`

示例：

- `https://raw.githubusercontent.com/username/repo/main/images/demo.png`

---

## 自查清单（排错用）

- [ ]  仓库是否为 Public（公开）？
- [ ]  链接里的分支名是否正确（`main` 还是 `master`）？
- [ ]  文件路径大小写是否一致（Linux 路径大小写敏感）？
- [ ]  图片是否被你重命名/移动导致 404？
- [ ]  如果用 jsDelivr：是否刚上传完就访问（可能需要等待 CDN 刷新）？

---

## 最佳实践（长期维护）

- 给图片做“不可变命名”（不要后改名）
- 重要文章的图片用 `tag` 固定版本（例如 `v1.0.0`）
- 大量图片时按主题分文件夹：`images/markdown/`、`images/github/` 等

## 常见坑

- **私有仓库**：外链一般无法公开访问或会失效。
- **引用 latest/未固定版本**：你替换同名文件后，CDN 可能缓存旧资源。
- **图片过大**：加载慢，建议先压缩（WebP/PNGQuant 等）。
