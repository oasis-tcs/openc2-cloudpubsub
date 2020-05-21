
![OASIS Logo](https://docs.oasis-open.org/templates/OASISLogo-v2.0.jpg)
# OASIS Committee Note
-------

# OpenC2 Use of Cloud-Based Pub/Sub Messaging Version 1.0

## Committee Note Draft 01 /<br>Public Review Draft 01

## 22 May 2020

#### Technical Committee:
[OASIS Open Command and Control (OpenC2) TC](https://www.oasis-open.org/committees/openc2/)

#### Chairs:
Joe Brule (jmbrule@nsa.gov), [National Security Agency](https://www.nsa.gov/) \
Duncan Sparrell (duncan@sfractal.com), [sFractal Consulting LLC](http://www.sfractal.com/)

#### Editors:
Robert O'Brien (robert.obrien@omnyon.com), [G2, Inc.](https://www.g2-inc.com/) \
David Lemire (david.lemire@hii-tsd.com), [G2, Inc.](https://www.g2-inc.com/)

#### Related work:
This specification is related to:
*  _Open Command and Control (OpenC2) Specification for Transfer of OpenC2 Messages via HTTPS Version 1.0_. Edited by David Lemire. Latest version: http://docs.oasis-open.org/openc2/open-impl-https/v1.0/open-impl-https-v1.0.html.

#### Abstract:
Open Command and Control (OpenC2) is a concise and extensible language to enable the command and control of cyber defense components, subsystems and/or systems in a manner that is agnostic of the underlying products, technologies, transport mechanisms or other aspects of the implementation. Google Cloud Platform (GCP) Pub/Sub (i.e., publish / subscribe) is an asynchronous communication method that enables messages to be exchanged without knowing the identity of the sender or recipient. This document describes the use of GCP Pub/Sub as a transfer mechanism for OpenC2 messages, and captures experience and lessons learned from utilizing GCP Pub/Sub at the January 2020 OpenC2 Plug Fest. The information contained within this document can be applied when using the GCP Pub/Sub implementation for conveying OpenC2 command and response messages, and may also be useful with other publish / subscribe protocol implementations.

#### Status:
This is a Non-Standards Track Work Product. The patent provisions of the OASIS IPR Policy do not apply.

This document was last revised or approved by the OASIS Open Command and Control (OpenC2) TC on the above date. The level of approval is also listed above. Check the "Latest version" location noted above for possible later revisions of this document. Any other numbered Versions and other technical work produced by the Technical Committee (TC) are listed at https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=openc2#technical.

TC members should send comments on this document to the TC's email list. Others should send comments to the TC's public comment list, after subscribing to it by following the instructions at the "Send A Comment" button on the TC's web page at https://www.oasis-open.org/committees/openc2/.

#### URI patterns:
Initial publication URI:  
https://docs.oasis-open.org/openc2/cloudpubsub/v1.0/cnd01/cloudpubsub-v1.0-cnd01.html

Permanent "Latest version" URI:  
https://docs.oasis-open.org/openc2/cloudpubsub/v1.0/cloudpubsub-v1.0.html


#### Citation format:
When referencing this document the following citation format should be used:

**[Cloud-Pub/Sub-v1.0]**

_OpenC2 Use of Cloud-Based Pub/Sub Messaging Version 1.0_. Edited by Robert O'Brien and David Lemire. 09 April 2020. OASIS Committee Note Draft 01 / Public Review Draft 01. https://docs.oasis-open.org/openc2/cloudpubsub/v1.0/cnprd01/cloudpubsub-v1.0-cnprd01.html. Latest version: https://docs.oasis-open.org/openc2/cloudpubsub/v1.0/cloudpubsub-v1.0.html.

-------

## Notices
Copyright © OASIS Open 2020. All Rights Reserved.

All capitalized terms in the following text have the meanings assigned to them in the OASIS Intellectual Property Rights Policy (the "OASIS IPR Policy"). The full [Policy](https://www.oasis-open.org/policies-guidelines/ipr) may be found at the OASIS website.

This document and translations of it may be copied and furnished to others, and derivative works that comment on or otherwise explain it or assist in its implementation may be prepared, copied, published, and distributed, in whole or in part, without restriction of any kind, provided that the above copyright notice and this section are included on all such copies and derivative works. However, this document itself may not be modified in any way, including by removing the copyright notice or references to OASIS, except as needed for the purpose of developing any document or deliverable produced by an OASIS Technical Committee (in which case the rules applicable to copyrights, as set forth in the OASIS IPR Policy, must be followed) or as required to translate it into languages other than English.

The limited permissions granted above are perpetual and will not be revoked by OASIS or its successors or assigns.

This document and the information contained herein is provided on an "AS IS" basis and OASIS DISCLAIMS ALL WARRANTIES, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO ANY WARRANTY THAT THE USE OF THE INFORMATION HEREIN WILL NOT INFRINGE ANY OWNERSHIP RIGHTS OR ANY IMPLIED WARRANTIES OF MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE.

-------

# Table of Contents
-   [1 Introduction](#1-introduction)
    -   [1.1 Terminology](#11-terminology)
    -   [1.2 References](#12-references)
    -   [1.3 Pub/Sub message flow](#13-pubsub-message-flow)
        -   [1.3.1 Figures and Captions](#131-figures-and-captions)
    -   [1.4 Categories of Projects](#14-categories-of-projects)
-   [2 Experience and Lessons
    Learned](#2-experience-and-lessons-learned)
    -   [2.1 Lessons Learned Using Pub/Sub
        Transport](#21-lessons-learned-using-pubsub-transport)
    -   [2.2 Lessons Learned on Blinky Light
        Project](#22-lessons-learned-on-blinky-light-project)
    -   [2.3 Lessons Learned on Cisco IOS XE Router Firewall
        Project](#23-lessons-learned-on-cisco-ios-xe-router-firewall-project)
    -   [2.4 Lessons Learned on HaHa Actuator on Raspberry Pi
        Project](#24-lessons-learned-on-haha-actuator-on-raspberry-pi-project)
    -   [2.5 Lessons Learned on Dynamic Recognition of Actuator
        Capabilities
        Project](#25-lessons-learned-on-dynamic-recognition-of-actuator-capabilities-project)
    -   [2.6 Lessons Learned on Erlang-based Command Generator
        Project](#26-lessons-learned-on-erlang-based-command-generator-project)
    -   [2.7 Lessons Learned on Active Cyber Defense of Critical
        Infrastructure (ACDCI)
        Project](#27-lessons-learned-on-active-cyber-defense-of-critical-infrastructure-acdci-project)
    -   [2.8 Lessons Learned on the Multiple Firewalls
        Project](#28-lessons-learned-on-the-multiple-firewalls-project)
-   [Appendix A. Acknowledgments](#appendix-a-acknowledgments)
-   [Appendix B. Revision History](#appendix-b-revision-history)

-------

# 1 Introduction

This document describes the use of GCP Pub/Sub as a transfer
mechanism for OpenC2 messages.  The January 2020 OpenC2 Plug
Fest included several categories of implementations using
GCP Pub/Sub as a transfer mechanism for OpenC2 messages.  

A publisher sends an OpenC2 message to a topic. The topic
forwards the OpenC2 message to the subscriber. Topics are
used for OpenC2 producers to send OpenC2 commands to a
topic.  The OpenC2 consumers subscribe to the topic to
receive the OpenC2 command. Topics are used for OpenC2
consumers to send OpenC2 responses to a topic.  The OpenC2
producers subscribe to the topic to receive the OpenC2
responses.   



## 1.1 Terminology

## 1.2 References

###### [OpenC2-Lang-v1.0]
_Open Command and Control (OpenC2) Language Specification Version 1.0_. Edited by Jason Romano and Duncan Sparrell. Latest version: http://docs.oasis-open.org/openc2/oc2ls/v1.0/oc2ls-v1.0.html.

###### [OpenC2-HTTPS-v1.0]
_Specification for Transfer of OpenC2 Messages via HTTPS Version 1.0_. Edited by David Lemire. Latest version: http://docs.oasis-open.org/openc2/open-impl-https/v1.0/open-impl-https-v1.0.html

###### [OpenC2-SLPF-v1.0]
_Open Command and Control (OpenC2) Profile for Stateless Packet Filtering Version 1.0_. Edited by Joe Brule, Duncan Sparrell, and Alex Everett. Latest version: http://docs.oasis-open.org/openc2/oc2slpf/v1.0/oc2slpf-v1.0.html

## 1.3 Pub/Sub message flow

The following is an overview of the components in the Publish-subscribe (pub/sub) system and how OpenC2 messages flow between them:

A OpenC2 publisher application generates a topic in the Pub/Sub service and sends OpenC2 messages to the topic. This OpenC2 message contains a payload and optional attributes that describe the data contained within the payload.  The service enables the published OpenC2 messages to be retained for the subscribers.  The OpenC2 published message is retained within a subscription until it is acknowledged by any OpenC2 subscriber consuming messages from that subscription.  Pub/Sub forwards OpenC2 messages from a topic to all of its OpenC2 subscriptions.  An OpenC2 subscriber receives its messages by Pub/Sub pushing them to the OpenC2 subscriber endpoint or by the OpenC2 subscriber pulling the messages from the service.  The OpenC2 subscriber transmits an acknowledgement to the Pub/Sub service as a receipt for each received OpenC2 message.  The service removes OpenC2 messages from the OpenC2 subscription's message queue upon receipt of the acknowledgement from the OpenC2 subscriber.

Publish-subscribe (commonly referred to as "pub-sub") messaging moves information from OpenC2 producers to OpenC2 consumers.  The pub-sub can also move messages from  OpenC2 consumers to OpenC2 producers.  Publish-subscribe messaging allows multiple OpenC2 consumers to receive each OpenC2 message in a topic. Pub-sub messaging enables each OpenC2 consumer to receive OpenC2 messages in a topic in the exact order in which they were received by the messaging system.  This form of transport enables the OpenC2 transport mechanism to be one-to-many (fan-out), many-to-one (fan-in), and many-to-many.




## 1.4 Categories of Projects

These are the Plug Fest use cases as described in the Plug Fest literature.

Category: Internet of Things 

IoT Hardware Board WS-2811LED Flashing Lights using Raspberry Pi 

The Google Pub/Sub transport mechanism was utilized to facilitate the transmission of OpenC2 messages from OpenC2 orchestrators to OpenC2 actuators.  This Blinky Light project demonstrated the ability to send OpenC2 commands from an Orchestrator to an OpenC2 Actuator using a Raspberry Pi device to perform a sequence of flashing of lights on a hardware board with WS-2811 LED Flashing Lights. GCP Pub/Sub was utilized to send Open2 messages between the orchestrator and actuator.    

Category: Unmanned Platforms (RF sensors) 

Active Cyber Defense of Critical Infrastructure (ACDCI) (Clarity Cyber / 
NSA OpenC2 Team)
Description: This project demonstrates the ability to send OpenC2 commands from a python-based OpenC2 Producer to an OpenC2 Consumer consisting of a series of nine actuators. It is intended to model an implementation of a system to defend a geographic area from hostile drone intrusions, but it is adaptable to other scenarios. The ACDCI used the Google Kubernetes Environment (GKE) hosted within the Google Cloud Platform (GCP). The ACDCI project utilized the GCP Pub/Sub transport mechanism to successfully interwork with the actuators hosted on the simulator.
 
Category: Orchestrator Dynamic Recognition of Actuator

The GCP Pub/Sub transport mechanism helped to facilitate the communication of OpenC2 messages between the OpenC2 Producer called Dynamic Recognition of Actuator and the actuators located within the Denver Airport Demo and the Blinking Lights Demo.  
The two OpenC2 consumers Denver Airport Demo and the Blinking Light Demo both were hosted within the Google Cloud Platform (GCP).  Both OpenC2 Consumers utilized the GCP Pub/Sub transport mechanism to successfully interwork with the OpenC2 Producer Dynamic Recognition of Actuator.  

Category: Orchestrator Erlang-based Command Generator (NineFX)

The GCP Pub/Sub transport mechanism helped to facilitate the communication of OpenC2 messages between the OpenC2 Producer called Erlang-based Command Generator and the Blinky IoT using the Google Cloud Platform (GCP).

Category: Orchestrator  Endpoint File Blacklist and Device Quarantine

Project: Endpoint File Blacklist and Device Quarantine (Symantec ICDx)
Description: ICDx stands for "Integrated Cyber Defense Exchange" and is 
our product that integrates all of the other Symantec products together. 
OpenC2 is a key part of that. We have branded our OpenC2 support in ICDx 
as an "Action Adapter" service, since it transparently adapts a standard 
OpenC2 Command (the "action") into whatever API the backend Symantec 
Product actually requires. As such, ICDx allows us to support a single 
API (OpenC2-based) that can be sent to multiple Symantec Products able to process the command in parallel, and aggregate the multiple Responses 
back to the caller. All while preserving the HTTPS compliance required 
by the OpenC2 1.0 specification.

At Plug Fest / described: The ICDx OpenC2 Lab that was available at the Plug Fest consists of three machines running on GCP:
An ICDx 1.3.1 server. For our use, it is an OpenC2 Consumer (which also supports the Orchestration to multiple Symantec Products when multiple Action Adapters have been configured).  A Symantec Endpoint Protection Manager 14.2 server. This is Symantec's premium endpoint security management solution for Enterprises. We call it "SEPM" for short. It implements a robust API that is not at all OpenC2 compliant. Our OpenC2 Action Adapter for SEPM makes it OpenC2-compliant.  A Windows computer (as a "client" to SEPM; what OpenC2 calls a "device") that we can put into Quarantine using an OpenC2 "contain/device" command. It's got a Web Server, which will go offline once the Quarantine command takes effect.

Category: Firewalls

The University of Oslo Firewall project hosted a Cisco Cloud Router in the GCP and sent OpenC2 commands to the firewall using an adapter to command the firewall of the cisco router. The Cisco IOS XE router used by the University Oslo was interworked using a OpenDXL broker hosted within the Google Cloud Platform.  The OpenDXL hosted in Google Cloud Platform utilized GCP pub/sub to issue OpenC2 commands to the Cisco IOS XE router. 

The AT&T Firewall project utilized a DXL Broker hosted within the Google Cloud Platform (GCP) to enable an OpenC2 Producer to send OpenC2 commands to three cloud infrastructures-Amazon Web Services (AWS), Azure and Google Cloud Platform.  
The DXL broker utilized the GCP Pub/Sub enterprise message-oriented middleware to send OpenC2 commands to an OpenC2 Consumer hosted in the AWS platform.  These OpenC2 commands triggered an OpenC2 actuator which changed the configuration of the AWS firewall.  The DXL broker utilized the GCP Pub/Sub enterprise message-oriented middleware to send OpenC2 commands to an OpenC2 Consumer hosted in the Azure platform.  These OpenC2 commands triggered an OpenC2 actuator which changed the configuration of the Azure firewall. The DXL broker utilized the GCP Pub/Sub enterprise message-oriented middleware to send OpenC2 commands to an OpenC2 Consumer hosted in the GCP platform.  These OpenC2 commands triggered an OpenC2 actuator which changed the configuration of the GCP firewall.

The OpenC2 actuator hosted in the AWS platform utilized the GCP Pub/Sub enterprise message-oriented middleware to send OpenC2 responses to an OpenC2 Producer hosted in the GCP platform.  These OpenC2 responses contained a status of the firewall configuration changes made by the OpenC2 actuator to the AWS firewall.  The OpenC2 actuator hosted in the Azure platform utilized the GCP Pub/Sub enterprise message-oriented middleware to send OpenC2 responses to an OpenC2 Producer hosted in the GCP platform.  These OpenC2 responses contained a status of the firewall configuration changes made by the OpenC2 actuator to the Azure firewall.  The OpenC2 actuator hosted in the GCP platform  utilized the GCP Pub/Sub enterprise message-oriented middleware to send OpenC2 responses to an OpenC2 Producer hosted in the GCP platform.  These OpenC2 responses contained a status of the firewall configuration changes made by the OpenC2 actuator to the GCP firewall.


Add horizontal rule lines where page breaks are desired in the PDF - before each major section 
- insert the line rules in markdown by inserting 3 or more hyphens on a line by themselves:  ---
- place these before each main section in markdown (usually "#" - which generates the HTML `<h1>` tag)

-------

# 2 Experience and Lessons Learned

Experience and lessons learned from utilizing the Google Cloud Platform Pub/Sub at the January 2020 OpenC2 Plug Fest are described in greater detail in the following sections.

## 2.1 Lessons Learned Using Pub/Sub Transport

The Google Pub/Sub was utilized to facilitate the transmission of OpenC2 messages from OpenC2 orchestrators to OpenC2 actuators.  The dry run held at the UMBC Training facility was very beneficial and was part of the reason the plug fest was described as a "remarkably successful" event showcasing the OpenC2 language.



## 2.2 Lessons Learned on Blinky Light Project

The Google Cloud Platform (GCP) Pub/Sub was needed in order to facilitate the transmission of OpenC2 Commands and Responses between the OpenC2 orchestrator and the Blinky Light actuator.The Blinky Light project demonstrated the ability to send OpenC2 commands from an Orchestrator to an OpenC2 Actuator using a Raspberry Pi device to perform a flashing of lights on a hardware board with WS-2811 LED Flashing Lights. 

Summary of the results from plugest are listed below:

Presented at Plug Fest: This project was presented at the plug fest.
Initial State: No lights flashing
Final State: Multiple producers successfully commanded the device
Releaseability: open source
Demonstrated? Yes
Interworked? Yes

## 2.3 Lessons Learned on Cisco IOS XE Router Firewall Project    

The GCP Pub/Sub was needed in order to facilitate the transmission of OpenC2 Commands and Responses between the OpenC2 Producer located in the GCP and the OpenC2 Consumer modifying the Access Control Lists of the Cisco IOS XE router.    The University of Oslo Firewall project hosted a Cisco Cloud Router in the GCP and utilized an OpenC2 Producer to send OpenC2 commands to the firewall using an adapter to command the firewall of the cisco router.   The Cisco IOS XE router used by the University Oslo was interworked with and OpenC2 Producer using a OpenDXL broker hosted within the Google Cloud Platform to the AT&T Cloud.  The OpenC2 Producer hosted in the Google Cloud Platform used an OpenDXL broker and GCP pub/sub to issue OpenC2 commands to modify the Access Control Lists (ACL) of the Cisco IOS XE router. The Cisco IOS XE Router utilized GCP pub/sub to send OpenC2 responses back to the OpenC2 producer.


## 2.4 Lessons Learned on HaHa Actuator on Raspberry Pi Project 
  
When interworking the OpenC2 producer from NineFX recommend using the Google Cloud Platform (GCP) Pub/Sub transport mechanism to send OpenC2 messages to the OpenC2 Consumer HaHa Blinky actuator using Service Account certificates provided within the GCP environment.

The HaHa Actuator on Raspberry Pi project is based on the HaHa elixir open source code provided by Sfractal Consulting. It consists of a HaHa/SBOM/Blinky actuator running on laptop interfacing with a Raspberry Pi Zero and a Raspberry Pi 4.  Accessing from HII OIF did not work due to header issues. Accessing from NineFX code worked if all the cert checks were disabled.  All software used was open source.  

Summary of the results from plugest are listed below:

Demonstrated - yes
Interworked:
Yes via HTTP (non-compliant with OC2 Transport Spec) using Postman
Yes via HTTPS using postman (probably not OC2-compliant, not clear on header issues)
Sort of Yes from NineFX code (custom response header issues on sFractal side)
No from HII OIF

## 2.5 Lessons Learned on  Dynamic Recognition of Actuator Capabilities Project

For the two OpenC2 consumers Denver Airport Demo and the Blinking Lights Demo, both OpenC2 Consumers utilized the GCP Pub/Sub transport mechanism and were able to successfully interwork with the OpenC2 Producer Dynamic Recognition of Actuator.  The lesson learned from this is that the GCP Pub/Sub transport mechanism helped to facilitate the communication of OpenC2 messages between the OpenC2 Producer called Dynamic Recognition of Actuator Capabilities and the actuators located within the Denver Airport Demo and the Blinking Lights Demo.

The Dynamic Recognition of Actuator Capabilities project was provided by UK Mod / CACI UK.  The project was an OpenC2 application for connecting to actuators. This  use case provided a GUI which enabled a user to execute a course of action.  The OpenC2-based application automatically discovered the actuator's capabilities, and then built and executed an OpenC2 command. The project used query features to build the profile via the GUI as form elements.

Summary of the results from plugest are listed below:

State before Plug Fest
Incomplete implementation for discovering actuators and executing actions
State at end of Plug Fest
Still not complete but much closer, could discover actuators, configure a profile and then execute actions against a target using 3rd party actuators
Not open sourced but will be soon.
Demonstrated - yes (slides)
Interworked:
Yes via HTTP to 3 actuators (Bluevector, Denver Airport Demo and Blinking Lights)

## 2.6 Lessons Learned on Erlang-based Command Generator Project

The OpenC2Producer NineFx Command Generator was able to successfully interwork with
the OpenC2 consumer Blinking Light Demo utilized the GCP Pub/Sub transport mechanism.  The lesson learned from this is that the GCP Pub/Sub transport mechanism helped to facilitate the communication of OpenC2 messages between the OpenC2 Producer called NineFx Command Generator and the actuators located within the Blinking Lights Demo.  

Summary of the results from plugest are listed below:

Project: Erlang-based Command Generator (NineFX)
Description: BEAM (Erlang/Elixir) OpenC2 SDK

State:
A set of libaries to facilitate OpenC2 producer actions.
Initial State: Proof of Concept
Releaseability: open source
Demonstrated: Yes
Interworked: NEC's SPLF firewall over HTTPS with client certificate authentication, 
HII Blinky IoT over Google Cloud Platform (GCP)


## 2.7 Lessons Learned on Active Cyber Defense of Critical Infrastructure (ACDCI) Project

The ACDCI used the Google Kubernetes Engine (GKE) ans was hosted within the Google Cloud Platform (GCP).  The ACDCI utilized the GCP Pub/Sub transport mechanism to successfully interwork with the actuators hosted on the simulator.  The lesson learned from the ACDCI project is that the GCP Pub/Sub transport mechanism helped to facilitate the communication of OpenC2 messages between the OpenC2 Producer and the actuators located within the ACDCI simulator.  

The Active Cyber Defense of Critical Infrastructure (ACDCI) was provided by the Clarity Cyber team. This project demonstrates the ability to send OpenC2 commands from a python-based OpenC2 Producer to an OpenC2 Consumer to a series of nine actuators. It is intended to model an implementation of a system to defend a geographic area from hostile drone intrusions, but is adaptable to other scenarios.

Summary of the results from plugest are listed below:

"Active Cyber Defense of Critical Infrastructure (ACDCI) Use Case.
Initial / Final State: ACDCI actuators received and processed commands from multiple OpenC2 Producers
Releaseability: open source
Demonstrated: Yes
Interworked: Yes
  
## 2.8 Lessons Learned on the Multiple Firewalls Project

The lesson learned from the Multiple Firewalls project is that the GCP Pub/Sub transport mechanism helped to facilitate the communication of OpenC2 messages to AWS, GCP, Azure firewalls and/or network Access Control Lists (ACL).  The multiple firewalls project provided by AT&T was implemented using the Google Cloud Platform (GCP).   This project utilized GCP pub/sub to issue OpenC2 commands to AWS, GCP, Azure firewalls and/or network Access Control Lists (ACL).  An OpenDXL broker was hosted within the Google Cloud Platform.   The OpenDXL broker used GCP pub/sub to issue OpenC2 commands to AWS, GCP, Azure firewalls and/or network ACLs.  This OpenC2 project incorporated a request-id in the OpenC2 messages sent in the topic. The request-id provided traceability from the OpenC2 Command from the OpenC2 Producer down to the OpenC2 Consumer.


# Appendix A. Acknowledgments

(Note: A Work Product approved by the TC must include a list of people who participated in the development of the Work Product. This is generally done by collecting the list of names in this appendix. This list shall be initially compiled by the Chair, and any Member of the TC may add or remove their names from the list by request.  
Remove this note before submitting for publication.)

The following individuals have participated in the creation of this specification and are gratefully acknowledged:

**OpenC2 TC Members:**

| First Name | Last Name | Company |
| :--- | :--- | :--- |
TBS

-------

# Appendix B. Revision History
| Revision | Date | Editor | Changes Made |
| :--- | :--- | :--- | :--- |
| cloudpubsub-v1.0-wd01 | 2020-05-21 | Robert O'Brien | Initial working draft |

