# Argo-PQC Strong

这是 Argo-PQC 的强化版安装包，默认目标是：

- 只启用 VLESS 家族协议，默认最简为 `VLESS + WS`。
- 强制启用 VLESS Encryption：`mlkem768x25519plus`。
- 默认严格模式：如果 Xray 不支持 `xray vlessenc` 或生成结果不是 `mlkem768x25519plus.*`，安装中止。
- 默认禁用 0-RTT，把生成参数规范化为 `native.600s`，更偏向保守安全。
- 敏感文件默认 `umask 077`，`/etc/argox` 与 custom/inbound/tunnel/systemd unit 尽量限制 root 可读。
- 安装后校验 `inbound.json` 是否存在 `decryption=mlkem768x25519plus*`。

## 一键部署

```bash
wget -qO argox.sh https://raw.githubusercontent.com/hkzping999/Argo-PQC-Strong/main/argox.sh
bash -n argox.sh && sudo bash argox.sh -l
```

如果你使用本包中的加强版脚本：

```bash
chmod +x argox.sh
bash -n argox.sh && sudo bash argox.sh -l
```

## 使用配置文件部署

```bash
sudo bash argox.sh -f config-pqc-strong.conf
```

默认配置只安装：

```text
VLESS + WS + VLESS Encryption mlkem768x25519plus
```

## 允许兼容协议

默认 `ALLOW_LEGACY_PROTOCOLS='n'`，VMess/Trojan/Shadowsocks/Hysteria2 会被过滤掉。

确实需要兼容旧客户端时，改为：

```bash
ALLOW_LEGACY_PROTOCOLS='y'
INSTALL_PROTOCOLS='efgh'
```

注意：只有 VLESS 家族协议支持 VLESS Encryption PQC；VMess/Trojan/Shadowsocks 不是 VLESS PQC。

## 验证是否启用 PQC

安装后检查：

```bash
grep -R '"decryption"[[:space:]]*:[[:space:]]*"mlkem768x25519plus' /etc/argox /usr/local/etc/xray 2>/dev/null
grep -R 'encryption=mlkem768x25519plus' /etc/argox /root 2>/dev/null
systemctl status xray --no-pager
systemctl status argo --no-pager
```

订阅链接中应出现：

```text
encryption=mlkem768x25519plus.native.600s...
```

服务端配置中应出现：

```json
"decryption": "mlkem768x25519plus.native.600s..."
```

如果看到 `encryption=none` 或 `"decryption": "none"`，说明不是 PQC VLESS 节点。

## 安全说明

PQC 增强的是密钥协商的长期保密性，尤其用于抵抗“先抓包、未来量子计算解密”的风险。它不等于流量不可识别，也不保证规避 DPI。外部仍可能识别 Cloudflare、TLS、WebSocket、长连接等流量形态。

不要把真实 Cloudflare Token、订阅链接、UUID、VLESS encryption/decryption 参数提交到 GitHub 或公开日志。
