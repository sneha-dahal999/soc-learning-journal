# Day 3: Linux Networking Investigation

## Scenario

A SOC alert reported external network activity from a company-managed Ubuntu endpoint. I was assigned to establish the endpoint's identity and network baseline, verify connectivity, inspect local services, and trace the external network path.

## Objectives

- Identify the active user and endpoint hostname
- Inspect network interfaces and assigned addresses
- Identify the default gateway and local route
- Verify local and external connectivity
- Test DNS and HTTPS communication
- Examine neighbour-table information
- Review listening ports and active services
- Trace the initial external network path

## Environment

- Ubuntu 24.04 LTS through WSL2
- Private virtual IPv4 networking
- systemd service manager
- Authorised local lab environment

## Investigation Summary

The endpoint contained an active loopback interface and an operational virtual Ethernet interface. The Ethernet interface held a private WSL address and used a private virtual gateway.

The gateway responded successfully to ICMP testing with no packet loss. An external IP address also responded successfully, confirming that traffic could travel beyond the local network.

DNS successfully translated a domain name into IPv4 and IPv6 addresses. An HTTPS header request returned a successful HTTP response, confirming that routing, DNS, TCP, TLS, and HTTP communication were operational.

The neighbour table associated the gateway IP address with a virtual MAC address. Its state changed from STALE to REACHABLE after renewed communication.

Listening-port inspection identified local DNS sockets on port 53. Process inspection associated them with the systemd-resolved service. The service was active and processing requests. No established TCP connections were observed during the captured snapshot.

Path tracing showed the expected initial route through the WSL gateway, local network gateway, and provider network. Later devices did not respond to trace requests, which does not by itself indicate a network failure.

## Findings

1. The endpoint's virtual Ethernet interface was active.
2. The configured source address and routing table were consistent.
3. The default gateway was reachable.
4. External IPv4 connectivity was operational.
5. DNS resolution worked for IPv4 and IPv6.
6. The tested HTTPS service returned a successful response.
7. Local DNS listeners were associated with a legitimate system service.
8. No established TCP sessions were visible during inspection.
9. The initial external route followed the expected network path.
10. No clear evidence of malicious network activity was identified.

## Security Conclusion

The investigated network activity was consistent with normal endpoint communication. The collected evidence confirmed working local networking, gateway access, external connectivity, DNS resolution, and HTTPS communication.

A lack of established connections in one snapshot does not prove that no previous connections existed. Similarly, listening ports are not automatically malicious; their associated processes and business purposes must be verified.

## Commands Practised

```bash
whoami
hostname
hostname -I
ip addr
ip route
ping -c 4 <gateway>
ping -c 4 1.1.1.1
getent hosts example.com
ip neigh show
ss -tuln
ss -tn state established
ip -s link show eth0
sudo ss -tulnp
ps -p 1 -o pid,user,comm,args
systemctl --type=service --state=running --no-pager
systemctl status systemd-resolved --no-pager
ip route get 1.1.1.1
cat /etc/resolv.conf
getent ahostsv4 example.com
curl -I --max-time 10 https://example.com
tracepath -m 8 -n 1.1.1.1
tracepath -m 8 example.com
