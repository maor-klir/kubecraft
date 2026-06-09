# Deliverables

## Output of `show interface lo0`

```bash
~/repositories/github/maor-klir/network-fundamentals/lessons/clab/02-ip-fundamentals/ansible main*
❯ docker exec -it clab-ip-fundamentals-srl1 sr_cli -c "show interface ethernet-1/1"
==========================================================================================================================================
ethernet-1/1 is up, speed 25G, type None
  ethernet-1/1.0 is up
    Network-instances:
      * Name: default (default)
    Encapsulation   : null
    Type            : routed
    IPv4 addr    : 10.1.1.1/24 (static, preferred, primary)
------------------------------------------------------------------------------------------------------------------------------------------
==========================================================================================================================================
```
