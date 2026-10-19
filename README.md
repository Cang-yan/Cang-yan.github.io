# 沧雁的笔记

个人博客的静态站点，基于 [Quartz v5](https://quartz.jzhao.xyz)。

## 内容从哪来

文章不存放在这个仓库。`blog/` 目录在 Gitee 私有仓库 `calm-capibara/notes` 里，由 Obsidian 编辑，
GitHub Actions 每天定时拉取后构建。仓库里只有 Quartz 的脚手架和配置。

```
Gitee 私仓 notes/blog/  →  Actions 拉取到 content/  →  quartz build  →  GitHub Pages
```

## 本地预览

需要 Node.js ≥ 22。

```bash
npm ci

# 从 vault 取一份内容（路径按你本机实际情况改）
mkdir -p content && rsync -a --delete ~/project/notes/blog/ content/

npx quartz build --serve
```

打开 http://localhost:8080 。`content/` 已被 `.git/info/exclude` 忽略，不会误提交。

## 部署

- 定时：每天 UTC 20:00（北京时间 04:00）自动构建
- 手动：Actions 页面点 `Run workflow`

Pages 的 Source 必须设为 **GitHub Actions**，不要用分支部署。

## 注意

- 这个仓库是公开的，**Actions 日志也是公开的**。workflow 里不要加 `-v`、`set -x`、
  也不要 `ls content`，否则私仓的文件名会出现在日志里。
- 公开仓库连续 60 天没有提交，GitHub 会自动停掉定时任务，且不会报错。偶尔手动跑一次。

## 一个容易踩的坑

**不要把 `content/` 写进 `.gitignore`。** Quartz 的 glob 会读取 `.gitignore`，
一旦忽略 `content/`，构建时就会"Found 0 input files"，站点直接空白。
本地用 `.git/info/exclude` 忽略它（已在仓库外配置好），这样 git 不会提交、Quartz 仍能读到。
