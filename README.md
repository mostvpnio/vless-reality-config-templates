# VLESS + Reality client config templates

Готовые **шаблоны клиентских конфигов** для протокола **VLESS + Reality** —
для Hiddify, Happ, V2RayN, NekoBox / sing-box и других совместимых клиентов
(Xray / sing-box). Reality маскирует трафик под обычный HTTPS к крупному сайту,
поэтому связка остаётся одной из немногих, что стабильно устанавливается из
российских сетей в 2026 году.

> В файлах **нет реальных ключей и секретов** — только плейсхолдеры
> (`YOUR_SERVER`, `YOUR_UUID`, `YOUR_PUBLIC_KEY`, `YOUR_SHORT_ID`, `YOUR_SNI`).
> Подставьте параметры своего сервера и импортируйте в клиент.

## Плейсхолдеры

| Плейсхолдер | Что означает |
|---|---|
| `YOUR_SERVER` | IP или домен вашего Reality-сервера |
| `YOUR_PORT` | порт (для Reality обычно `443`) |
| `YOUR_UUID` | UUID пользователя |
| `YOUR_PUBLIC_KEY` | публичный ключ Reality (`pbk`) |
| `YOUR_SHORT_ID` | короткий ID Reality (`sid`) |
| `YOUR_SNI` | SNI для маскировки (напр. `m.vk.com`, `www.icloud.com`) |

## Универсальная ссылка (Happ, NekoBox, Hiddify — импорт из буфера)

```
vless://YOUR_UUID@YOUR_SERVER:443?encryption=none&type=tcp&flow=xtls-rprx-vision&security=reality&sni=YOUR_SNI&fp=chrome&pbk=YOUR_PUBLIC_KEY&sid=YOUR_SHORT_ID&packetEncoding=xudp#MyReality
```

Импорт: в клиенте `+` → «Добавить из буфера обмена» → вставить строку.

## Hiddify / V2RayN — JSON (Xray)

```json
{
  "log": { "loglevel": "warning" },
  "inbounds": [
    { "tag": "socks", "port": 10808, "listen": "127.0.0.1", "protocol": "socks", "settings": { "udp": true } }
  ],
  "outbounds": [
    {
      "tag": "proxy",
      "protocol": "vless",
      "settings": {
        "vnext": [
          {
            "address": "YOUR_SERVER",
            "port": 443,
            "users": [
              { "id": "YOUR_UUID", "encryption": "none", "flow": "xtls-rprx-vision" }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "fingerprint": "chrome",
          "serverName": "YOUR_SNI",
          "publicKey": "YOUR_PUBLIC_KEY",
          "shortId": "YOUR_SHORT_ID",
          "spiderX": ""
        }
      }
    }
  ]
}
```

## NekoBox / sing-box — JSON

```json
{
  "outbounds": [
    {
      "type": "vless",
      "tag": "proxy",
      "server": "YOUR_SERVER",
      "server_port": 443,
      "uuid": "YOUR_UUID",
      "flow": "xtls-rprx-vision",
      "packet_encoding": "xudp",
      "tls": {
        "enabled": true,
        "server_name": "YOUR_SNI",
        "utls": { "enabled": true, "fingerprint": "chrome" },
        "reality": { "enabled": true, "public_key": "YOUR_PUBLIC_KEY", "short_id": "YOUR_SHORT_ID" }
      }
    }
  ]
}
```

## Совместимые клиенты

| Платформа | Клиент | Где скачать |
|---|---|---|
| iOS | Happ / Hiddify Next | App Store |
| Android | Hiddify Next / NekoBox | Google Play · F-Droid · GitHub releases |
| Windows | V2RayN / Hiddify Next | GitHub releases |
| macOS / Linux | Hiddify Next | GitHub releases |

## Лицензия

MIT. Используйте свободно, форкайте, дорабатывайте.

---

<sub>Шаблоны поддерживаются командой [MostVPN](https://mostvpn.io). Нашли неточность — откройте issue.</sub>
