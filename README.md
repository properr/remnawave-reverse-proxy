<p align="center">
  <img src="./media/logo.jpg" alt="Remnawave Reverse Proxy" />
</p>

<p align="center">
  <img src="./media/ru.png" alt="Русский" /> <strong>Русская сборка</strong>
</p>

---

# Remnawave Reverse Proxy

Установщик панели управления и нод **Remnawave** — готовое решение для раздачи VPN на своём сервере. Всё настраивается автоматически: после установки панель уже работает, инбаунд создан, ключи сгенерированы.

Сборка полностью на русском языке.

---

## Что умеет

- **Панель + нода** — на одном сервере или на разных
- **Всё из коробки** — при установке автоматически создаётся рабочий конфиг: ключи генерируются сами, ваш домен подставляется сам, ничего настраивать вручную не нужно
- **nginx или Caddy** — на выбор
- **Сайт-заглушка** — сервер маскируется под обычный сайт: свои клиенты получают VPN, посторонние видят обычный сайт
- **SSL-сертификаты** — выпускаются и обновляются автоматически
- **Защита панели** — вход только по секретному адресу, без него панель не видна
- **Только русский язык** — без лишних вопросов при установке

---

## Требования

- **ОС:** Debian 11 / 12 / 13 или Ubuntu 22.04 / 24.04, свежая установка
- **Доступ:** root
- **Домены:** свой домен + поддомены (панель, страница подписок, домен для заглушки)

---

## Подготовка к установке

**⚡️ Обновление системы**

Перед установкой Remnawave рекомендуется обновить систему:

Hit:1 http://mirror.selectel.ru/ubuntu noble InRelease
Hit:2 http://deb.debian.org/debian trixie InRelease
Get:3 http://mirror.selectel.ru/ubuntu noble-updates InRelease [126 kB]
Get:4 http://mirror.selectel.ru/ubuntu noble-backports InRelease [126 kB]
Get:5 http://security.ubuntu.com/ubuntu noble-security InRelease [126 kB]
Get:6 http://deb.debian.org/debian trixie-updates InRelease [47.3 kB]
Hit:7 https://mirror.selectel.ru/3rd-party/cloud-init-deb/noble noble InRelease
Get:8 http://mirror.selectel.ru/ubuntu noble-updates/main amd64 Packages [1,184 kB]
Hit:9 https://deb.nodesource.com/node_24.x nodistro InRelease
Get:10 http://mirror.selectel.ru/ubuntu noble-updates/universe amd64 Packages [1,682 kB]
Get:11 http://security.ubuntu.com/ubuntu noble-security/main amd64 Packages [927 kB]
Get:12 http://mirror.selectel.ru/ubuntu noble-updates/restricted amd64 Packages [1,414 kB]
Err:7 https://mirror.selectel.ru/3rd-party/cloud-init-deb/noble noble InRelease
  Sub-process /usr/bin/sqv returned an error code (1), error message is: Signing key on 28EF171FF9D298E6C15B4801717DBAC67A378451 is not bound:            No binding signature at time 2026-04-20T20:16:50Z   because: Policy rejected non-revocation signature (PositiveCertification) requiring second pre-image resistance   because: SHA1 is not considered secure since 2026-02-01T00:00:00Z
Hit:13 https://nginx.org/packages/mainline/ubuntu noble InRelease
Hit:14 https://ppa.launchpadcontent.net/ondrej/php/ubuntu noble InRelease
Get:15 https://apt.hestiacp.com noble InRelease [22.0 kB]
Get:16 http://security.ubuntu.com/ubuntu noble-security/restricted amd64 Packages [1,320 kB]
Err:15 https://apt.hestiacp.com noble InRelease
  Sub-process /usr/bin/sqv returned an error code (1), error message is: Error: Failed to parse keyring "/usr/share/keyrings/hestia-keyring.gpg"  Caused by:     0: Reading "/usr/share/keyrings/hestia-keyring.gpg": EOF     1: EOF
Hit:17 https://dlm.mariadb.com/repo/mariadb-server/11.4/repo/ubuntu noble InRelease
Fetched 6,974 kB in 1s (4,949 kB/s)
Reading package lists...
Building dependency tree...
Reading state information...
32 packages can be upgraded. Run 'apt list --upgradable' to see them.
Reading package lists...
Building dependency tree...
Reading state information...
Calculating upgrade...
The following packages were automatically installed and are no longer required:
  android-libart        android-liblog           android-libziparchive
  android-libbacktrace  android-libnativebridge
  android-libbase       android-libnativeloader
Use 'apt autoremove' to remove them.

Get more security updates through Ubuntu Pro with 'esm-apps' enabled:
  libmagickcore-6.q16-7t64 libzvbi-common restic imagemagick python3-pip-whl
  libcjson1 libavdevice60 ffmpeg libpostproc57 python3-wheel libmbedcrypto7t64
  libavcodec60 libzvbi0t64 libavutil58 imagemagick-6.q16 libswscale7
  python3-pip libswresample4 imagemagick-6-common libavformat60 libavfilter9
  libmagickwand-6.q16-7t64
Learn more about Ubuntu Pro at https://ubuntu.com/pro
Upgrading:
  iproute2       php8.3-cli     php8.3-mbstring  php8.3-zip
  libxmlb2       php8.3-common  php8.3-mysql     php8.4-cli
  nginx          php8.3-curl    php8.3-opcache   php8.4-common
  nodejs         php8.3-fpm     php8.3-pspell    php8.4-curl
  php8.3         php8.3-gd      php8.3-readline  php8.4-opcache
  php8.3-apcu    php8.3-imap    php8.3-soap      php8.4-readline
  php8.3-bcmath  php8.3-intl    php8.3-sqlite3
  php8.3-bz2     php8.3-ldap    php8.3-xml

Not upgrading yet due to phasing:
  qemu-guest-agent  qemu-utils

DOWNGRADING:
  bsdmainutils          fonts-dejavu-core  libjs-underscore  python3-serial
  ca-certificates-java  fonts-dejavu-mono  ncal              xfonts-cyrillic
  connect-proxy         libjs-jquery       python3-jmespath  xml-core

Summary:
  Upgrading: 30, Installing: 0, Downgrading: 12, Removing: 0, Not Upgrading: 2

**✅ Проверка DNS**

Перед запуском скрипта установки убедитесь, что DNS-записи уже указывают на IP-адрес вашего VPS. Это поможет избежать ошибок при выпуске SSL-сертификатов и настройке панели.

Проверьте записи:



Все три команды должны вернуть **IP-адрес вашего VPS**. Если вместо IP-адреса ничего не отображается или указан другой адрес — дождитесь обновления DNS-записей и повторите проверку.

---

## Быстрый старт

```bash
bash <(curl -Ls https://raw.githubusercontent.com/properr/remnawave-reverse-proxy/refs/heads/main/install_remnawave.sh)
```

Скрипт сам всё сделает: установит, настроит, сгенерирует ключи и покажет логин с паролем от панели.

<p align="center">
  <img src="./media/remnawave-reverse-proxy.png" alt="Интерфейс установки" />
</p>

---

## Режимы установки

| Режим | Когда нужен |
|-------|-------------|
| **Панель + нода на одном сервере** | Компактная установка, умеренный трафик |
| **Только панель** | Центр управления без VPN-ноды |
| **Только нода** | Отдельный сервер с VPN, подключается к панели по ключу |

---

## Настройка DNS

**Один сервер (панель + нода):**

| Тип | Имя | Значение | Прокси |
|-----|-----|----------|--------|
| A | example.com | IP сервера | DNS only |
| CNAME | panel.example.com | example.com | DNS only |
| CNAME | sub.example.com | example.com | DNS only |
| CNAME | node.example.com | example.com | DNS only |

**Раздельная установка:** `node.example.com` → IP сервера ноды, остальное → IP панели.

---

## Обновление

Установщик умеет обновлять сам себя из этого репозитория — пункт меню «Обновление».

---

## Структура репозитория

```
install_remnawave.sh   — основной скрипт (меню установки)
src/nginx/             — модули установки для nginx
src/caddy/             — модули установки для Caddy
src/modules/           — управление панелью, нодами, Warp, IPv6
src/api/               — работа с API панели
src/lang/              — языковой файл (русский)
```

---

> [!CAUTION]
> Инструмент предназначен для администрирования собственных серверов. Используйте на свой страх и риск.
