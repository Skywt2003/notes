# tc 服务器折腾记录

[toc]

## Caddy

- 官方脚本安装。

## Pyenv & Python



## Docker

- 添加官方源的安装：

  ```
  sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
  curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo tee /etc/apt/trusted.gpg.d/caddy-stable.asc
  curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
  sudo apt update
  sudo apt install caddy
  ```

## Docker-compose

- 安装：

  ```
  curl -L "https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  chmod +x /usr/local/bin/docker-compose
  ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
  ```

## V2ray

- http 地址：`127.0.0.1:10800`
- socks5 地址：`127.0.0.1:10801`
- 配置文件：`/usr/local/etc/v2ray/config.json`
- 作为客户端，连接 SkyServer-bw。

## Netdata

- 地址：`0.0.0.0:19999`
- 配置文件：`/etc/netdata`
- 官方的一键安装脚本 `bash <(curl -Ss https://my-netdata.io/kickstart.sh)`。

## Overleaf

- 地址：`127.0.0.1:28080`
- ~~使用 Overleaf toolkit，[文档](https://github.com/overleaf/toolkit/blob/master/doc/quick-start-guide.md)。~~
- ~~直接用内建的 MongoDB 和 Redis 的 Docker。如果需要修改，可以修改 overleaf.rc 然后 bin/docker-compose down 以及 bin/up 重新构建镜像。参见[配置说明](https://github.com/overleaf/toolkit/blob/master/doc/overleaf-rc.md)。~~
- 使用了全套安装。
  - 进入 Docker：`docker exec -it sharelatex /bin/bash`
  - 换源：`tlmgr option repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet/`
    - 或者用中科大的 `tlmgr option repository https://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet/`
      - 主机在广州，中科大明显比清华快很多。
  - `tlmgr update --self`
  - `tlmgr install scheme-full`
- 代码高亮配置[参考](https://www.overleaf.com/learn/latex/Code_Highlighting_with_minted#Reference_guide)
  - 安装 Pygments：`apt-get update & apt-get insatll python-pygments`。
  - `/usr/local/texlive/2021/aaa.cnf` 中加入 `shell_escape = t`。

```sh
docker run -d \
  --name sharelatex \
  --network sharelatex \
  -p 28080:80 \
  -v /data/sharelatex:/var/lib/sharelatex \
  --env-file ~/overleaf/variables.env \
  sharelatex/sharelatex:latest
```



```
# 环境变量
MONGO_URL=mongodb://mongo/sharelatex
REDIS_HOST=redis
REDIS_PORT=6379

SHARELATEX_APP_NAME=SkyLatex

ENABLED_LINKED_FILE_TYPES=project_file,project_output_file

# Enables Thumbnail generation using ImageMagick
ENABLE_CONVERSIONS=true

# Disables email confirmation requirement
EMAIL_CONFIRMATION_DISABLED=true

# temporary fix for LuaLaTex compiles
# see https://github.com/overleaf/overleaf/issues/695
TEXMFVAR=/var/lib/sharelatex/tmp/texmf-var

SHARELATEX_SITE_URL=https://latex.skywt.cn/
SHARELATEX_NAV_TITLE=SkyLatex
# SHARELATEX_HEADER_IMAGE_URL=http://somewhere.com/mylogo.png
SHARELATEX_ADMIN_EMAIL=me@skywt.cn

SHARELATEX_LEFT_FOOTER=[{"text":"Powered by <a href=\"https://overleaf.com/\">Overleaf</a>"}, {"text":"Hosted on <a href=\"https://skywt.cn/\">SkyWT.cn</a>"}]
SHARELATEX_RIGHT_FOOTER=[{"text":"<a href=\"https://www.overleaf.com/learn\">Help</a>"}]

SHARELATEX_EMAIL_FROM_ADDRESS=notice@skywt.cn

# SHARELATEX_EMAIL_AWS_SES_ACCESS_KEY_ID=
# SHARELATEX_EMAIL_AWS_SES_SECRET_KEY=

SHARELATEX_EMAIL_SMTP_HOST=smtp.exmail.qq.com
SHARELATEX_EMAIL_SMTP_PORT=465
SHARELATEX_EMAIL_SMTP_SECURE=true
SHARELATEX_EMAIL_SMTP_USER=notice@skywt.cn
SHARELATEX_EMAIL_SMTP_PASS=EFg3svYe6Pff24Yr
# SHARELATEX_EMAIL_SMTP_NAME=
SHARELATEX_EMAIL_SMTP_LOGGER=false
SHARELATEX_EMAIL_SMTP_TLS_REJECT_UNAUTH=true
SHARELATEX_EMAIL_SMTP_IGNORE_TLS=false
SHARELATEX_CUSTOM_EMAIL_FOOTER="<a href='https://latex.skywt.cn/'>SkyLatex</a> hosted on <a href='https://skywt.cn/'>SkyWT.cn</a>."

TEX_LIVE_DOCKER_IMAGE=quay.io/sharelatex/texlive-full:2020.1
ALL_TEX_LIVE_DOCKER_IMAGES=quay.io/sharelatex/texlive-full:2020.1,quay.io/sharelatex/texlive-full:2019.1
```

## Redis

- `127.0.0.1:6379`

- ```sh
  docker run -d \
    --name redis \
    -v /data/redis:/data \
    -p 6379:6379 \
    redis:latest
  ```

- 加入 sharelatex, cloudreve 网络。

## MongoDB

- `127.0.0.1:27017`

- ```sh
  docker run -d \
    --name mongo \
    --network skylatex_default \
    -v /data/mongo/db:/data/db \
    -v /data/mongo/configdb:/data/configdb \
    -p 27017:27017 \
    mongo:latest
  ```

- 加入 sharelatex 网络。

## Cloudreve

```sh
docker run -d \
  --name cloudreve \
  --network cloudreve \
  -p 5212:5212 \
  --restart=unless-stopped \
  -v /data/cloudreve/uploads:/cloudreve/uploads \
  -v /data/cloudreve/config:/cloudreve/config \
  -v /data/cloudreve/db:/cloudreve/db \
  -v /data/cloudreve/avatar:/cloudreve/avatar \
  xavierniu/cloudreve:latest
```

参见[这里](https://hub.docker.com/r/xavierniu/cloudreve)。

## MariaDB

```sh
docker run -d \
  --name mariadb \
  -p 3306:3306 \
  --env MARIADB_ROOT_PASSWORD=13579\!Wthaaa \
  -v /data/mysql:/var/lib/mysql \
  mariadb:latest
```

## phpMyAdmin

```sh
docker run -d --name phpmyadmin -e PMA_HOST=mariadb -p 8080:80 phpmyadmin/phpmyadmin:latest
```

## CodeServer

```sh
docker run -d \
  --name code-server \
  -p 38080:8080 \
  -v "$HOME/.config:$HOME/.config" \
  -v "/data/coder/project:/home/coder/project" \
  -u "$(id -u):$(id -g)" \
  -e "DOCKER_USER=$USER" \
  codercom/code-server:latest
```

## Plausible #



## 其他

- hosts 修改：主机名从 `VM-20-13-debian` 改为 `SkyServer-tc`，在 hosts 中也做出了相应修改。

