# ArgoX PQC Hybrid Installer

本版本基于上传的 ArgoX 脚本改造，默认启用 VLESS 后量子混合加密。

## 默认部署

默认最简部署只安装：

- VLESS + WS + Cloudflare Tunnel
- VLESS Encryption：`mlkem768x25519plus`
- 服务端自动写入 `decryption`
- 订阅链接自动写入 `encryption`

`mlkem768x25519plus` 是 Xray VLESS Encryption 使用的混合模式，目标是 ML-KEM-768 + X25519。

## 一条命令安装

```bash
bash argox.sh -l
```

英文界面：

```bash
bash argox.sh -k
```

使用配置文件非交互安装：

```bash
bash argox.sh -f config.conf
```

## Cloudflare 固定隧道

编辑 `config.conf`：

```bash
ARGO_DOMAIN='argo.example.com'
ARGO_AUTH='你的 Cloudflare Tunnel Token / Json / API Token'
ENABLE_VLESS_PQC='y'
INSTALL_PROTOCOLS='e'
```

然后执行：

```bash
bash argox.sh -f config.conf
```

## 兼容模式

如果需要同时输出传统兼容节点，可把配置改成：

```bash
INSTALL_PROTOCOLS='efgh'
```

其中 `e` 是 VLESS + WS PQC，`f/g/h` 是 VMess/Trojan/SS 兼容节点。

## 失败策略

默认：

```bash
VLESS_PQC_STRICT='y'
```

如果当前 Xray 不支持 `vlessenc`，安装会中止，避免误以为已启用 PQC。

需要自动回退到传统 VLESS 时：

```bash
VLESS_PQC_STRICT='n'
```

## 客户端提醒

需要客户端支持 VLESS Encryption 的 `encryption=` 参数。已知新版本 Xray、部分 Mihomo/Happ 类客户端支持；老客户端可能导入成功但无法连接。

