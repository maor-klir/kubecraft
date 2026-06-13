# Deliverables

## Interface status output showing the disabled link

```sh
❯ docker exec -it clab-routing-basics-srl1 sr_cli -c "show interface brief"
+---------------------+----------------------+----------------------+----------------------+----------------------+----------------------+
|        Port         |     Admin State      |      Oper State      |        Speed         |         Type         |     Description      |
+=====================+======================+======================+======================+======================+======================+
| ethernet-1/1        | enable               | up                   | 25G                  |                      |                      |
| ethernet-1/2        | enable               | up                   | 25G                  |                      |                      |
| ethernet-1/3        | disable              | down                 | 25G                  |                      |                      |
```

## Explanation of why route existence does not guarantee path availability

A route present in a routing table is a forwarding instruction. It does not guarantee forwarding execution.  
When an interface (the physical link) is disabled, a router cannot forward the packet to the next hop.  
Important to note: a static route will not get removed from a routing table when a link goes down.

