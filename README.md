<br>English | [简体中文](README_zh.md)

[![CI](https://github.com/gngpp/opengpt/actions/workflows/CI.yml/badge.svg)](https://github.com/gngpp/opengpt/actions/workflows/CI.yml)
[![CI](https://github.com/gngpp/opengpt/actions/workflows/Release.yml/badge.svg)](https://github.com/gngpp/opengpt/actions/workflows/Release.yml)
	<a target="_blank" href="https://github.com/gngpp/vdns/blob/main/LICENSE">
		<img src="https://img.shields.io/badge/license-MIT-blue.svg"/>
	</a>
  <a href="https://github.com/gngpp/opengpt/releases">
    <img src="https://img.shields.io/github/release/gngpp/opengpt.svg?style=flat">
  </a><a href="https://github.com/gngpp/opengpt/releases">
    <img src="https://img.shields.io/github/downloads/gngpp/opengpt/total?style=flat">
  </a>
  [![](https://img.shields.io/docker/image-size/gngpp/opengpt)](https://registry.hub.docker.com/r/gngpp/opengpt)
  [![Docker Image](https://img.shields.io/docker/pulls/gngpp/opengpt.svg)](https://hub.docker.com/r/gngpp/opengpt/)

# opengpt

A reverse engineered unofficial `ChatGPT` proxy (bypass Cloudflare 403 Access Denied)

###  Features

- API key acquisition
- Email/password account authentication (Google/Microsoft third-party login is temporarily not supported)
- `Unofficial`/`Official`/`ChatGPT-to-API` Http API proxy (for third-party client access)
- Support proxy pool
- Comes with ChatGPT WebUI
- Minimal memory usage

> Limitations: This cannot bypass OpenAI's outright IP ban

### ArkoseLabs

Sending a `GPT4` conversation requires `Arkose Token` to be sent as a parameter, and there are only three supported solutions for the time being

1) The endpoint obtained by `Arkose Token`, no matter what method you use, use `--arkose-token-endpoint` to specify the endpoint to obtain the token. The supported `JSON` format is generally in accordance with the format of the community: `{"token": "xxxxxx"}`

2) The `ChatGPT` official website sends a `GPT4` session message, and the browser `F12` downloads `https://tcr9i.chat.openai.com/fc/gt2/public_key/35536E1E-65B4-4D96-9D97-6ADB7EFF8147` interface The HAR log record file, use the startup parameter `--arkose-har-path` to specify the HAR file path to use. Support uploading and updating HAR `request path: /har/upload`, the HAR file must exist, at this time, uploading and updating the HAR file is supported, optional upload authentication parameter `--arkose-har-upload-key`

3) Use the [YesCaptcha](https://yescaptcha.atlassian.net/wiki/spaces/YESCAPTCHA/overview?homepageId=33020) platform for AI coding, start the parameter `--arkose-yescaptcha-key` fill in the Key to enable, Affordable price, `10RMB` is calculated by point submission, `10000/3 ~= 3333 submissions`,

All three schemes are used, and the priority is: `HAR` > `Arkose Token endpoint` > `YesCaptcha`

### Platform Support

- Linux
  - `x86_64-unknown-linux-musl`
  - `aarch64-unknown-linux-musl`
  - `armv7-unknown-linux-musleabi`
  - `armv7-unknown-linux-musleabihf`
  - `arm-unknown-linux-musleabi`
  - `arm-unknown-linux-musleabihf`
  - `armv5te-unknown-linux-musleabi`
- Windows
  - `x86_64-pc-windows-msvc`
- MacOS
  - `x86_64-apple-darwin`
  - `aarch64-apple-darwin`

### Command Line(dev)

### Http Server

> Public API, `*` means any `URL` suffix
>
> - backend-api, <https://host:port/backend-api/*>
> - public-api, <https://host:port/public-api/*>
> - platform-api, <https://host:port/v1/*>
> - dashboard-api, <https://host:port/dashboard/*>
> - chatgpt-to-api, <https://host:port/to/v1/chat/completions>
>
> Detailed API documentation
>
> - Platform API [doc](https://platform.openai.com/docs/api-reference)
> - Backend API [doc](doc/rest.http)

- Authentic ChatGPT WebUI
- Expose `unofficial`/`official API` proxies
- The `API` prefix is consistent with the official
- `ChatGPT` To `API`
- Accessible to third-party clients
- Access to IP proxy pool to improve concurrency
- API documentation

- Parameter Description
  - `--level`, environment variable `LOG`, log level: default info
  - `--host`, environment variable `HOST`, service listening address: default 0.0.0.0,
  - `--port`, environment variable `PORT`, listening port: default 7999
  - `--tls-cert`, environment variable `TLS_CERT`', TLS certificate public key. Supported format: EC/PKCS8/RSA
  - `--tls-key`, environment variable `TLS_KEY`, TLS certificate private key
  - `--proxies`, Proxy, supports proxy pool, multiple proxies are separated by `,`, format: protocol://user:pass@ip:port, if the IP is Banned, it is recommended to turn off the use of direct IP when using the proxy pool, `--disable-direct`turn off direct connection
  - `--workers`, worker threads: default 1

...

### Install

- #### Ubuntu(Other Linux)

Making [Releases](https://github.com/gngpp/opengpt/releases/latest) has a precompiled deb package, binaries, in Ubuntu, for example:

```shell
wget https://github.com/gngpp/opengpt/releases/download/v0.4.9/opengpt-0.4.9-x86_64-unknown-linux-musl.deb
dpkg -i opengpt-0.4.9-x86_64-unknown-linux-musl.deb
opengpt serve run
```

- #### OpenWrt

There are pre-compiled ipk files in GitHub [Releases](https://github.com/gngpp/opengpt/releases/latest), which currently provide versions of aarch64/x86_64 and other architectures. After downloading, use opkg to install, and use nanopi r4s as example:

```shell
wget https://github.com/gngpp/opengpt/releases/download/v0.4.9/opengpt_0.4.9_aarch64_generic.ipk
wget https://github.com/gngpp/opengpt/releases/download/v0.4.9/luci-app-opengpt_1.0.6-1_all.ipk
wget https://github.com/gngpp/opengpt/releases/download/v0.4.9/luci-i18n-opengpt-zh-cn_1.0.6-1_all.ipk

opkg install opengpt_0.4.9_aarch64_generic.ipk
opkg install luci-app-opengpt_1.0.6-1_all.ipk
opkg install luci-i18n-opengpt-zh-cn_1.0.6-1_all.ipk
```

- #### Docker

```shell
docker run --rm -it -p 7999:7999 --name=opengpt \
  -e WORKERS=1 \
  -e LOG=info \
  gngpp/opengpt:latest serve run
```

- Docker Compose

```yaml
version: '3'

services:
  opengpt:
    image: ghcr.io/gngpp/opengpt:latest
    container_name: opengpt
    restart: unless-stopped
    environment:
      - TZ=Asia/Shanghai
      - PROXIES=socks5://warp:10000
      # - CONFIG=/serve.toml
      # - PORT=8080
      # - HOST=0.0.0.0
      # - TLS_CERT=
      # - TLS_KEY=
    # volumes:
      # - ${PWD}/ssl:/etc
      # - ${PWD}/serve.toml:/serve.toml
    command: serve run
    ports:
      - "8080:7999"
    depends_on:
      - warp

  warp:
    container_name: warp
    image: ghcr.io/gngpp/warp:latest
    restart: unless-stopped

  watchtower:
    container_name: watchtower
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    command: --interval 3600 --cleanup
    restart: unless-stopped

```

### Command Manual

```shell
$ opengpt serve --help
Start the http server

Usage: opengpt serve run [OPTIONS]

Options:
  -C, --config <CONFIG>
          Configuration file path (toml format file) [env: CONFIG=]
  -H, --host <HOST>
          Server Listen host [env: HOST=] [default: 0.0.0.0]
  -L, --level <LEVEL>
          Log level (info/debug/warn/trace/error) [env: LOG=] [default: info]
  -P, --port <PORT>
          Server Listen port [env: PORT=] [default: 7999]
  -W, --workers <WORKERS>
          Server worker-pool size (Recommended number of CPU cores) [default: 1]
      --concurrent-limit <CONCURRENT_LIMIT>
          Enforces a limit on the concurrent number of requests the underlying [default: 65535]
  -x, --proxies <PROXIES>
          Server proxies pool, Example: protocol://user:pass@ip:port [env: PROXIES=]
      --disable-direct
          Disable direct connection [env: DISABLE_DIRECT=]
      --timeout <TIMEOUT>
          Client timeout (seconds) [default: 600]
      --connect-timeout <CONNECT_TIMEOUT>
          Client connect timeout (seconds) [default: 60]
      --tcp-keepalive <TCP_KEEPALIVE>
          TCP keepalive (seconds) [default: 60]
      --tls-cert <TLS_CERT>
          TLS certificate file path [env: TLS_CERT=]
      --tls-key <TLS_KEY>
          TLS private key file path (EC/PKCS8/RSA) [env: TLS_KEY=]
      --puid <PUID>
          PUID cookie value of Plus account [env: PUID=]
      --puid-user <PUID_USER>
          Obtain the PUID of the Plus account user, Example: `user:pass` or `user:pass:mfa`
      --api-prefix <API_PREFIX>
          Web UI api prefix [env: UI_API_PREFIX=]
      --arkose-endpoint <ARKOSE_ENDPOINT>
          Arkose endpoint, Example: https://client-api.arkoselabs.com
  -A, --arkose-token-endpoint <ARKOSE_TOKEN_ENDPOINT>
          Get arkose token endpoint
  -a, --arkose-har-path <ARKOSE_HAR_PATH>
          About the browser HAR file path requested by ArkoseLabs
  -K, --arkose-har-upload-key <ARKOSE_HAR_UPLOAD_KEY>
          HAR file upload authenticate key
  -Y, --arkose-yescaptcha-key <ARKOSE_YESCAPTCHA_KEY>
          About the YesCaptcha platform client key solved by ArkoseLabs
  -S, --sign-secret-key <SIGN_SECRET_KEY>
          Enable url signature (signature secret key)
  -T, --tb-enable
          Enable token bucket flow limitation
      --tb-store-strategy <TB_STORE_STRATEGY>
          Token bucket store strategy (mem/redis) [default: mem]
      --tb-redis-url <TB_REDIS_URL>
          Token bucket redis url, Example: redis://user:pass@ip:port [default: redis://127.0.0.1:6379]
      --tb-capacity <TB_CAPACITY>
          Token bucket capacity [default: 60]
      --tb-fill-rate <TB_FILL_RATE>
          Token bucket fill rate [default: 1]
      --tb-expired <TB_EXPIRED>
          Token bucket expired (seconds) [default: 86400]
      --cf-site-key <CF_SITE_KEY>
          Cloudflare turnstile captcha site key [env: CF_SECRET_KEY=]
      --cf-secret-key <CF_SECRET_KEY>
          Cloudflare turnstile captcha secret key [env: CF_SITE_KEY=]
  -D, --disable-webui
          Disable WebUI [env: DISABLE_WEBUI=]
  -h, --help
          Print help
```

### Compile

- Linux compile, Ubuntu machine for example:

```shell

sudo apt update -y && sudo apt install rename

# Native compilation
git clone https://github.com/gngpp/opengpt.git && cd opengpt
./build.sh

# Cross-platform compilation, relying on docker (if you can solve cross-platform compilation dependencies on your own)
# Default using docker build linux/windows platform 
./build_cross.sh
# The MacOS platform is built on MacOS by default
os=macos ./build_cross.sh 

# Compile a single platform binary, take aarch64-unknown-linux-musl as an example: 
docker run --rm -it --user=$UID:$(id -g $USER) \
  -v $(pwd):/home/rust/src \
  -v $HOME/.cargo/registry:/root/.cargo/registry \
  -v $HOME/.cargo/git:/root/.cargo/git \
  ghcr.io/gngpp/opengpt-builder:x86_64-unknown-linux-musl \
  cargo build --release
```

- OpenWrt compile

```shell
cd package
svn co https://github.com/gngpp/opengpt/trunk/openwrt
cd -
make menuconfig # choose LUCI->Applications->luci-app-opengpt  
make V=s
```

### Preview

![img0](./doc/img/img0.png)
![img1](./doc/img/img1.png)
![img2](./doc/img/img2.png)
