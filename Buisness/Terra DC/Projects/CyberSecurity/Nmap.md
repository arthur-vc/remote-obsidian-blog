sudo -i
# Commands
nmap -p- : all ports
nmap -p : specific port
-A : aggresive
-sV : service version
-sS : sneaky half handshake - SYN then RST
-sT : full handshake
--script vuln : vulnerabilities
.0/24 : all ip's from 0-255
-oA /Documents/.../<filename> : saves 3 outputs simulatneously
-sn : checks live host servers



sudo nmap -p- -A -oX /<filepath> <ip>

