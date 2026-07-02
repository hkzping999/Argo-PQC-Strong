This hotfix fixes VLESS Encryption client-side RTT formatting.

- Server inbound `decryption` third block remains a ticket time such as `600s`.
- Client outbound/share-link `encryption` third block is now `1rtt` by default, not `600s`.
- Strong mode still disables 0-RTT by default.

Expected client link format:

```
bash <(wget -qO- https://raw.githubusercontent.com/hkzping999/Argo-PQC-Strong/main/argox.sh)

```
