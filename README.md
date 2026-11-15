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
release ships a `SHA256SUMS` file covering both binaries, plus a
`build-info.txt` file per binary (Caddy version, build date, sha256).
The first release (`caddy-2.11.9-20261114`) shipped per-asset
`.sha256` files instead of `SHA256SUMS`.

## Install

```sh
# Pick the variant for your system, then:
chmod +x caddy-linux-amd64
sudo mv caddy-linux-amd64 /usr/local/bin/caddy
```

## Verify

```sh
sha256sum -c SHA256SUMS
caddy version
```

Every line must print `OK`. To check one binary only:

```sh
grep caddy-linux-amd64 SHA256SUMS | sha256sum -c -
```

`caddy version` prints the Caddy version this binary was built from.

## Run

```sh
caddy run
caddy file-server --listen :8080 --root /var/www/html
caddy adapt --config Caddyfile
```

See the [Caddy documentation](https://caddyserver.com/docs/) for
configuration. Releases are built by GitHub Actions from source;
the build definitions live in a private repo.

## Install with mise

Releases are signed with [packslip](https://packslip.dev), so
[mise](https://mise.jdx.dev) installs them with no extra setup:

```sh
mise use packslip:github.com/popeico/caddy-releases@2.11.9
```

That writes this line to `mise.toml`:

```toml
"packslip:github.com/popeico/caddy-releases" = "2.11.9"
```

The signer fingerprint for this release is
`ps1_a7pg3ai5hc6n4k4keutpxkmcse` (verify with
`packslip pin packslip.sigstore.json` on the release bundle).

Both binaries in a release are the same static build; the musl-named
file is also available as the `musl` variant for hosts that want it
by name.
