---
layout: note
title: "полезные функции и утилиты"
---

наглядная теория про ip-адреса и маски
```python
ipaddr_node = '192.168.0.0'
ipaddr_mask = '255.255.255.240'

def to_binary_magic(ip):
    return ''.join([bin(int(x)+256)[3:] for x in ip.split('.')])

from ipaddress import ip_network
net = ip_network(ipaddr_node + '/' + ipaddr_mask, strict=False)

print(ipaddr_mask, to_binary_magic(ipaddr_mask), sep='\t')

for i, ip in enumerate(net):
    to_print = "\t".join([str(ip), to_binary_magic(str(ip))])
    if i == 0:
        to_print += "\tNetwork Address"
    elif i == net.num_addresses - 1:
        to_print += "\tBroadcast Address"
    print(to_print)
```