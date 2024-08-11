 <h3> <img width="64" height="64" src="https://img.icons8.com/cute-clipart/64/home.png" alt="home"/>  Home network Documentation </h3>

-  Connected the ISP router to the MikroTik router’s WAN port to ensure internet connectivity. This setup allows the MikroTik router to manage internal network traffic.

-  Configured the MikroTik router to manage the WiFi network with an IP range of 10.10.10.0/24. The DHCP server is set to assign IP addresses from 10.10.10.2 to 10.10.10.254, providing wireless devices with appropriate IP addresses.

 - Configured network with an IP range of 192.168.20.0/24 on the MikroTik router. The DHCP server allocates IP addresses from 192.168.20.2 to 192.168.20.254 to devices connected via Ethernet.

 - Implemented custom firewall rules on the MikroTik router to secure and control traffic between the WiFi and LAN networks, as well as to manage external access through the ISP router.

 - Added [Raspberry pi5](https://github.com/ManolisCraftedTech/RaspberryDNS) for DNS provider Ensuring ad avoidance by activating Pi hole blocklists
   


![WELCOME TO MY HOME NETWORK](https://github.com/ManolisCraftedTech/Home-Network/blob/main/%CE%97%CE%9F%CE%9C%CE%95%20%CE%9D%CE%95%CE%A4.drawio.png)


<h1> <img width="64" height="64" src="https://img.icons8.com/external-flatart-icons-lineal-color-flatarticons/64/external-firewall-web-hosting-flatart-icons-lineal-color-flatarticons-1.png" alt="external-firewall-web-hosting-flatart-icons-lineal-color-flatarticons-1"/> Firewall rules Configuration </h1>


🟢 Block  Incoming Ping Requests

/ip firewall filter add chain=input protocol=icmp action=drop comment="Block all incoming ping requests (ICMP)"

🟢Drop Traffic to Unused Ports

/ip firewall filter add chain=forward protocol=tcp dst-port=21,23,25,110,143,389,4433,4445,5555,6666 action=drop comment="Drop traffic to unused ports"

🟢Drop Invalid Connections  attempting to exploit vulnerabilities.

/ip firewall filter add chain=input connection-state=invalid action=drop comment="Drop invalid connections"

/ip firewall filter add chain=forward connection-state=invalid action=drop comment="Drop invalid connections in forwarded traffic"

🟢Allow only My ip for configuration via Winbox ON mikrotik.

/ip firewall filter add chain=input src-address=(My ip )protocol=tcp dst-port=8291 action=accept comment="Allow WinBox from MY IP "

/ip firewall filter add chain=input protocol=tcp dst-port=8291 action=drop comment="Drop all other WinBox access"

🟢Block ICMP (Ping) Between Networks

/ip firewall filter add chain=forward protocol=icmp action=drop comment="Block ICMP between networks"

🟢Drop TCP Port Scanners

/ip firewall filter add chain=input protocol=tcp connection-limit=5,32 action=drop comment="Drop TCP port scanners"

🟢Block Management Access from the Internet

/ip firewall filter add chain=input in-interface=<WAN_interface> protocol=tcp dst-port=22,8291,80,443 action=drop comment="Block management access from the internet"

🟢Drop Excessive SYN Packets

/ip firewall filter add chain=input protocol=tcp connection-state=new limit=10,30:packet action=accept comment="Allow new connections up to rate limit"

/ip firewall filter add chain=input protocol=tcp connection-state=new action=drop comment="Drop excessive new SYN packets"

🟢Block Common Malware Ports

/ip firewall filter add chain=forward protocol=tcp dst-port=22,23,25,3389,4444,5555,5900,8080,8081,31337 action=drop comment="Block common malware ports"

/ip firewall filter add chain=forward protocol=udp dst-port=69,1080,61616 action=drop comment="Block common UDP ports used by malware"

🟢Log of dropped Packets

/ip firewall filter add chain=input action=log log-prefix="Dropped Input: " log=yes comment="Log dropped packets to the router"

/ip firewall filter add chain=forward action=log log-prefix="Dropped Forward: " log=yes comment="Log dropped forwarded packets"

<h1>In the picture you can see all the firewall rules enabled.</h1>

![WELCOME TO MY HOME NETWORK](https://github.com/ManolisCraftedTech/Home-Network/blob/main/rules%20.png)
