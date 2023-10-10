# Security in physical layer
## Fundamentals of physical layer security
- Physical layer contains: 
	- Definition of hardware specifications
	- Encoding and signaling
	- Data transmition and reception
	- Topology and physical network design
- In all communication systems, the issues of authentication, confidentiality and privacy are handled in the upper layers of the protocol stack using variation of private-key and public-key system.
- Nowadays, many results from information theory, signal processing, and cryptography suggest that there is much security to be gained by accounting for the imperfections of the **physical layer** when designing secure systems.

**Physical layer threats**
- Network are made up of devices and communication links.
- Devices and links can be physically threatened
- Vandalism, lightning, fire, excessive pull force, corrolision, wiretaping, crosstalk
- We need to make networks mechanically resilient and trustworthy

**Types of media** 
- Metal: wired
- EM/RF: wireless
- Light: optical firber

**Physical tapping**
- Conductive taps
	- Form conductive connection with cable
- Inductive taps
	- Passively read signal from EM induction
	- No need for any direct physical connection
	- Harder to detect
	- Harder to do with non-electric conductors

==> To achieve security in Physical layer, there are multiple approaches
	- Preprocessing Scheme
		- Coding
		- Key generation
		- Articial noise scheme
	- Game theory scheme
	- Signal processing
	- Cooperation comunications
	- Etc

## Security in WLAN
- A typical WLAN consist of 2 parts: wireless access devices, Access points
- WLAN deployment: 
	- Ad-hoc: no AP, P2P
	- BSS: Centralized connection
	- ESS: Centralized connection, extend by connect BSSs
**Threat in WLAN**:
- Jamming: create noise by sending stronger signals with the same frequency
-> Purpose
	- Fake computer
	- Rouge AP
	- DoS: destroy connection between host and AP
- Disassociation: send disassociation frame to host or AP to disrupt the current connection
- Deauthentication attack: send deauthentication frame to host to require re-authentication
-> Purpose: 
	- Capture WiFi password
	- Let the host to connect to Rouge AP
- Rouge AP
	- Collective authentication information between AP and host
	- Active eavesdrop
	- Attack man-in-the middle
	- Defence: use network management tool to find the connected strange device

## Security in LAN

**Attack switch**
- MAC flooding: send packets with fake MAC address -> MAC table is overloaded -> the normal packets are forwarded by broadcasting: 
	- Broadcasting storm -> consume bandwidth and resource of other nodes
	- Eavesdropping attack
- Fake MAC address
- Defence: Port security

**Attack ARP protocol**
- Address resoution protocol: find MAC of an IP
	- Broadcast ARP Request is connectionless
	- No authentication in ARP Response
- Attack: 
	- ARP Spoofing
	- Dos
- More dangerous if the MAC address of gateway router, local DNS server are fake: 
	- Eavesdropping
	- Man-in-the-middle
- Defence: Dynamic ARP Inpection

**Attack DHCP**
- Dynamic Host Configuration Protocol
- Provide IP address automatically newcomming hosts: 
	- IP
	- Gateway router
	- DNS server
- User UDP, port 67(server) and 68(client)
- DHCP Starvation: 
	- Any client/server can request an IP allocation => DoS to drain the IP address => DHCP starvation
- DHCP Spoofing:
	- Can't verify the information from DHCP server => DHCP spoofing 
	- Exploit potential: 
		- Replace DNS server address by the DNS address of the attacker
		- Replace the defaut router address, which allow attacker to 
			- Block or eavesdropping information
			- Replay attack
			- Man-in-the-middle attack

**Attack VLAN**
- VLAN: logic broadcast domain on switch -> split traffic in layer 2
	- Access control 
	- Resource separation
- Each VLAN has a different IP range
- Ethernet frame added VLAN tag (802.1Q or ISL)
- Frame switch within the same VLAN

**** 
# Network layer

## Switching loop
- If there is more than one path between two switches:
	- Forward tables become unstable: Source MAC addresses are repeatedly seen coming from different port
	- Switches will broadcast each other's broadcasts 
		- All available bandwidth is utilized
		- Switch processors cannot handle the load
- Redundant paths improve resilience when: 
	- A switch fails 
	- Wiring breaks
=> How to create redundancy without creating dangerouse traffic loops ?
**Redundancy**
- Achieving such goal requires extremely reliable networks.
- Reliability in networks is achived by reliable equipment and by. designing networks that are tolerant to failures and faults.
- The network is designed to reconverge rapidly so that the fault is bypassed.
- Fault tolerance is achieved by redundancy.
- Redundancy means to be in excess or exceeding what is usual and natural.
**Type of traffic**:
- Known unicast: Destination addresses are in switch tables
- Unknown unicast: Destination addresses are not in switch tables
- Multicast: Traffic sent to a group of addresses
- Broadcast: Traffic forwarded out all interfaces except incoming interface
**Redundant switched topologies**
- Switches learn the MAC addresses of devices on their ports so that data can be properly forwarded to the destination
- Switches will flood frames for unknown destinations until they learn the MAC addresses of the devices.
- Broadcasts and multicast are also flooded 
- A redundant switched topology may cause broadcast storms, multiple frame copies, and MAC address table instability problems.



# Application layer

## Security in application level

### DNS security

**Threat**
- Threats to lookup secrecy: Definition of DNS system says this data isn't secret
- Threats to DNS information integrity: Very important, since everythings trusts that this translation is correct
- Threats to DNS availability: Potential disrupt Internet service
=> The core problem is that DNS aren't authenticated

**DNS hijacking**
- Wait for resolver(target) to ask about a name from victim's domain
	- Provide reply faster than auth server
	- Provide some extra information 
- This poison the cache at that resolver, not globally
- Attack's goal: Impersonate the victim

=> Solution:
- Only accept auth if its domain is the same about the domain you asked for
- Don't accept extra info in the reply if you did not ask for it

**Short-circuiting waiting**

- Ask the target resolver about some name from the victim's domain
	- Doesn't have to be the name you intend to hijack since DNS will take extra info in the reply 
	- Asking about non-exist names guarantees they are not in resolver's caches already
- Can ask about different domain

**Hijacking with sniffing**

- If on the same network as the target, attacker can try to ARP spoof the next hop
	- Position itself on the path of target's requests 
	- DNS traffic uses UDP and is not encrypted, easy to see port and request ID and provide appropriate reply

**Hijacking with guessing**

- If target uses predictable numbers for source port and request ID attacker can perform many attacks, each time trying to guess the right port/replyID combination

**Defenses**

- Randomize source port
	- Makes guessing harder but not impossible
- Use DNSSEC 
	- Replies must be signed by auth
	- Everyone can check signatures
	- Auth servers for zones sign certificates when they delegate a sub-zone
	- Requires clients to implement DNSSEC to verify replies


### Email security
**Threats**:
- Loss of confidentiality
- Loss of integrity
- Lack of data origin authentication
- Lack of non repudiation
- Lack of notification of receipt

**Solution**:
- Secure the server to client connections
- Secure the end-to-end email delivery
- Email based attacks
	- Active content attack 
	- Buffer over-flow attack
	- Trojan horse
	- Web bugs(for tracking)
- Secure E-mail Standards and products
	- PEM
	- X.400
	- S/MIME:
		- Version: must be "1.0" -> RFC 2045, RFC 2046
		- Content-type: More types being added by developers
		- Content-Transfer-Encoding: How message has been encoded (radix-64)
		- Content-ID: Unique identifying character string.
		- Content Description: Needed when content is not readable text
		- Enveloped Data: Encrypted content and encrypted session keys for recipients
		- Signed data: Message Digest encrypted with private key of "signer"
	- PGP
- 
