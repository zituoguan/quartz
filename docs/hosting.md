---
title: 06 托管
---

Quartz 可以将你的 Markdown 文件和其他资源高效地打包为 HTML、JS 和 CSS 文件（即一个网站！）。

不过，如果你想让全世界都能访问你的网站，你需要将其托管到线上。本指南将详细介绍如何使用常见的托管服务进行部署，但任何支持静态 HTML 部署的服务都可以使用。

> [!warning]
> 本指南假设你已经为 Quartz 创建了自己的 GitHub 仓库。如果还没有，请先[[setting up your GitHub repository|完成这一步]]。

> [!hint]
> 某些 Quartz 功能（如 [[RSS Feed]] 和站点地图生成）需要在 [[configuration]] 中正确配置 `baseUrl`。请在部署前设置好！

## Cloudflare Pages

1. 登录 [Cloudflare 控制台](https://dash.cloudflare.com/)，选择你的账户。
2. 在账户主页选择 **Workers & Pages** > **Create application** > **Pages** > **Connect to Git**。
3. 选择你新建的 GitHub 仓库，并在 **Set up builds and deployments** 部分填写以下信息：

| 配置项                | 值                  |
| --------------------- | ------------------- |
| Production branch     | `v4`                |
| Framework preset      | `None`              |
| Build command         | `npx quartz build`  |
| Build output directory| `public`            |

点击 "Save and deploy"，Cloudflare 会在大约一分钟内部署你的网站。之后，每次你将 Quartz 的更改同步到 GitHub，网站都会自动更新。

如需添加自定义域名，请参考 [Cloudflare 官方文档](https://developers.cloudflare.com/pages/platform/custom-domains/)。

> [!warning]
> Cloudflare Pages 默认执行浅克隆，如果你依赖 `git` 获取时间戳，建议在构建命令前加上 `git fetch --unshallow &&`（如：`git fetch --unshallow && npx quartz build`）。

## GitHub Pages

在本地 Quartz 项目中新建文件 `quartz/.github/workflows/deploy.yml`。

```yaml title="quartz/.github/workflows/deploy.yml"
name: Deploy Quartz site to GitHub Pages

on:
  push:
    branches:
      - v4

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0 # 获取完整历史以便 git 信息
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - name: 安装依赖
        run: npm ci
      - name: 构建 Quartz
        run: npx quartz build
      - name: 上传构建产物
        uses: actions/upload-pages-artifact@v3
        with:
          path: public

  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: 部署到 GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

然后：

1. 进入你 Fork 的仓库的 "Settings" 标签页，在侧边栏点击 "Pages"，在 "Source" 选择 "GitHub Actions"。
2. 提交这些更改并执行 `npx quartz sync`。这会将你的网站部署到 `<github-username>.github.io/<repository-name>`。

> [!hint]
> 如果遇到因环境保护规则无法部署到 `github-pages` 的错误，请删除已有的 GitHub Pages 环境。
>
> 进入 GitHub 仓库的 Settings 页面，点击 Environments 标签，点击垃圾桶图标删除。下次同步 Quartz 时，GitHub Action 会自动重新创建环境。

> [!info]
> Quartz 生成的文件格式为 `file.html` 而不是 `file/index.html`，这意味着非文件夹路径的链接不会有斜杠结尾。GitHub Pages 不会自动重定向，可能导致原有带斜杠的链接失效。如果你需要兼容旧链接（如从 Quartz 3 迁移），建议使用 [[#Cloudflare Pages]]。

### 自定义域名

为 GitHub Pages 部署添加自定义域名的方法如下：

1. 进入你 Fork 的仓库的 "Settings" 标签页。
2. 在侧边栏 "Code and automation" 部分点击 "Pages"。
3. 在 "Custom Domain" 输入你的自定义域名并点击 "Save"。
4. 根据你使用的是顶级域名（如 `example.com`）还是子域名（如 `subdomain.example.com`）进行如下操作：
   - 顶级域名：在 DNS 服务商处添加 `A` 记录，指向 GitHub 的 IP 地址：
     - `185.199.108.153`
     - `185.199.109.153`
     - `185.199.110.153`
     - `185.199.111.153`
   - 子域名：在 DNS 服务商处添加 `CNAME` 记录，将子域名指向你的 GitHub Pages 默认域名。例如，`quartz.example.com` 指向 `<github-username>.github.io`。

![[dns records.png]]_上图为 Google Domains 配置了 `jzhao.xyz`（顶级域名）和 `quartz.jzhao.xyz`（子域名）的截图。_

详细操作可参考 [GitHub 官方文档](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain)。

> [!question] 为什么我的更改没有显示？
> 可能有多种原因，但最常见的是你忘记将更改推送到 GitHub。
>
> 请确保你已保存更改并通过 `npx quartz sync` 同步到 GitHub。这也会拉取你在其他设备上的更新，保证本地内容同步。

## Vercel

### 修复 URL

在部署到 Vercel 前，需要在项目根目录下添加 `vercel.json` 文件，内容如下，以确保 URL 不需要 `.html` 后缀：

```json title="vercel.json"
{
  "cleanUrls": true
}
```

### 部署到 Vercel

1. 登录 [Vercel 控制台](https://vercel.com/dashboard)，点击 "Add New..." > Project
2. 导入包含 Quartz 项目的 Git 仓库。
3. 给项目命名（仅小写字母和连字符）
4. 检查以下配置项：

| 配置项                                   | 值                  |
| ---------------------------------------- | ------------------- |
| Framework Preset                         | `Other`             |
| Root Directory                           | `./`                |
| Build and Output Settings > Build Command| `npx quartz build`  |

5. 点击 Deploy。部署完成后，你会获得两个 `*.vercel.app` 的访问地址。

### 自定义域名

> [!note]
> 如果该域名已有内容，以下步骤会覆盖原有内容。如需保留原内容，可使用 Next.js 重写或参考下节使用子域名。

1. 如有需要，更新 `quartz.config.js` 中的 `baseUrl`。
2. 前往 Vercel 的 [Domains - Dashboard](https://vercel.com/dashboard/domains) 页面。
3. 连接域名到 Vercel。
4. 点击 "Add" 添加自定义域名。
5. 选择你的 Quartz 仓库并点击 Continue。
6. 输入你要绑定的域名。
7. 按提示更新 DNS 记录，直到显示 "Valid Configuration"。

### 使用子域名

如 `docs.example.com`，子域名可用于将多个部署绑定到同一主域名。

1. 如有需要，更新 `quartz.config.js` 中的 `baseUrl`。
2. 确保你的域名已添加到 Vercel 的 [Domains - Dashboard](https://vercel.com/dashboard/domains)。
3. 在 [Vercel 控制台](https://vercel.com/dashboard) 选择你的 Quartz 项目。
4. 进入 Settings 标签页，点击侧边栏的 Domains。
5. 输入你的子域名并点击 Add。

## Netlify

1. 登录 [Netlify 控制台](https://app.netlify.com/)，点击 "Add new site"。
2. 选择你的 Git 提供商和包含 Quartz 项目的仓库。
3. 在 "Build command" 输入 `npx quartz build`。
4. 在 "Publish directory" 输入 `public`。
5. 点击 Deploy。部署完成后，你会获得一个 `*.netlify.app` 的访问地址。
6. 如需添加自定义域名，在左侧栏 "Domain management" 进行设置，方法与 Vercel 类似。

## GitLab Pages

在本地 Quartz 项目中新建 `.gitlab-ci.yml` 文件。

```yaml title=".gitlab-ci.yml"
stages:
  - build
  - deploy

image: node:20
cache: # 缓存依赖
  key: $CI_COMMIT_REF_SLUG
  paths:
    - .npm/

build:
  stage: build
  rules:
    - if: '$CI_COMMIT_REF_NAME == "v4"'
  before_script:
    - hash -r
    - npm ci --cache .npm --prefer-offline
  script:
    - npx quartz build
  artifacts:
    paths:
      - public
  tags:
    - gitlab-org-docker

pages:
  stage: deploy
  rules:
    - if: '$CI_COMMIT_REF_NAME == "v4"'
  script:
    - echo "Deploying to GitLab Pages..."
  artifacts:
    paths:
      - public
```

提交 `.gitlab-ci.yaml` 后，GitLab 会自动构建并部署为 GitLab Page。你可以在侧边栏的 `Deploy > Pages` 查看网址。

默认情况下，页面为私有，仅仓库成员可见，可在设置中打开公开访问：`Deploy` -> `Pages`。

## 自托管

将 `public` 目录复制到你的 Web 服务器，并配置服务器以提供这些文件。Quartz 生成的链接不带 `.html` 后缀，需要让服务器正确处理。

### Nginx 示例

Nginx 配置如下：

```nginx title="nginx.conf"
server {
    listen 80;
    server_name example.com;
    root /path/to/quartz/public;
    index index.html;
    error_page 404 /404.html;

    location / {
        try_files $uri $uri.html $uri/ =404;
    }
}
```

### Apache 示例

Apache 配置如下：

```apache title=".htaccess"
RewriteEngine On

ErrorDocument 404 /404.html

# 去除 .html 后缀的重写规则（带目录检查）
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_URI}.html -f
RewriteRule ^(.*)$ $1.html [L]

# 显式处理目录请求
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^(.*)/$ $1/index.html [L]
```

别忘了启用 brotli / gzip 压缩。

### Caddy 示例

Caddy 配置如下：

```caddy title="Caddyfile"
example.com {
    root * /path/to/quartz/public
    try_files {path} {path}.html {path}/ =404
    file_server
    encode gzip

    handle_errors {
        rewrite * /{err.status_code}.html
        file_server
    }
}
```

