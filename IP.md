# Internet_Protocol
IP means ? this is a guideline to explain it.

What do we expect from a network? We expect transparent (means no user intervension is required to establish and maintain) communication among devices connected to the network.


                Host (connect with server using SSH or FTP)
                 |
                  \
                   \
                    \ 
                     \
                      \
                       |
                      Network 
                       ||
                        \\
                         \\
                          \\
                           \\
                            \\
                             ||
                            Server

# Network Components Implementation

The network components hereby means hardware and software, they provide the communication for users.

# Network Adapter (Network Tool provided by Vendors)

Inbound to Server

The software we use, such as SSH or Telnet, to know it adapter functionality as below ->

      Telnet Client App format the data that is acceptable 
          Host (connect with server using Telnet)
                 |
                  \
                   \
                    \ the formatted data intends to transmit and send to Server
                     \
                      \
                       |
                      Network 
                       ||
                        \\
                         \\
                          \\
                           \\
                            \\
                             ||
                            Server
  
 Outbound to Server
 
      Data arrives unsyncronized with trasmission of data sent from Server.
       Telnet Client App needs to handle the incoming formatted data.
          Host (connect with server using Telnet)
                 |
                  \
                   \
                    \ the formatted data intends to transmit and send to Client
                     \ 
                      \
                       |
                      Network 
                       ||
                        \\
                         \\
                          \\
                           \\
                            \\
                             ||
                            Server
                            
 It seems the Adapter does many complex task for us:
 
 [1] format data
 
 [2] communicate with network
 
 [3] handle async arrival of data

# Data Formation

To make data understandable by different implementations by different vendors falls into data presentation issues.

# End-to-End Communication Devices

To establish and maintain the connection between the end devices including:

[a] retransmit corrupted data

[b] detrmine which network devices is the next hop

[c] two ends are seperated by the network, which is consist of multi-devices and multi-links.

# Async Data

sort out-of-sequence data

_______________________________________________________________

It is interesting that above mentioned task is unique from the others in functionality, however the tasks depend on each other tightly.

_______________________________________________________________

# PDU, the Protocol Data Unit

Protocol is the rule that defines that the peer entities use to do package exchange with each other.

PDU is

* the rule control the exchange of PDUs.

* the interpretation of info carried in the PDUs.

* the formats of packets that the entities can exchange.

It is convenient to think of a protocol as a language that the peer entities use to communicate each other.

The analog is quite precise: PDUs are similar to words, which we interpret and understand. Also, while constructing sentences, we follow certain grammar as rule, and while speaking, we follow certain rule, such as expecting an answe while we asking question.

The process od encapsulation of PDUs amongs layer of higher and lower levels.


                                                 ________________________________
      Layer n       
                                                  Layer N Header | Layer N Payload
                                                 ________________________________
                    
                    
                               ____________________________________________________
                    
      Layer n-1                Layer N-1 Header |  Layer N-1 Payload
                               ____________________________________________________
                                                 inluding Layer n Packet
                                                 
                 __________________________________________________________________

      Layer n-2  Layer N-2 Header | Layer N-2 Payload
                 __________________________________________________________________
                                   including Layer n-1 Packet
                                   
 A PDU is said to transmitted using a protocol when it is passed between peer entities that use this protocol.
 
 # SAP, Service Access Point
 
 The PDU goes thru the layer no matter the higher or lower shall pass into specific Service (kind of functionality) that peers perform.
 
 The interface called SAP, Service Access Point. And Peer Entities rside on End System.
 
                
                 SAP                                      SAP
                 
               peer entity A    <- Protocol A ->    peer enitity A
               
                                                           A
                  |                                        |
                 PDU           boundary of Layers         PDU
                  |                                        |
                  V                   
                
                peer entity B   <- Protocol B ->     peer entity B
                
                 SAP                                       SAP
 
# OSI, layers seperated due to MTU

such as in this case of Telnet


    APP Layer                   Telnet

    Transport Layer             TCP

    Internet Layer              IP

    Link Layer                  Ethernet

    Phisical Layer
 
to see the flow in detail

    APP Layer                   Telnet
    
                                 |
                                PDU
                                 |

    Transport Layer             TCP
                   
                                 |
                              Segment 區段
                                 |

    Internet Layer              IP
             
                                 |
                              Datagram 封包
                                 |

    Link Layer                Ethernet

                                 |
                               Frame 訊框
                                 |

                              Binary Pulse 二進制脈衝
    Phisical Layer           Network (twisted pair, coaxial cable 和 optical fiber)

# MTU, Min Transmission Unit


          Host A                                        Host B
            |                                             |
             \                                           /
          MTU 1500                                    MTU 1500
               \                                       /
                \_   Router C __ MTU 512 __  Router D_/

There are various data link layer technologies that have different MTU sizes, and that is why network comprises segments with different size (MTU).

# IP datagram

From big MTU to small MTU is called "Fragmentation" process, breaking the data Segments into fragments to match the MTU. And those fragments is called as "IP datagram".

# IP Datagram Header
 
                     8                 16                24              31
     0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 
     |_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|
     
     Version   IHL      --->  ToS <---  ---------> Total Length <---------
      
     ----------->    ID     <----------- ->falg<-  Fragment Offset       ->
     
     -----> TTL <----- --> Protocol <-- --------> Header Checksum <--------
     
     ---------------------------> Src Address <----------------------------
 
     ---------------------------> Des Address <----------------------------
     
     Options | Padding .....................................................
     
     ...
     
# version

it contaions IP version number. (can be IPv4 || IPv6)

# IHL, IP Header Length

the IP header length field is the size of the datagram header.

# ToS, Type of Service

the ToS consists of 

                                     
     0 1 2 3 4 5 6 7 8 
     |_|_|_|_|_|_|_|_|
     
     |     |
     Precedence 
           |       |
               ToS
                    | |
                    MBZ

* precedence 
  indicates the importance of the datagram.
  
        Decimal  Bin   Des

        7        111   Network Control
        6        110   Internetwork Control
        5        101   CRITC/ECP
        4        100   Flash Override
        3        011   Flash
        2        010   Immediate
        1        001   Prority
        0        000   Routine

* ToS
  indicates the desired characteristics of the path along which the datagram shall be sent.
  
         Bin   Des
        1000   Min Delay
        0100   Max Throughput
        0010   Max Reliability
        0001   Min Monetary Cost
        0000   Normal Sevice

* MBZ

# TTL, Time to Live

(in case of the datagram sent to a multicast IP addr, then TTL is called datagram scope.)

TTL field is designed to avoid loop (which led out the congestion network), if TTL is not used, then the IP datagram would end up being forwarded an ultimated number of times.

the time to live reflects that how long the datagram can exist before the intermediate routers drop it as expired.

Routers canculate the TTL field as below:

[1] When a router receives a datagram, it decrements the value of TTL by 1.

[2] If the value is 0, the router drops the datagram; otherwise, it forward the datagram to the next hop router or the final destionation.

[3] if the router is forced to store the datagram in its memory for more than 1 sec, it decrements the value of the TTL field by 1 for each additional second.

# Protocol field, which acts as demultiplex key

      Hex     Decimal      Protocol
       6         6          TCP
      11        17          UDP
       1         1          ICMP
       9         9          IGRP//
      58        88          EIGRP//
      59        89          OSPF
       4         4          IP
      2F        47          GRE//

# Header Checksum

this checksum is for header, not for payload, so once the payload is corrupted, the IP protocol is not able to detect it.

This means thay the Transport Protocol - whose PDU is carried as the payload of an IP datagram (after encapsulation) - shall has its own verification methods for data integrity. (Should the payload arrive corrupted, it is the responsibilty of the transport layer protocol to request retransmission if it considers it neccessary.)

# IP address

https://github.com/QsCake/IP_Addr

