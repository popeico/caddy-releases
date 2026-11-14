# caddy-releases

Personal Caddy builds, built from source in CI. Download and run — no
build tools needed.

## Downloads

Each release has two Linux x86_64 binaries:

| File | For |
| --- | --- |
| `caddy-linux-amd64` | Standard Linux (Debian, Ubuntu, etc.) |
| `caddy-linux-amd64-musl` | musl-based systems (Alpine, etc.) |

Both are statically linked, so they run without extra libraries. Each
binary ships with a `.sha256` file and a `build-info.txt` file (Caddy
version, build date, sha256).

## Install

```sh
# Pick the variant for your system, then:
chmod +x caddy-linux-amd64
sudo mv caddy-linux-amd64 /usr/local/bin/caddy
```

## Verify

```sh
sha256sum -c caddy-linux-amd64.sha256
caddy version
```

The checksum must print `OK`. `caddy version` prints the Caddy
version this binary was built from.

## Run

```sh
caddy run
caddy file-server --listen :8080 --root /var/www/html
caddy adapt --config Caddyfile
```

See the [Caddy documentation](https://caddyserver.com/docs/) for
configuration. Releases are built by GitHub Actions from source;
the build definitions live in a private repo.
