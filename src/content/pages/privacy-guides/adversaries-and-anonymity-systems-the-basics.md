---
title: 'Adversaries and Anonymity Systems: The Basics'
author: mirimir (gpg key 0x17C2E43E)
url: /privacy-guides/adversaries-and-anonymity-systems-the-basics/
section: Basic
weight: 20
date: 2014-09-04T08:10:03+00:00
layout: guides-details
---
There are three sorts of players in this game. First, there are users who communicate with other users and/or destinations. Second, there are adversaries (archetypic attackers) with goals such as observing communications, blocking communications, identifying users, associating users with other users and/or destinations, impersonating and/or compromising users and destinations, and so on.

Third, there are services and systems that protect users' communications, providing some mix of anonymity, freedom, privacy and security. Given how anonymity reduces the risk of targeted attack, it's useful to consider these as primarily `anonymity systems`. In this discussion, we first summarize background information about available anonymity systems. We then explore how each is vulnerable to adversaries with various capabilities.

It's crucial to keep in mind that none of these anonymity systems provide end-to-end encryption between users and Internet destinations. All traffic between users and system exit nodes is encrypted, of course. But traffic between exit nodes and destinations is not encrypted, unless users and destinations are employing end-to-end encryption (such as HTTPS for websites, TLS for email or SSH for remote login).

For email messages, anonymity systems do obscure the user's ISP-assigned IP address, but they don't affect other metadata, such as user's and correspondent's email addresses, message subject, and time. Even with end-to-end encryption between users and their email servers, message content is not encrypted between users' email servers and their correspondents, unless users and their correspondents are employing end-to-end encryption, such as OpenPGP.

## Anonymity Systems

Three types of low-latency anonymity systems are available for general Internet access. There are numerous VPN services, one mix network (JonDonym) and one onion-routing network (Tor). All employ encryption to provide privacy and security between users and system exits. Even so, it's always prudent to use end-to-end encryption, because system exits (and adversaries observing them and/or destinations) can otherwise see unencrypted traffic.

Each of these anonymity systems provides anonymity in a particular way, more or less effectively against various adversaries. Excluded from this discussion are various proxy services, such as SSH tunnels (which are harder to use), and web proxies and browser plug-ins (which are far easier to compromise). Also excluded are Freenet and I2P. Freenet is a P2P network designed for anonymous and takedown-resistant publishing, often among closed groups of trusted participants. I2P is a garlic-routing network that focuses primarily on P2P content sharing. Neither Freenet nor I2P focus on general Internet access, although I2P does have Internet gateways.

### VPN Services

VPN services are the simplest type of anonymity system. Once a user client and remote VPN server have negotiated an encrypted virtual network connection, the server acts as a proxy for all of the client's Internet traffic. Those services employing properly configured OpenVPN or IPsec protocols (but not the PPTP protocol) provide strong security and privacy (with perfect forward secrecy) between users and system exits.

VPN services provide privacy by hiding Internet destinations from ISPs. And they provide anonymity by hiding user information (such as ISP, identity and geolocation) from destinations. That is, both ends (and associated network observers) see only a VPN server's IPv4 address. Network lag (latency) is far lower than with either JonDonym or Tor, and speed (bandwidth) is 10-20 fold higher.

Reputable VPN services use perfect forward secrecy. For OpenVPN, that relies on TLS with transient symmetric session keys. The keys are negotiated on-the-fly, after the server and client have authenticated themselves. They are unpredictable, and frequently changed (by default, hourly). An adversary that compromises a particular session can decrypt only traffic from that session. Traffic from retained intercepts and traffic from future sessions remain secure, because they're encrypted with different session keys.

VPN services are very easy to set up and use, because providers handle the technical aspects. However, the privacy and anonymity that VPN services provide hinges entirely on the operator's integrity and discretion, on its technical competence, and on its ability to prevent adversaries from observing, manipulating and/or compromising its servers.

VPN services provide strong protection against local adversaries, and good protection against censorship and routine mass surveillance, even at the national level. However, they provide limited protection against adversaries with international reach. Such adversaries may coerce providers and/or their hosting providers or ISPs, and so may observe, manipulate and/or compromise their servers. They also provide limited protection against determined and resourceful censors. We discuss that further below, under `Passive Adversaries with Limited Network Reach`.

In some jurisdictions, VPN providers may be served with court orders that can not be disclosed without serious penalties. But there's a workaround: the warrant canary. As long as no such court order has been received, the provider may regularly publish a statement to that effect. If the warrant canary isn't renewed on schedule, users may safely infer that the provider has received such a court order. There is no need for the provider to take active steps that would violate the order.

Some VPN services provide multi-hop routing. Users' traffic is proxied, in turn, through servers in different nations. Given that, users sharing a given entry node are typically using different exit nodes, and users sharing a given exit node are typically using different entry nodes. Other VPN services rotate users' traffic among multiple exit servers. Such approaches protect better against adversaries with limited international reach. Even so, all bets are off for those who are targeted by more resourceful state adversaries.

### JonDonym

[JonDonym](https://anonymous-proxy-servers.net/) is a mix-based anonymity system, currently comprising 42 mixing servers (mixes). It is a closed system of trusted mixes. It provides anonymity through unpredictable randomization of traffic through fixed cascades of anonymizing mixes (`mixing`). Each mix delays incoming packets, from multiple user clients or adjacent mixes, for random periods of time, and then forwards them in randomized order. JonDonym mixes strip layers of encryption to anonymize traffic from non-adjacent mixes.

Six free two-mix JonDonym cascades are available, and ten premium three-mix cascades. Unlike multi-hop VPN services and Tor, each mix participates in just one cascade, and so each cascade has a static pair of entry and exit IP addresses (as do one-hop VPN services). As with VPN services, anonymity ultimately relies on mix operators' integrity and discretion, and on their ability to prevent adversaries from observing, manipulating and/or compromising their servers. Still, given mixing and the distribution of trust among multiple mix operators, JonDonym generally provides stronger anonymity than VPN services can, even those services that provide multi-hop routing.

Mix operators must be verified and certified by JonDos GmbH, and cascades are created through negotiation among mix operators. The entry cost of becoming a mix operator is nontrivial, given oversight by the existing community of trusted mix operators. There is no such process for operators of Tor relays. There is no firm requirement to even provide contact information. Tor developers do monitor relay behavior, and relays that behave maliciously are restricted, and eventually banned. Even so, there's constant flux of Tor relays, but JonDonym mix operators can't come and go so easily.

### Tor

[Tor](https://www.torproject.org/) is a second-generation onion-routing anonymity system, currently comprising about 6000 anonymizing relays. It is an open system, with highly distributed trust, and no centralized ownership. It provides anonymity through dynamic, unpredictable and hard-to-trace routing through a large network of untrusted relays. Unlike VPN services and JonDonym, adversaries are free to participate by running relays. Even so, there is oversight by a core group of trusted developers and relay operators, and there is a vetting process for new relays.

User clients connect through the Tor network, creating encrypted three-relay circuits at random, and changing them frequently. Circuit traffic is relayed in fixed-size (512-byte) cells. At each step, relays remove a layer of encryption. That prevents non-adjacent relays from identifying each other, and helps protect against malicious relays. Traffic between relays is TLS encrypted, on top of the onion-routing circuit encryption. That somewhat obscures the circuit's cell pattern (number and timing) from external adversaries. However, unlike JonDonym mixes, Tor relays do not explicitly mix traffic.

Although the Tor network is far larger than JonDonym, many of its 6000 relays have limited uptime, limited usable bandwidth, and/or exit restrictions (e.g., blocking IRC). Such limitations reduce the network's effective size, and they also increase its vulnerability to adversaries who can introduce numerous attractive relays.

## Adversaries

All low-latency anonymity systems are broken against adversaries that can observe, manipulate and/or compromise both ends of a connection. That is certainly so for VPN services, JonDonym and Tor. Increasing the number of intervening system nodes doesn't prevent such compromise. Conversely, all three systems protect well against weak local adversaries. However, one can distinguish them by considering their vulnerability to three canonical classes of attackers, or adversaries, each resourceful in distinct ways.

Passive adversaries simply intercept and analyze network traffic, seeking to correlate streams entering and exiting anonymity systems. Byzantine adversaries can mark or otherwise modify traffic, primarily to facilitate traffic correlation. Realistic passive adversaries are Byzantine, and so we lump them together. However, there is a key distinction: anonymity systems can't detect purely passive adversaries, except through consequent Byzantine activity, and so active defense against them is problematic.

There are two sorts of active adversaries. Sybil adversaries focus on system-level vulnerabilities, and exploit them by running numerous malicious clients and/or network nodes. Sybil is the pseudonym of the patient in a well-known book about multiple-personality disorder. In this context, its use puns on the strategy of using numerous, apparently independent puppets in a collective attack.

Coercive adversaries focus on security vulnerabilities of particular network nodes, and exploit them appropriately, seeking system compromise. They may also go after system operators, employing social engineering or phishing attacks, physical attacks, political or legal authority, and so on. They may also carry out such attacks against high-value users. These are highly complex topics, and not at all specific to anonymity systems, and so we don't discuss them further.

These distinctions are clearly artificial, and some actual attackers (prototypically, the NSA) are obviously strong in all three areas. Even so, there are realistic examples for each canonical adversary. And in any case, they serve as a useful framework for analysis and discussion.

### Passive Adversaries with Limited Network Reach

For passive and Byzantine adversaries, key resources are network reach to obtain intercepts, data storage, and computing capacity for traffic correlation (and for Byzantine adversaries, modification). For governments, network reach typically depends on legal authority and/or political influence, supplemented through agreements with peers. For non-governmental passive adversaries, such as schools, businesses and ISPs at various levels, ownership and/or management authority typically limits network reach. And for those adversaries with requisite expertise and resources, stealth is always an option.

All low-latency anonymity systems arguably protect against passive adversaries that can access just one end of a connection. That's typically the case for most non-governmental passive adversaries, except for Tier 1 ISPs. Most governments (excepting the NSA and collaborators, such as the Five Eyes) can only see one end of international connections. In such cases, the hardest part is typically penetrating a perimeter firewall. It might be an enterprise firewall, or the Great Firewall (GFW) of China. But without additional intercepts, traffic correlation and modification can't accomplish very much.

While China is obviously a very formidable adversary, its international network reach has apparently remained quite limited. If that assessment is accurate, all low-latency anonymity systems that manage to connect through the GFW will arguably protect users in China for accessing destinations located outside China, with three exceptions. First, all of them are easily broken for destinations that are under Chinese control. Second, all are broken for destinations that are vulnerable to Chinese man-in-the-middle (MitM) attacks, perhaps relying on counterfeit SSL certificates or protocol vulnerabilities. Third, all are more-or-less vulnerable to Sybil attacks, as we discuss below.

The GFW blocks anonymity systems in at least four ways. First, it blocks access to known entry servers. Second, it blocks traffic based on connection protocol, determined from characteristic headers and packet patterns. Third, it probes suspected entry servers, trying to detect anonymity systems by posing as a client. Fourth, as a last resort, it may simply throttle or block all encrypted traffic.

Anonymity systems can evade the GFW (and other firewalls) by encapsulating their traffic in more generic connections routed via proxy servers. Some VPN services offer SSH and/or SSL (stunnel) proxies, and a few use proprietary closed-source transport protocols. The Tor Project has developed an open-source pluggable transport framework, comprising the core [obfsproxy](https://www.torproject.org/projects/obfsproxy.html.en) app, and an evolving series of transport plug-ins. It's a SOCKS5 proxy, and should work with virtually any anonymity system. So far, only iVPN is using it.

However, against resourceful adversaries, obfuscating the transport protocol is just a temporary fix. Once an adversary has identified a proxy server, it can simply block traffic to that IP address. More seriously, the adversary can also readily identify all users connecting to that proxy server. Furthermore, by investigating hosts that those users subsequently connect to, it can readily identify additional proxy servers.

Distributing proxies is a hard problem. Adversaries can enumerate proxies by posing as users, and resourceful adversaries can field numerous malicious users. Tor bridges are distributed in several ways. Volunteers can create bridges, and share addresses on an ad hoc basis. There's a central BridgeDB, which can be accessed by the Tor client, and reached through its website. The BridgeDB can also be queried by email, but only from email addresses that are more-or-less difficult to obtain.

Reputation-based alternatives are under investigation. Bridges could be distributed through various channels, such as individual users, private email lists, or social networks. The reputation of each channel would depend on the overall fate of bridges that had been distributed through it, and new bridges would be distributed through channels in proportion to reputation.

The recently proposed [CloudTransport bridge](https://www.petsymposium.org/2014/papers/Brubaker.pdf) design takes a different approach. It relies on cloud servers, with IP addresses that would change frequently, and would be drawn from large pools shared with mainstream cloud-hosted services. Censors couldn't readily and reliably distinguish bridge users from those using other services on the cloud host. In order to block such bridges, censors would need to block entire ranges of IP addresses, and in doing so, they would also block access to other cloud services.

### Passive Adversaries with International Network Reach

Tor is generally far less vulnerable than are JonDonym or most VPN services to passive adversaries with international network reach. It is far larger, and far less vulnerable to coercion. There are many more simultaneous users, and many more nodes (relays). Relays are distributed globally, in numerous data centers, among many nations, and with no central ownership or management. Furthermore, traffic paths change, frequently and unpredictably. Given that, it is arguably impractical for most adversaries to obtain enough intercepts.

Global passive adversaries would, by definition, have enough intercepts. However, there are typically about two million Tor users, and on the order of several million concurrent circuits. Tracing a particular Tor circuit would entail correlating conversations in one intercept (presumably starting with an exit relay or entry guard) with several million conversations intercepted from at most a few thousand other relays. That would be trivial for a global adversary. However, cross correlating all of the several million concurrent conversations from all Tor relays would involve on the order of 10{{< sup >}}13{{< / sup >}} comparisons, which is arguably not so trivial. In other words, all but the most resourceful global passive adversaries may be computationally bounded. And in any case, as discussed below, Sybil attacks against Tor are far easier.

Against adversaries with enough network reach to observe a given fraction of the system's nodes, JonDonym does resist compromise better than Tor does. That is so because JonDonym mixes distort traffic patterns, whereas Tor relays do not. That distortion hinders correlation of traffic flows captured in different network segments. However, because JonDonym is smaller than Tor, with less geographic diversity, observing substantially all system nodes would require less network reach for JonDonym than for Tor. And once an adversary can see both ends of a connection, in-network mixing becomes relatively useless.

There are but nine JonDonym mix operators, headquartered in six nations. There are 16 mix cascades: six free two-mix cascades, and ten premium three-mix cascades. Each cascade is static, with a fixed entry and exit IP address. The 42 entry and exit mixes (each used in just one cascade) are located in just 17 data centers, in 12 nations. And there are typically about 20-60 users on each of the ten premium cascades, and about 400-500 users for each of the six free cascades.

Against adversaries with limited international network reach, Tor resists compromise better than JonDonym does. That is so for two reasons. First, as noted, observing all system nodes is harder for Tor than for JonDonym. Second, cross correlating user conversations between entry and exit intercepts would involve far less comparisons for JonDonym. Given that there is no mixing among cascades, cross correlating all of JonDonym's 2600-3600 entry and exit conversations would involve only on the order of 10{{< sup >}}7{{< / sup >}} comparisons, which would arguably be trivial for a global adversary, even in real time. Conversely, compromising all Tor conversations would require on the order of 10{{< sup >}}13{{< / sup >}} comparisons.

Most VPN services are even more vulnerable. There are typically 10-100 servers, located in 5-20 data centers, in perhaps as many nations, with about 50 users per server. All servers are typically under common ownership and/or management. Most providers offer only one-hop routes, and so an adversary need only correlate entry and exit conversations on one server. For all but the largest VPN services, cross correlating all entry and exit conversations would involve far less than a million comparisons.

A few large VPN services have several hundred or more servers, with numerous IP addresses per server, located in perhaps more than 100 data centers. But even for the largest, cross correlating all entry and exit conversations would involve at most a few million comparisons. Still, it's possible that some of the largest VPN services offer better anonymity than JonDonym does. It all depends on where entry and exit nodes are located, where an adversary can observe traffic, and how many comparisons among concurrent conversations would be required. However, given common ownership and/or management of VPN services, social engineering, or legal and/or political coercion, would be more-likely approaches.

Some VPN services offer multi-hop routes. For example, there might be three servers (A,B,C) in different countries, with six available two-hop routes (A-B,A-C,B-A,B-C,C-A,C-B). Multi-hop routes can offer better protection against passive adversaries with limited network reach, because all users' traffic transits two or more nations. Also, as the entry and exit servers connect using VPNs, adversaries can't intercept individual user connections between servers. But again, common ownership and/or management is the key vulnerability.

### Sybil Adversaries

For Sybil adversaries, key assets are large server clusters and fast uplinks. That allows them to run numerous malicious clients and/or attractive network nodes, to efficiently analyze collected data, and to exploit what they learn. They are strongest when they own both clients and network nodes of anonymity systems, because they can use them synergetically. There is no requirement for broad network reach, just bandwidth. We conservatively assume that Sybil adversaries are computationally unbounded.

Even with limited organizational support, anyone with the financial resources and expertise to wield large cloud server clusters (such as AWS cluster compute instances) can be a strong Sybil adversary, at least for limited periods of time. Given typical cloud pricing structures, enormous resources are very affordable on limited terms. China is undoubtedly a formidable Sybil adversary, given its immense technical (and human) resources. But other plausible examples range from skilled individuals to small academic research groups to non-government gangs to state-level intelligence agencies (such as the NSA).

### Sybil Adversaries vs VPN Services

Introducing malicious VPN servers is both difficult (because one entity owns and/or manages all of the servers) and immediately fatal to anonymity (because there's usually just one server between users and destinations). Given that, Sybil attacks involving malicious VPN servers amount to coercion, which we do not discuss.

Consider an adversary, with limited network reach, that seeks to deanonymize those using VPN services to access an Internet destination, such as a social networking site, a discussion forum or an IRC channel. While engaging targeted users there, it could carry out distributed denial of service (DDoS) attacks on various VPN servers, perhaps by initiating bogus TLS handshakes from numerous malicious clients. Unless those VPN servers were protected by intervening firewalls that limited the rate of new connections, this would tie up CPU capacity needed for handling traffic of already-connected clients, and might even crash them.

An effective DDoS attack on a particular VPN server would interfere with its users activity, and might even take them offline. Given enough testing, the Sybil adversary would know which VPN server each targeted user was connecting through. Knowing that, the adversary might try to directly compromise the server, or go after the operator and/or hosting provider. Depending on its resources, it might use such approaches as political or legal coercion, spearphishing and social engineering.

For adversaries that can observe traffic at Internet exchange points between users and VPN servers, there may be no need to compromise VPN servers or their operators. Given an effective DDoS attack on the right VPN server, the adversary would see impacts on both a user's online activity and their connection to the server. State-level adversaries are canonically resourceful for such attacks against all low-latency anonymity systems, but especially against VPN services (smallest) and JonDonym (smaller than Tor).

### Sybil Adversaries vs JonDonym

Introducing malicious JonDonym mixes is also difficult. As noted, mix operators must be verified and certified, and there is apparently close oversight by an existing community of trusted mix operators. Also, JonDonym cascades are static, and new operators generally don't join existing cascades, but rather find partners for new ones. Given that, compromising existing mixes or operators would arguably be more efficient. Therefore, we consider Sybil attacks involving malicious JonDonym mixes to be coercion, which we don't discuss.

The six free JonDonym cascades are in theory vulnerable to the attacks discussed above for VPN services. However, given that free cascades typically operate at near maximum capacity, DDoS attacks would ramp up quite slowly. Sybil attacks against the ten premium cascades would be faster, because they typically operate at far below maximum capacity. Although numerous premium accounts would be required, creating them would not be an issue for resourceful adversaries. Perhaps more problematic would be the response of mix operators to a large influx of new premium clients.

### Sybil Adversaries vs Tor

Although Tor is much larger than JonDonym and VPN services, it is an open system, where Sybil adversaries can readily wield both clients and relays. Given that, Tor is arguably more vulnerable to pure Sybil adversaries, which we consider to have very limited network reach and no coercive authority. Indeed, Sybil attacks by academic research groups have apparently compromised substantial percentages of Tor users over several months.

Given that the NSA (or even China) has orders of magnitude more resources, one might expect that Tor is entirely defenseless against them. However, even though Tor is an open system of untrusted relays, entry and behavior of relays are subject to oversight by a core group of trusted developers and relay operators. Also, there is a vetting process for new relays, which seeks to limit disruptive and malicious behavior.

In other words, Sybil attacks on Tor aren't so much limited by an adversary's resources as they are by oversight. While that largely mitigates the resource advantage possessed by nation-state adversaries such as the NSA and China, it does so only for Sybil attacks. There is no such defense against passive network analysis by nation-state adversaries with adequate network reach, because it's not readily detectable.

Consider a pure Sybil adversary, which can wield numerous malicious Tor clients and relays, but lacks other resources. It fields two groups of malicious relays, one targeted for use as entry guards, and the other targeted for use as exit relays. By comparing traffic through circuits handled by member of those groups, it can identify circuits where it provides both an entry guard and an exit relay. That compromises clients, because the adversary knows both their IP address and the Internet destinations that they are accessing.

For malicious entry guards, the strategy involves avoiding the Exit flag by blocking connections to the Internet, and getting the Guard flag by being online continuously for at least eight days. In practice, malicious entry guards would remain online continuously during an attack, so as to maximize their usage. For malicious exit relays, the strategy involves getting the Exit flag by allowing connections to the Internet, and avoiding the Guard flag by being continuously online for periods of a week or less.

An adversary can increase the speed and breadth of this Sybil attack by employing malicious clients in DDoS attacks against honest relays. By attacking honest entry guards, the adversary can gradually push user clients to its malicious entry guards. Similarly, by attacking honest exit relays, the adversary can push user clients to its malicious exit relays.