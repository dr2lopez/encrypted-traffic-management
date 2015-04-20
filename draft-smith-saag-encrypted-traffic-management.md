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
    email: kevin.smith@vodafone.com

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
-------
Routing {#rou}
-------
Intrusion detection {#id}
-------------------
Policy enforcement {#pe}
------------------

SPAM and malware filtering {#spam}
--------------------------

Encryption protocols and their effect {#encryp}
=====================================



Flow information  {#flow}
================

TLS Server Name Indication {#sni}
--------------------------

Application Layer Protocol Negotiation (ALPN) {#alpn}
---------------------------------------------

DiffServ Code Points (DSCP) {#dscp}
---------------------------

Explicit Congestion Notification {#dscp}
--------------------------------
 
Multi Protocol Label Switching {#mpls}
------------------------------

IP 5-tuple {#ip5}
----------

Internet Control Message Protocol
---------------------------------

Inferred flow information {#inflow}
=========================

Heuristics {#heur}
----------


Notable research work items {#research}
===========================

Acknowledgements {#ack}
================

IANA Considerations {#iana}
===================

Security Considerations {#sec}
=======================



