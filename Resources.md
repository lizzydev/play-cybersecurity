# [Hack The Box](https://app.hackthebox.eu)
## Tools Needed
* [OpenVPN](https://openvpn.net/)
* [nmap](https://nmap.org/)
* [OWASP Zap](https://www.zaproxy.org/)
* [sqlmap](https://github.com/sqlmapproject/sqlmap)

## Misc
Hack the box works by hosting vulnerable systems inside of a vpn.
In order to get to the system at all, you need to be connected to their
provided vpn, hence why OpenVPN was required.

You get a generated .ovpn file for the set of machines you're working on
(I started with the *Starting Point* machines), import that .ovpn file int
OpenVpn and get to work!