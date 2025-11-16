# IPv6

1. IPv6 Address Structure & Rules
Basic Format

IPv6 has 8 blocks, each block has 4 digits which may contain alphabets and numbers.

Zero Compression Rules

Leading zeros in a block can be removed.
Example:
2001:0db8:85a3:0000:0000:8a2e:0370:7334 →
2001:db8:85a3:0:0:8a2e:370:7334

A full zero block can be replaced with ::

You can ONLY use :: one time in an address.

:: means “one or more blocks of zeros”.

Example:
2001:0db8:85a3:0000:0000:8a2e:0370:7334 →
2001:db8:85a3::8a2e:370:7334

2. IPv6 Address Types
2.1 Global Unicast Address (GUA)

IPv6 version of public WAN IP.

Provided by ISP like public IPv4.

Starts with: 2000::/3
(Any address starting with 2xxx or 3xxx)

Example:
2a02:8440:715c:4cdf:c0a6:459f:0ddc:f5c1

First 4 blocks = provided by ISP

Last 4 blocks = Interface ID (device generated)

2.2 Link-Local Address (LLA)

Automatically created by every IPv6 device.

Starts with fe80::/10

Used for:

Neighbor Discovery

Router Discovery

SLAAC

Local communication

Format: fe80:xxxx:xxxx:xxxx:…

2.3 Unique Local Address (ULA)

Used in LAN.

Like private IPv4.

Not automatic.

Prefix: fc00::/7

Examples:
fd12:3456:789a::1
fd00:abcd::5
fd34:55aa:1::10

2.4 ULA vs LLA
Scenario	Uses LLA?	Uses ULA?
Talking to router	✔ Yes	✖ No
Finding neighbors	✔ Yes	✖ No
Local-only traffic	✔ Yes	✖ No
Communication across LAN segments	✖ No	✔ Yes
Internal servers	✖ No	✔ Yes
Internet access	❌ No	✔ (only inside LAN; GUA used for internet)
3. Duplicate Address Detection (DAD)

Before the device uses an IPv6 address, it asks the LAN:

“Is anyone using this address?”

If duplicate → ❌ Address rejected

If not → ✔ Device uses the address

IPv4 has no automatic duplicate protection.

4. Network Prefix & Interface ID

Example Address:
2001:db8:1234:5678:abcd:ef12:3456:7890

Network prefix: 2001:db8:1234:5678:

Interface ID: abcd:ef12:3456:7890

Interface ID generation methods

EUI-64 (based on MAC)
Example:
MAC: A2-BB-CC-DD-EE-FF
IID: a0bb:ccff:fedd:eeff

Randomized IID (Privacy Extensions)

Random

Changes periodically

Used for GUA to protect privacy

Which method is used?
Address Type	IID Type Used	Why
Link-Local (fe80::)	Often EUI-64 or Stable IID	Needs to be stable for LAN functions
GUA (2000::)	Randomized IID	Privacy
ULA (fdxx::)	Usually stable IID	Internal networks
5. Prefix Length

99% of IPv6 uses /64

First 64 bits = network prefix

Last 64 bits = Interface ID

6. IPv6: No Broadcast

IPv6 uses multicast instead of broadcast.

Common multicast groups:

ff02::1 — all nodes

ff02::2 — all routers

ff02::1:ffXX:XXXX — solicited-node (used by NDP)

ff02::1:2 — DHCPv6 servers

IPv6 also supports anycast.

Example:
2001:db8:100::53

7. Neighbor Discovery Protocol (NDP)

NDP replaces:

ARP

Parts of DHCP

Some ICMP functions

What NDP does:

Address resolution (who has this IPv6? what is your MAC?)

Uses NS → NA

Router Discovery

Device sends RS

Router sends RA

Prefix Discovery

Router tells devices the prefix, SLAAC, DHCP, DNS

Duplicate Address Detection (DAD)

Device sends NS for its own address

If no response → safe

8. If Router Does Not Support IPv6

Works:

LLA

Does NOT work:

❌ GUA

❌ ULA

❌ DNS via IPv6

❌ Default gateway

No RA → no IPv6 routing.

9. SLAAC (Stateless Address Autoconfiguration)
Steps:

Device creates LLA

Device sends RS

Router sends RA

Device generates Interface ID

Device combines Prefix + IID

DAD check

Device now has GUA or ULA

10. DHCPv6

Optional (unlike IPv4).

Feature	SLAAC	Stateful DHCPv6	Stateless DHCPv6
IPv6 address	✔ Yes	✔ Yes	✔ Yes (via SLAAC)
DNS	✔ Maybe	✔ Yes	✔ Yes
Gateway	✔ Yes (RA)	✔ Yes (RA)	✔ Yes (RA)
Full admin control	❌ No	✔ Yes	❌ No

DHCPv6 cannot give default gateway.

11. Default Gateway in IPv6

Learned through Router Advertisements

Gateway is usually router's link-local address

If RA missing → no IPv6 routing at all

12. IPv6 Packet Structure
Field	Purpose
Source	Who sent the packet
Destination	Who receives it
Hop Limit	Prevent infinite loops
Next Header	TCP/UDP/ICMPv6
Flow Label	Group packets from same flow

Simpler than IPv4 (no checksum, no fragmentation by routers).

13. IPv6 Multicast Groups Summary

ff02::1 — all devices

ff02::2 — all routers

ff02::1:ffXX:XXXX — solicited-node (for NS instead of ARP)
