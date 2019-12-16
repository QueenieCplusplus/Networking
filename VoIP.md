# CCDA VoIP & QoS
聊下賴, while calling the VoIP is initialized and take its function ...


This topic covers voice architectures, integrated voice design, and QoS in voice network.

There are 2 architects such as TDM && PSTN, TDM means Time Division Model, and PSTN is the global voice network that provides voice services. The PSTN is a variety of networks and services that are in place worldwide; it provides a circuit-sw service using SS7 (Signaling System 7) for out-of-band (帶外管理) call provisioning thru network.

CO sw exchange SS7 msg to place and route voice call thru network. The PSTN uses TDM facilities for calls. From the CO to the customer premises, the call can be analog, ISDN, or Digital TDM, Modem. And each call consumes 64kbps of bandwidth, which is called DS0 (digital service 0).

# Traddition Call Flow

                                                                        Taiwan
       local loop ground start                                                local loop ground start
      phone -----------  CO                                               CO--------- Fax Machine
                            \                                              |
      phone                  \ E & M                                       |trunk
        \                     \                                            |                                                     
         \ ---- local loop ----- CO/FXS---  Toll --- Toll --- Toll --- CO/FXS ------ Cell                                                           
                           trunk                      trunk

               Australia                                                Canada   

# Ports connects to Voice End Stations (Phone)

    * trunk line using T1/E1 of DSL 

    * trunk line using ISDN

    * E & M (for Private SW between CO to CO) 

    * FXO, Foreign Exchange Office

    * FXS, Foreign Exchange Station

# Call on-Net,using PBXs and their Network

   PBXs are customer owned voice SW. Enterprise companies install and config their own PBXs to provide tele service: voicemail and private routing within other features.
   
   Organizations can reduce toll charges by using private tie line routing!
   

                                     PSTN in Korea

                                            |
    call route to PSTN in Taipei - private Tie Line -  PSTN in USA
                                            |

                                        PSTN in JP

# PSTN has many other features

    * voicemail

    * keep call detail record in DB system

    * centrex

    * IVR, Interactive Voice Response

    * ACD, Automatic Call Distribution

# Packet-Voice Tech = IP Telephony

    [a] VoFR using Frame Relay

    [b] VoATM using Async Transfer Mode

    [c] VoIP 

We will discuss the VoIP only in this Topic.

# VoIP using Multi-Protocols

    [1]DHCP
    [2]DNS
    [3]TFTP
    [4]SSCP, Skinny Station Control Protocol
    [5]CRTP, Streaming in Station-to-Station traffic in ongoing call
    [6]RTCP, for Call Control
    [7]MGCP, Media Gateway Control Protocol
    [8]H.323, handling call from ITU, may support H.225 | H.245 | Q.931
    [9]SIP, feature is like [7], and defined by IETF 

After[5] the datagram will be encapsulated.

      before
      
            IP Header + UDP + RTP + Payload
            
      after
      
            Header      +           Payload
        (small byte size)

# SIP, an alternative to H.323

SIP is a app-layer protocol and multimedia framework defined by the IETF and specified in RFC 2543, Cisco supports SIP protocol on its IP phones and gateways.

It is used to create, modify, terminate the Internet Mutimedia Conferences, Internet Telephone Calls, and Multimedia Distribution.

Communication between members in a session can be via a multicast, a unicast mesh, or combination.

SIP uses many protocols in its architecture:

    [a] RSVP for reserve resources
    [b] RTP for performing real-time and fit the QoS
    [c] RTSP for streaming media delivery
    [d] SAP, Session Announcement Protocol using multicast
    [e] SDP, Session Description Protocol

SIP uses many components in its module:

      [1] User Agent
      [2] Proxy Server
      [3] Redirect Server
      [4] Register Server
      [5] Logical Server
      [6] Back to Back User Agent

# QoS Tool

CRTP

Auto QoS

LLQ

PQ-WFQ

WFQ

LFI

PQ

# Fix Delay Components

    Propagation
    Processing
    Serialize
    De-jitter ??? (will study it later)
