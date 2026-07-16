mixed-port: 7890
allow-lan: true
mode: Rule
log-level: info

dns:
  enable: true
  ipv6: false
  enhanced-mode: fake-ip
  nameserver:
    - 223.5.5.5
    - 1.1.1.1


proxies:

  - name: 🇭🇰 香港01-Mieru
    type: mieru
    server: hk.cdn.games.cdnazuremicrosoft.com
    port: 443
    username: db9e8875-fb87-4a29-a131-1adc7bd9ebf5
    password: db9e8875-fb87-4a29-a131-1adc7bd9ebf5
    udp: true
    transport: tcp
    profile: 直连-香港01
    traffic-pattern: CgYImAEQmgQ=

  - name: 🇭🇰 香港02-Mieru
    type: mieru
    server: boxhk.cdnazuremicrosoft.com
    port: 65535
    username: db9e8875-fb87-4a29-a131-1adc7bd9ebf5
    password: db9e8875-fb87-4a29-a131-1adc7bd9ebf5
    udp: true
    transport: tcp
    profile: 直连-香港02
    traffic-pattern: CgYIkQEQ9QM=


proxy-groups:

  - name: 🚀 Mieru自动选择
    type: url-test
    proxies:
      - 🇭🇰 香港01-Mieru
      - 🇭🇰 香港02-Mieru
    url: http://www.gstatic.com/generate_204
    interval: 300
    tolerance: 50


  - name: 🌐 手动选择
    type: select
    proxies:
      - 🚀 Mieru自动选择
      - 🇭🇰 香港01-Mieru
      - 🇭🇰 香港02-Mieru


  - name: Proxy
    type: select
    proxies:
      - 🌐 手动选择
      - 🚀 Mieru自动选择


rules:

  - DOMAIN-SUFFIX,google.com,Proxy
  - DOMAIN-SUFFIX,youtube.com,Proxy
  - DOMAIN-SUFFIX,x.com,Proxy
  - DOMAIN-SUFFIX,telegram.org,Proxy

  - GEOIP,CN,DIRECT

  - MATCH,Proxy
