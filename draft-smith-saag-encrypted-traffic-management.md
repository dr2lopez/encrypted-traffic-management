---
title: Network management of encrypted traffic
abbrev: encrypted-traffic-management
docname: draft-smith-saag-encrypted-traffic-management-latest
date: 2015-04-17
category: info

ipr: trust200902
area: Security
workgroup:
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: K. Smith
    name: Kevin Smith
    organization: Vodafone Group
    email: kevin.smith[ at ]vodafone.com

normative:
  RFC7258:
  RFC2818:

informative:
  mm-effect-encrypt: 
    target: https://datatracker.ietf.org/doc/draft-mm-wg-effect-encrypt/ 
    title: Effect of Ubiquitous Encryption
    author: 
      name: K. Moriarty, A. Morton 
      org: IETF 
      date: 2014-11-14
  
  IAB: 
    target: https://www.iab.org/2014/11/14/iab-statement-on-internet-confidentiality/ 
    title: IAB statement on Internet confidentiality 
    author: 
      name: Internet Architecture Board 
      org: IAB 
      date: 2014-11-14
  
  TAG: 
    target: https://w3ctag.github.io/web-https/ 
    title: Securing the Web 
    author: 
      name: W3C TAG 
      org: W3C	 
      date: 2015-01-15
  
--- abstract

Encrypted Internet traffic may pose traffic management challenges to network operators. This document recommends approaches to help manage encrypted traffic, without breaching user privacy or security.

--- middle

Introduction        {#intro}
============
Networks utilise various management techniques to ensure efficient throughput, congestion management, anti-SPAM and security measures. Historically these functions have utilised visibility of the Internet application layer.

This visibility is rapidly diminishing - encrypted Internet traffic is expected to continue its upward trend, driven by increased privacy
awareness, uptake by major players and advocacy from the {{IAB}},  {{RFC7258}} and W3C {{TAG}}. 

{{IAB}}, {{RFC7258}} and {{mm-effect-encrypt}} recognise that network management functions are impacted by encryption, and that solutions are needed to persist them - as long as they do not threaten privacy. Such solutions would ensure the benefits of encryption do not degrade network efficiency. 


Document structure   {#struc}
----------------------------
This document describes the network management functions that are likely to be hindered by traffic encryption.

It then describes the technical details of existing options to fully or partially persist these functions
under encryption. 'Encryption' in this document typically refers to HTTP over TLS {{RFC2818}}; other forms of encryption are noted where applicable.

Finally a summary is provided of ongoing IETF work which is investigating how middleboxes along the network path can improve encrypted traffic delivery.

The legal, political and commercial aspects of network management are not covered in this technical document.


Network management functions {#netman}
============================

Queuing {#qu}
------------

Traffic flowing through a network may be queued according to the latency and bandwidth required to deliver it. 
This is important at an access network, especially so if the network conditions can change rapidly. 

TO BE COMPLETED


Routing {#rou}
--------------



Intrusion detection {#id}
------------------------

Policy enforcement {#pe}
-----------------------


SPAM and malware filtering {#spam}
---------------------------------

Encryption protocols and their effect {#encryp}
==============================================




Flow information  {#flow}
========================


TLS Server Name Indication {#sni}
--------------------------------

When initiating the TLS handshake, the Client may provide an extension field (server_name)
which indicates the server to which it is attempting a secure connection.
TLS SNI was standardized in 2003 to enable servers to present the "correct TLS certificate"
to clients in a deployment of multiple virtual servers hosted by the same server infrastructure
and IP-address. Although this is an optional extension, it is today supported by all modern
browsers, web servers and developer libraries. Notable exceptions are Android 2.2 and
Internet Explorer 6 on Windows XP.
It should be noted that HTTP/2 introduces the Alt-SVC method for upgrading the connection
from HTTP/1 to either unencrypted or encrypted HTTP/2. If the initial HTTP/1 request is
unencrypted, the destination alternate service name can be identified before the
communication is potentially upgraded to encrypted HTTP/2 transport.
HTTP/2 implementations MUST support the Server Name Indication (SNI) extension.

Limitiation: This solution only works if the browser is populating the Server Name
Indication extension. This need not be done, but may be done as per TLS standard.
Therefore, even if existing network filters look out for seeing a Server Name Indication
extension, they may not find one. The per-domain nature of SNI may not reveal the specific
service or media type being accessed, especially where the domain is of a provider offering
a range of email, video, Web pages etc. For example, certain Facebook or Twitter feeds may
be deemed ‘adult content’, but the Server Name Indication will only indicate the server
domain (facebook.com, twitter.com) rather than a URL path to be blocked


Application Layer Protocol Negotiation (ALPN) {#alpn}
----------------------------------------------------

ALPN is a TLS extenion which may be used to indicate the application protocol within the TLS 
session. THis is likely to be of more value to the network where it indicates a protocol dedicated 
to a particular traffic type (such as video streaming) rather than a multi-use protocol.
ALPN is used as part of HTTP/2 'h2', but will not indicate the traffic types which may make up
streams within an HTTP/2 multiplex.

DiffServ Code Points (DSCP) {#dscp}
----------------------------------

Data packets are flagged with a traffic class (class of service). Network
operators may honour a DiffServ classification entering their network, or may choose to
override it (since it is potentially open to abuse by a service provider that classifies all its
content as high priority). The purpose is to help manage traffic and congestion in the
network.

Limitations:  This requires the content provider to flag data packets. This is extra work for
the provider, and it has potential for abuse if a content provider simply flags all packets with
high priorities. The network would need to know which flags to trust and which to override.
The use of DiffServ within the operator network is beneficial where the operator determines
the class of service itself; but where content is encrypted then heuristics would be needed to
predict the traffic type entering the network.
HTTP/2 allows several streams to be multiplexed over a single TCP connection. This means
that if a provider decides to send Web pages, videos, chat etc. as individual streams over
the same connection, then DiffServ would be useless as it would apply to the TCP/IP
connection as a whole. However it may be more efficient for such Web providers to serve
each content type from separate, dedicated servers – this will become clearer as HTTP/2
deployments are tuned for optimal delivery.


Explicit Congestion Notification {#ecs}
--------------------------------------
Explicit Congestion Notification (ECN) routers can exchange congestion
notification headers to ECN compliant endpoints. This is in preference to inferring congestion
from dropped packets (e.g. in TCP). The purpose is to help manage traffic and congestion in
the network.

This solution is required to be implemented at network and service provider. The
service provider will utilise the ECN to reduce throughput until it is notified that congestion
has eased.

Limitation: As with DiffServ, operators may not trust an external entity to mark packets in a
fair/consistent manner. Relative cost to implement: high (without existing ECN infrastructure), 
medium (with existing ECN infrastructure). Also requires content provider support0.

 
Multi Protocol Label Switching {#mpls}
-------------------------------------

Description: on entering an MPLS-compliant network, IP packets are flagged with a
’Forward Equivalence Class’ (FEC). This allows the network to make packet-forwarding
decisions according to their latency requirements. MPLS routers within the network parse
and act upon the FEC value. The FEC is set according to the source IP address and port.
The purpose is to help managing traffic and congestion in the network.

Requires deployment of an MPLS ‘backbone’ with label-aware switches/ routers. Complexity
will depend upon the existing network topology.

Limitations: some entity has to maintain an up-to-date correspondence table between a
website to block (parental controls and regulatory filtering use cases) and the IP address of a
server. Then, this category of traffic has to be consistently mapped to a certain known set of
MPLS labels which entails a great cost of setting it up and a recurring cost of maintaining it

Note: MPLS can specify how OSI Layer 3 (IP layer) traffic can be routed over Layer 2 (Data Link);
DiffServ only operates over Layer 3. DiffServ is potentially a less complex integration as it is
applied at the network edge servers only.


Inferred flow information {#inflow}
===================================


Heuristics {#heur}
-----------------

All IP packets are associated with meta data that can be utilized for identification and
classification of traffic. The data available are: Source IP-address, destination IP-address,
protocol (TCP, UDP), source port and destination port (collectively known as 5-tuple). In
many scenarios, this makes it possible to directly classify the traffic related to a specific
server/service even when the traffic is fully encrypted.

If the server/service is co-located on an infrastructure with other services that shares the
same IP-address, the encrypted traffic cannot be directly classified. However, commercial
traffic classifiers today typically apply heuristic methods, using traffic pattern matching 
algorithms to be able to identify the traffic. As an example, classifier products are able to 
identify popular VoIP services using heuristic methods although the traffic is encrypted and
mostly peer-to-peer.

Heuristics can be used to map given input data to particular conclusions via some heuristic
reasoning. Examples of this input data include IP destination address, TCP destination port, 
server name from SNI, typical traffic pattern (e.g. occurrence of IP packets and TCP segments over time).
The accuracy of heuristics depends on whether the observed traffic originates from a source 
delivering a single service, or a blend of services.


IP 5-tuple {#ip5}
----------------

This indicates source and destination IP addresses/ports and the transport protocol. This information 
is available during TLS, TCP-layer encryption (except ports), and IP-layer encryption (IPSec); although it may be
obscured in Tunnel mode IPSec. 

This allows network management at a coarse IP-source level, which makes it of limited value where the
origin server supports a blend of service types. 


Experimental protocols {#exp}
=============================

Substrate Protocol for User Datagrams (SPUD) {#spud}
----------------------------------------------------

Ref to SPUD here

Mobile throughput Guidance {#mtg}
---------------------------------

Ref to MTG here


Acknowledgements {#ack}
======================

IANA Considerations {#iana}
==========================

Security Considerations {#sec}
=============================



