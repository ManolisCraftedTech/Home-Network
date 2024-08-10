<h1># Mikrotik routerboard Firewall rules Configuration </h1>

<h2>- Block  Incoming Ping Requests</h2>

/ip firewall filter add chain=input protocol=icmp action=drop comment="Block all incoming ping requests (ICMP)"

- Drop Traffic to Unused Ports

/ip firewall filter add chain=forward protocol=tcp dst-port=21,23,25,110,143,389,4433,4445,5555,6666 action=drop comment="Drop traffic to unused ports"

- Drop Invalid Connections  attempting to exploit vulnerabilities.

/ip firewall filter add chain=input connection-state=invalid action=drop comment="Drop invalid connections"
/ip firewall filter add chain=forward connection-state=invalid action=drop comment="Drop invalid connections in forwarded traffic"

- Allow only My ip for configuration via Winbox ON mikrotik.

/ip firewall filter add chain=input src-address=(My ip )protocol=tcp dst-port=8291 action=accept comment="Allow WinBox from MY IP "
/ip firewall filter add chain=input protocol=tcp dst-port=8291 action=drop comment="Drop all other WinBox access"

- Block ICMP (Ping) Between Networks

/ip firewall filter add chain=forward protocol=icmp action=drop comment="Block ICMP between networks"

- Drop TCP Port Scanners

/ip firewall filter add chain=input protocol=tcp connection-limit=5,32 action=drop comment="Drop TCP port scanners"

- Block Management Access from the Internet

/ip firewall filter add chain=input in-interface=<WAN_interface> protocol=tcp dst-port=22,8291,80,443 action=drop comment="Block management access from the internet"

- Drop Excessive SYN Packets

/ip firewall filter add chain=input protocol=tcp connection-state=new limit=10,30:packet action=accept comment="Allow new connections up to rate limit"
/ip firewall filter add chain=input protocol=tcp connection-state=new action=drop comment="Drop excessive new SYN packets"

- Block Common Malware Ports

/ip firewall filter add chain=forward protocol=tcp dst-port=22,23,25,3389,4444,5555,5900,8080,8081,31337 action=drop comment="Block common malware ports"
/ip firewall filter add chain=forward protocol=udp dst-port=69,1080,61616 action=drop comment="Block common UDP ports used by malware"

- Log of dropped Packets

/ip firewall filter add chain=input action=log log-prefix="Dropped Input: " log=yes comment="Log dropped packets to the router"
/ip firewall filter add chain=forward action=log log-prefix="Dropped Forward: " log=yes comment="Log dropped forwarded packets"
