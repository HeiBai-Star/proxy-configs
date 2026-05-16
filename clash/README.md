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

## 公开仓库更新

只提交模板：

```powershell
git add clash/template.yaml clash/README.md .gitignore
git commit -m "clash: add safe mihomo template"
git push
```

不要提交 `config.local.yaml`、订阅链接、节点密码、token。
