# Clash / Mihomo 配置说明

## 文件用途

- `template.yaml`：公开仓库模板，不填写真实订阅地址。
- `config.local.yaml`：本地使用文件，可从 `template.yaml` 复制后填写真实订阅地址；已被 `.gitignore` 忽略，不要上传。

## 本地使用

复制模板：

```powershell
Copy-Item .\clash\template.yaml .\clash\config.local.yaml
```

然后在 `config.local.yaml` 里填写：

```yaml
proxy-providers:
  主用机场:
    url: "你的机场订阅地址"
  备用机场:
    url: "你的第二个机场订阅地址"
  家宽机场:
    url: "你的住宅/家宽机场订阅地址"
```

如果只有一个或两个订阅，可以删除不用的 provider，并同步从 `⭐ 香港自动`、`⭐ 新加坡自动`、`⭐ 日本自动`、`⭐ 美国自动` 的 `use` 列表里删掉对应名称。

大部分地区分组使用 `include-all: true`，会自动读取所有订阅里的节点；节点能否进入香港/美国/家宽等分组，主要取决于节点名称是否匹配对应 `filter`。

## DNS 防泄漏

`template.yaml` 默认使用严格防泄漏模式：

- 关闭 IPv6。
- DNS 只监听 `127.0.0.1:1053`。
- TUN 劫持 `udp://any:53` 和 `tcp://any:53`。
- `nameserver`、`direct-nameserver`、`proxy-server-nameserver` 都走 `🚀 节点选择`。
- 国内域名策略也不再使用 `#DIRECT` DNS。

如果客户端启动后无法解析订阅节点，通常是因为节点服务器本身是域名，而严格模式下代理还没建立就要先通过代理解析节点域名。此时可只在本地 `config.local.yaml` 中把 `proxy-server-nameserver` 临时改为：

```yaml
proxy-server-nameserver:
  - https://dns.alidns.com/dns-query#DIRECT
  - https://doh.pub/dns-query#DIRECT&h3=false
```

这只用于解析代理节点服务器域名，不用于普通网站域名；但会让 DNS 服务商知道你解析过哪些节点域名。

如果 DNS 泄漏测试仍看到本地运营商 DNS，优先检查：

- Clash/Mihomo 客户端是否真正启用了 TUN。
- Windows 是否允许客户端安装/启用虚拟网卡和 DNS 劫持。
- 浏览器是否开启了自带“安全 DNS / Secure DNS / DoH”，建议关闭或设为跟随系统。
- 是否正在使用没有加载本模板的旧配置。

## 公开仓库更新

只提交模板：

```powershell
git add clash/template.yaml clash/README.md .gitignore
git commit -m "clash: add safe mihomo template"
git push
```

不要提交 `config.local.yaml`、订阅链接、节点密码、token。
