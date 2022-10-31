# v2rayS-install

# 只推荐 Docker compose 安装
0. 安装docker-compose: 
```
curl -fsSL https://get.docker.com | bash -s docker
curl -L "https://github.com/docker/compose/releases/download/v2.12.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```
1. `git clone https://github.com/thank243/v2rayS-install.git`
2. `cd v2rayS-install`
3. 编辑config。
配置文件基本格式如下，Nodes下可以同时添加多个面板，多个节点配置信息，只需添加相同格式的Nodes item即可。
4. 启动docker：`docker-compose up -d`

```yaml
Log:
  Level: error # Log level: none, error, warning, info, debug
  AccessPath: # /etc/v2rayS/access.Log
  ErrorPath: # /etc/v2rayS/error.log
DnsConfigPath: /etc/v2rayS/dns.json # /etc/v2rayS/dns.json # Path to dns config, check https://guide.v2fly.org/basics/dns.html for help
RouteConfigPath: /etc/v2rayS/route.json # /etc/v2rayS/route.json # Path to route config, check https://guide.v2fly.org/basics/routing/basics_routing.html for help
InboundConfigPath: /etc/v2rayS/custom_inbound.json # /etc/v2rayS/custom_inbound.json # Path to custom inbound config, check https://www.v2fly.org/v5/config/inbound.html for help
OutboundConfigPath: /etc/v2rayS/custom_outbound.json # /etc/v2rayS/custom_outbound.json # Path to custom outbound config, check https://www.v2fly.org/v5/config/outbound.html for help
ConnectionConfig:
  Handshake: 4 # Handshake time limit, Second
  ConnIdle: 30 # Connection idle time limit, Second
  UplinkOnly: 2 # Time limit when the connection downstream is closed, Second
  DownlinkOnly: 4 # Time limit when the connection is closed after the uplink is closed, Second
  BufferSize: 64 # The internal cache size of each connection, kB 
Nodes:
  - PanelType: "V2board" # Panel type: V2board. PMpanel, Proxypanel, V2RaySocks
    ApiConfig:
      ApiHost: "https://example.com"
      ApiKey: "YOUR TOKEN"
      NodeID: 1
      NodeType: V2ray # Node type: V2ray, Trojan
      Timeout: 30 # Timeout for the api request
      SpeedLimit: 0 # Mbps, Local settings will replace remote settings, 0 means disable
      DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
      RuleListPath: /etc/v2rayS/rulelist # /etc/v2rayS/rulelist Path to local rulelist file
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      SendIP: 0.0.0.0 # IP address you want to send package
      UpdatePeriodic: 60 # Time to update the nodeInfo, how many sec.
      EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
      DNSType: AsIs # AsIs, UseIP, UseIPv4, UseIPv6, DNS strategy
      EnableProxyProtocol: false # Only works for WebSocket and TCP
      AutoSpeedLimitConfig:
        Limit: 0 # Warned speed. Set to 0 to disable AutoSpeedLimit (mbps)
        WarnTimes: 0 # After (WarnTimes) consecutive warnings, the user will be limited. Set to 0 to punish overSpeed user immediately.
        LimitSpeed: 0 # The speedlimit of a limited user (unit: mbps)
        LimitDuration: 0 # How many minutes will the limiting last (unit: minute)
      FallBackConfigs: # Support multiple fallbacks
        - Alpn: # Alpn, Empty for any
          Path: # HTTP PATH, Empty for any
          Dest: 80 # Required, Destination of fallback, check https://xtls.github.io/config/features/fallback.html for details.
          ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for disable
      CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will force disable the tls config.
        CertDomain: node1.test.com # Domain to cert
        CertFile: /etc/v2rayS/cert/node1.test.com.cert # Provided if CertMode: file
        KeyFile: /etc/v2rayS/cert/node1.test.com.key
        Provider: cloudflare # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          CLOUDFLARE_EMAIL: AAAA
          CLOUDFLARE_API_KEY: BBBB
```
## Docker compose升级
在docker-compose.yml目录下执行：
```
docker-compose pull
docker-compose up -d
```
