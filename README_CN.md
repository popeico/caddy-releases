[EN](README.md) | 中文

# caddy-releases

个人用的 Caddy 构建，在 CI 里从源码编译。下载即用，不用装构建工具。

## 下载

每个 release 有两个 Linux x86_64 二进制：

| 文件 | 适用 |
| --- | --- |
| `caddy-linux-amd64` | 标准 Linux（Debian、Ubuntu 等） |
| `caddy-linux-amd64-musl` | musl 系统（Alpine 等） |

两个都是静态链接，不需要额外运行库。每个 release 带一个覆盖两个二进制的 `SHA256SUMS` 文件，以及每个二进制一份 `build-info.txt`（Caddy 版本、构建日期、sha256）。第一个 release（`caddy-2.11.9-20261114`）用的是每文件一份的 `.sha256`，而不是 `SHA256SUMS`。

## 安装

```sh
# 按系统选一个版本，然后：
chmod +x caddy-linux-amd64
sudo mv caddy-linux-amd64 /usr/local/bin/caddy
```

## 校验

```sh
sha256sum -c SHA256SUMS
caddy version
```

每行都必须打印 `OK`。只校验一个二进制：

```sh
grep caddy-linux-amd64 SHA256SUMS | sha256sum -c -
```

`caddy version` 打印构建所用的 Caddy 版本。

## 运行

```sh
caddy run
caddy file-server --listen :8080 --root /var/www/html
caddy adapt --config Caddyfile
```

配置见 [Caddy 文档](https://caddyserver.com/docs/)。Release 由 GitHub Actions 从源码构建，构建定义在私有仓库。

## 用 mise 安装

Release 经 [packslip](https://packslip.dev) 签名，[mise](https://mise.jdx.dev) 无需额外配置即可安装：

```sh
mise use packslip:github.com/popeico/caddy-releases@2.11.9
```

这行会写入 `mise.toml`：

```toml
"packslip:github.com/popeico/caddy-releases" = "2.11.9"
```

该 release 的签名指纹是 `ps1_a7pg3ai5hc6n4k4keutpxkmcse`（用 release bundle 里的 `packslip pin packslip.sigstore.json` 验证）。

一个 release 里的两个二进制是同一份静态构建；musl 命名的文件也可作为 `musl` variant，供想按名字取用的主机使用。

## 两个渠道

每个 release 都由一个 Caddy 版本构建，但通过两个渠道之一发布：

- **Stable（稳定版）**——版本号就是 Caddy 版本号（`2.11.9`）。这是默认渠道：`mise latest` 和直接 `mise install` 都解析到最新的稳定版。每个 Caddy 版本只有一个稳定版条目；稳定版只会被显式裁剪（trim）取代。
- **Experimental（实验版）**——版本号保留构建日期（`2.11.9-20261122`，同一天多次构建再加 `.1`、`.2`）。实验版是 GitHub prerelease：不会出现在 `mise latest` 里，可按精确版本号安装，或开启 prerelease 后安装：

```sh
mise install packslip:github.com/popeico/caddy-releases@2.11.9-20261122
```

为了可复现，建议在 `mise.toml`（或 `mise.lock`）里钉精确版本号，而不是跟随 `latest`。每个 release 的 Caddy 版本、构建 tag 和渠道都写在它的 tag、`build-info*.txt` 文件和 release notes 里。
