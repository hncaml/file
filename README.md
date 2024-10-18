# sing-box 配置说明
{
  "log": {
    "disabled": false, 禁用日志，启动后不输出日志。
    "level": "info",  日志等级，可选值：trace debug info warn error fatal panic
    "output": "box.log",输出文件路径，启动后将不输出到控制台。
    "timestamp": true 添加时间到每行。
  },
  
  "experimental": {
    "clash_api": {
      "external_controller": "0.0.0.0:9090",
      "external_ui": "ui",
      "secret": "",
      "external_ui_download_url": "https://mirror.ghproxy.com/https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip",
      "external_ui_download_detour": "🎯 全球直连",
      "default_mode": "rule"
      }
    },
{
  "ntp": {   一般不使用
    "enabled": false, 启用 NTP 服务。
    "server": "time.apple.com",NTP 服务器地址。
    "server_port": 123,NTP 服务器端口。默认使用 123。
    "interval": "30m",时间同步间隔。默认使用 30 分钟。

    ... // 拨号字段
  }
}
  "dns": {
    "fakeip": {"enabled": true, "inet4_range": "198.18.0.0/15", "inet6_range": "fc00::/18"},  enabled 启用 FakeIP 服务。inet4_range 用于 FakeIP 的 IPv4 地址范围。inet6_range 用于 FakeIP 的 IPv6 地址范围。
    "servers": [  一组 DNS 服务器 
      {"tag": "proxyDns", "address": "tls://8.8.8.8", "address_resolver": "localDns", "address_strategy": "ipv4_only", "detour": "🚀 节点选择"},
      {"tag": "localDns", "address": "223.5.5.5", "detour": "🎯 全球直连"},
      {"tag": "fakeip", "address": "fakeip"},
      {"tag": "block", "address": "rcode://success"}
    ],
    "rules": [  一组 DNS 规则
      {"outbound": "any", "server": "localDns"},
      {"clash_mode": "direct", "server": "localDns"},
      {"clash_mode": "global", "server": "proxyDns"},
      {"query_type": ["A", "AAAA"], "rule_set": "geosite-cn", "server": "fakeip"},
      {"rule_set": "geosite-cn", "server": "localDns"},
      {"type": "logical", "mode": "and", "rules": [
        {"rule_set": "geosite-geolocation-!cn", "invert": true},
        {"rule_set": "geoip-cn"}
      ], "server": "proxyDns", "client_subnet": "114.114.114.114/24"},
      {"query_type": ["A", "AAAA"], "server": "fakeip"}
    ],
    "independent_cache": true, 使每个 DNS 服务器的缓存独立，以满足特殊目的。如果启用，将轻微降低性能。
    "strategy": "prefer_ipv4" 默认解析域名策略。可选值: prefer_ipv4 prefer_ipv6 ipv4_only ipv6_only。如果设置了 server.strategy，则不生效。
  },

  "inbounds": [
    {
      "tag": "tun",
      "type": "tun",
      "inet4_address": "172.19.0.0/30",
      "inet6_address": "fdfe:dcba:9876::0/126",
      "stack": "system",
      "auto_route": true,
      "strict_route": true,
      "sniff": true,
      "platform": {
        "http_proxy": {"enabled": true, "server": "0.0.0.0", "server_port": 7890}
      }
    },
    {
      "tag": "mixed",
      "type": "mixed",
      "listen": "0.0.0.0",
      "listen_port": 7890,
      "sniff": true
    }
  ],

  
  "outbounds": [
    { "tag": "🚀 节点选择", "type": "selector", "outbounds": ["🔯 香港自动", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🐸 手动切换", "♻️ 自动选择",  "🎯 全球直连"] },
    { "tag": "📹 YouTube", "type": "selector", "outbounds": ["🚀 节点选择", "♻️ 自动选择", "🔯 香港自动", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🐸 手动切换"] },
    { "tag": "🤖 OpenAI", "type": "selector", "outbounds": ["🚀 节点选择", "♻️ 自动选择", "🔯 香港自动", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🐸 手动切换"] },
    { "tag": "🍀 Google", "type": "selector", "outbounds": ["🚀 节点选择", "♻️ 自动选择", "🔯 香港自动", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🐸 手动切换"] },
    { "tag": "📲 Telegram", "type": "selector", "outbounds": ["🚀 节点选择", "♻️ 自动选择", "🔯 香港自动", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🐸 手动切换"] },
    { "tag": "🎵 TikTok", "type": "selector", "outbounds": ["🚀 节点选择", "♻️ 自动选择", "🔯 香港自动", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🐸 手动切换"] },
    { "tag": "🎥 Netflix", "type": "selector", "outbounds": ["🚀 节点选择", "♻️ 自动选择", "🔯 香港自动", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🐸 手动切换"] },
    { "tag": "🪟 Microsoft", "type": "selector", "outbounds": ["🚀 节点选择", "♻️ 自动选择", "🔯 香港自动", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🎯 全球直连"] },
    { "tag": "🍎 Apple", "type": "selector", "outbounds": ["🎯 全球直连", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点"] },
    { "tag": "🐸 手动切换", "type": "selector", "outbounds": ["{all}"]},
    { "tag": "🇭🇰 香港节点", "type": "selector", "outbounds": ["{all}"], "filter": [{ "action": "include", "keywords": ["🇭🇰|HK|hk|香港|港|HongKong"] }] },
    { "tag": "🇯🇵 日本节点", "type": "selector", "outbounds": ["{all}"], "filter": [{ "action": "include", "keywords": ["🇯🇵|JP|jp|日本|日|Japan"] }] },
    { "tag": "🇺🇲 美国节点", "type": "selector", "outbounds": ["{all}"], "filter": [{ "action": "include", "keywords": ["🇺🇸|US|us|美国|美|United States"] }] },
    { "tag": "🔯 香港自动", "type": "urltest", "outbounds": ["{all}"], "filter": [{ "action": "include", "keywords": ["🇭🇰|HK|hk|香港|港|HongKong"] }], "url": "http://www.gstatic.com/generate_204", "interval": "10m", "tolerance": 50 },
    { "tag": "♻️ 自动选择", "type": "urltest", "outbounds": ["{all}"], "filter": [{ "action": "exclude", "keywords": ["网站|地址|剩余|过期|时间|有效"] }], "url": "http://www.gstatic.com/generate_204", "interval": "10m", "tolerance": 50 },
    { "tag": "🐟 漏网之鱼", "type": "selector", "outbounds": ["🚀 节点选择", "🎯 全球直连", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🐸 手动切换"] },
    { "tag": "GLOBAL", "type": "selector", "outbounds": ["♻️ 自动选择", "🎯 全球直连", "🇭🇰 香港节点", "🇯🇵 日本节点", "🇺🇲 美国节点", "🐸 手动切换"] },
    { "tag": "🎯 全球直连", "type": "direct" },
    { "tag": "dns-out", "type": "dns" }
  ],
  
  "route": {
        "auto_detect_interface": true,
        "final": "🐟 漏网之鱼",
    "rules": [
      { "type": "logical", "mode": "or", "rules": [{ "port": 53 }, { "protocol": "dns" }], "outbound": "dns-out" },
      { "clash_mode": "direct", "outbound": "🎯 全球直连" },
      { "clash_mode": "global", "outbound": "GLOBAL" },
      { "domain": ["clash.razord.top", "yacd.metacubex.one", "yacd.haishan.me", "d.metacubex.one"], "outbound": "🎯 全球直连" },
      { "ip_is_private": true, "outbound": "🎯 全球直连" },
      { "rule_set": "geosite-openai", "outbound": "🤖 OpenAI" },
      { "rule_set": "geosite-youtube", "outbound": "📹 YouTube" },
      { "rule_set": ["geoip-google", "geosite-google"], "outbound": "🍀 Google" },
      { "rule_set": ["geoip-telegram", "geosite-telegram"], "outbound": "📲 Telegram" },
      { "rule_set": "geosite-tiktok", "outbound": "🎵 TikTok" },
      { "rule_set": ["geoip-netflix", "geosite-netflix"], "outbound": "🎥 Netflix" },
      { "rule_set": ["geoip-apple", "geosite-apple"], "outbound": "🍎 Apple" },
      { "rule_set": "geosite-microsoft", "outbound": "🪟 Microsoft" },      
      { "rule_set": "geosite-geolocation-!cn", "outbound": "🚀 节点选择" },
      { "rule_set": ["geoip-cn", "geosite-cn"], "outbound": "🎯 全球直连" }
    ],
    
    "rule_set": [
      { "tag": "geosite-openai", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/Toperlock/sing-box-geosite/main/rule/OpenAI.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-youtube", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/youtube.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geoip-google", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geoip/google.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-google", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/google.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-github", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/github.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geoip-telegram", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geoip/telegram.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-telegram", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/telegram.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-tiktok", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/tiktok.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geoip-netflix", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geoip/netflix.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-netflix", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/netflix.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geoip-apple", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo-lite/geoip/apple.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-apple", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/apple.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-microsoft", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/microsoft.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-geolocation-!cn", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/geolocation-!cn.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geoip-cn", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geoip/cn.srs", "download_detour": "🎯 全球直连" },
      { "tag": "geosite-cn", "type": "remote", "format": "binary", "url": "https://mirror.ghproxy.com/https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/sing/geo/geosite/cn.srs", "download_detour": "🎯 全球直连" }
    ]
  }
}
