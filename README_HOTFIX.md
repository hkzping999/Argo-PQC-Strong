# Argo-PQC-Strong Hotfix 2.2.1

This hotfix fixes VLESS Encryption client-side RTT formatting.

- Server inbound `decryption` third block remains a ticket time such as `600s`.
- Client outbound/share-link `encryption` third block is now `1rtt` by default, not `600s`.
- Strong mode still disables 0-RTT by default.

Expected client link format:

```
wget -qO argox.sh https://raw.githubusercontent.com/hkzping999/Argo-PQC-Strong/main/argox.sh
wc -l argox.sh
bash -n argox.sh
```

Expected server config format:

```json
"decryption": "mlkem768x25519plus.native.600s...."
```

Deploy:

```bash
chmod +x argox.sh
bash -n argox.sh && sudo bash argox.sh -l
```

If reinstalling over a broken 2.2.0 install, uninstall first or remove `/etc/argox/custom` so old `vlessPqcEncryption=...600s...` does not persist.

```bash
sudo bash argox.sh -u
# or, if you want a clean reset:
sudo rm -rf /etc/argox /etc/systemd/system/xray.service /etc/systemd/system/argo.service /usr/bin/argox
sudo systemctl daemon-reload
sudo bash argox.sh -l
```
