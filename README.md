# hammal

Hammal 是运行于 cloudflare workers 上的 Docker 镜像加速工具，用于解决获取 Docker 官方镜像无法正常访问的问题。

文档： https://singee.atlassian.net/wiki/spaces/MAIN/pages/5079084/Cloudflare+Workers+Docker 

# 部署

利用 Cloudflare Workers 自建 Docker 镜像


Last updated: Jun 08, 2024

## fork

首先 fork 仓库 [GitHub - Jv0id/hammal: docker-registry proxy run in cloudflare workers](https://github.com/Jv0id/hammal) ，并克隆到本地

## install

使用 `pnpm install` 安装依赖

创建 `Workers` 项目
进入 `Cloudflare Dashboard` 创建一个新的 `Workers` 项目，给他一个命名（例如 hammal）

复制 `wrangler.toml.sample` 文件改名 `wrangler.toml` 并修改其 `name` 和 `account_id`

 > `account_id` 可以通过 `npx wrangler whoami` 获得，也可以从 CF Workers Dashboard 右侧获得

## 创建 cache 缓存 kv

在克隆好的项目目录下执行 `npx wrangler kv:namespace create hammal_cache` 来创建缓存 kv，记录下来输出的 id，填写到 wrangler.toml 文件中

Deploy
在克隆好的项目目录下执行 pnpm run deploy 来部署项目

进入你的 Workers 脚本的 dashboard，为它绑定一个自定义域名（必要，因为默认的 workers.dev 域名被墙了）

## 本地配置
使用你的自定义域名作为 `docker registry mirror` 即可


```
sudo tee /etc/docker/daemon.json <<EOF
{
  "registry-mirrors": [
    "https://hammal.example.com"
  ]
}
EOF
```
