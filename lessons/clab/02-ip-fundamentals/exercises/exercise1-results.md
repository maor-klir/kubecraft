# Deliverables

## Which piongs succeeded and which failed

### Succeeded

Host-to-router:

```bash
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/02-ip-fundamentals/ansible main*​
❯ docker exec clab-ip-fundamentals-host1 ping -c 3 10.1.1.1
PING 10.1.1.1 (10.1.1.1): 56 data bytes
64 bytes from 10.1.1.1: seq=0 ttl=64 time=3.409 ms
64 bytes from 10.1.1.1: seq=1 ttl=64 time=1.334 ms
64 bytes from 10.1.1.1: seq=2 ttl=64 time=2.269 ms

--- 10.1.1.1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 1.334/2.337/3.409 ms

~/repositories/github/maor-klir/network-fundamentals/lessons/clab/02-ip-fundamentals/ansible main*​ 2s
❯ docker exec clab-ip-fundamentals-host2 ping -c 3 10.1.3.1
PING 10.1.3.1 (10.1.3.1): 56 data bytes
64 bytes from 10.1.3.1: seq=0 ttl=64 time=1.673 ms
64 bytes from 10.1.3.1: seq=1 ttl=64 time=1.519 ms
64 bytes from 10.1.3.1: seq=2 ttl=64 time=1.426 ms

--- 10.1.3.1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 1.426/1.539/1.673 ms
```

Router-to-router:

```bash
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/02-ip-fundamentals main*​ 2s
❯ docker exec -it clab-ip-fundamentals-srl1 sr_cli
Using configuration file(s): []
Welcome to the srlinux CLI.
Type 'help' (and press <ENTER>) if you need any help using this.
--{ + running }--[  ]--
A:srl1# ping 10.1.2.2 network-instance default
Using network instance default
PING 10.1.2.2 (10.1.2.2) 56(84) bytes of data.
64 bytes from 10.1.2.2: icmp_seq=1 ttl=64 time=101 ms
64 bytes from 10.1.2.2: icmp_seq=2 ttl=64 time=2.96 ms
64 bytes from 10.1.2.2: icmp_seq=3 ttl=64 time=2.84 ms
64 bytes from 10.1.2.2: icmp_seq=4 ttl=64 time=3.27 ms
64 bytes from 10.1.2.2: icmp_seq=5 ttl=64 time=3.20 ms
64 bytes from 10.1.2.2: icmp_seq=6 ttl=64 time=2.90 ms
64 bytes from 10.1.2.2: icmp_seq=7 ttl=64 time=2.88 ms
^CCommand execution aborted : 'ping 10.1.2.2 network-instance default'
--{ + running }--[  ]--
A:srl1# exit
```

### Failed

Cross-subnet:

```bash
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/02-ip-fundamentals main*​ 43s
❯ docker exec clab-ip-fundamentals-host1 ping -c 3 10.1.3.2
PING 10.1.3.2 (10.1.3.2): 56 data bytes
^C
```

## ARP tables

```bash
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/02-ip-fundamentals main*​ 10s
❯ docker exec clab-ip-fundamentals-host1 arp -n
? (10.1.1.1) at 1a:79:02:ff:00:01 [ether]  on eth1
? (172.20.20.1) at b2:cd:94:29:55:f7 [ether]  on eth0

~/repositories/github/maor-klir/network-fundamentals/lessons/clab/02-ip-fundamentals main*​
❯ docker exec -it clab-ip-fundamentals-srl1 sr_cli -c "show arpnd arp-entries"
+-------------+-------------+----------------+-------------+--------------------------+--------------------------------------------------+
|  Interface  | Subinterfac |    Neighbor    |   Origin    |    Link layer address    |                      Expiry                      |
|             |      e      |                |             |                          |                                                  |
+=============+=============+================+=============+==========================+==================================================+
| ethernet-   |           0 |       10.1.1.2 |     dynamic | AA:C1:AB:AD:9B:AC        | 3 hours from now                                 |
| 1/1         |             |                |             |                          |                                                  |
| ethernet-   |           0 |       10.1.2.2 |     dynamic | 1A:08:03:FF:00:01        | 3 hours from now                                 |
| 1/2         |             |                |             |                          |                                                  |
| mgmt0       |           0 |    172.20.20.1 |     dynamic | B2:CD:94:29:55:F7        | 3 hours from now                                 |
+-------------+-------------+----------------+-------------+--------------------------+--------------------------------------------------+
--------------------------------------------------------------------------------------------------------------------------------------------
  Total entries : 3 (0 static, 3 dynamic)
--------------------------------------------------------------------------------------------------------------------------------------------
Parsing error: Unknown token 'commit'. Options are ['!', '#', '/', 'acl', 'back', 'bash', 'bfd', 'cls', 'date', 'diff', 'echo', 'enter', 'environment', 'exit', 'file', 'help', 'history', 'info', 'interface', 'list', 'monitor', 'network-instance', 'passwd', 'ping', 'ping6', 'platform', 'pwc', 'qos', 'quit', 'references', 'routing-policy', 'save', 'show', 'source', 'ssh', 'system', 'tcptraceroute', 'tech-support', 'tools', 'traceroute', 'traceroute6', 'tree', 'tunnel', 'tunnel-interface', 'watch', '{']
```

## Why cross-subnet ping failed

There's no defined route to reach the other host across each side of this chained linkage (host1 --> host2 and vice versa).
