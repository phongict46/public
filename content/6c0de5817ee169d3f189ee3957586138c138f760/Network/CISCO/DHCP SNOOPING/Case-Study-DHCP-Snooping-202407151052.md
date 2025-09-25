- dhcp snooping need trust on interface connect with devices layer 2 (switch to switch) NOTE: don't trust interface connect to client.
- in the sernario Router On A Stick for vlan communication need declare SVI (sub virtual inteface) and trust interface on switch connect to router.
- Can save dhcp database snooping on local of devices(comand "flash: Name-db") or storge with URL extend (comand: "ip dhcp snooping database tftp://ip/directory").
- Example: diagram in enterprise have three switch, sw 1 is core and configure dhcp snooping, sw 2 connect with sw 1, sw 3 connect with sw 2. How dhcp snooping can apply to sw 2 and sw 3 ?

