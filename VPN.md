# CCDA_VPN_DSL
虛擬私有網路與數位用戶迴路

# VPN

Enterprise customers shall consider the internet, with the use of the VPN tech, as a means to connect coporate locations when bandwidth guarantees are not required to support critical traffic and as an affordable backup circuits.

VPNs create private tunnels accross the Internet. It can be used in creating these tunnels from a single host to a VPN concentrator, or you can create site-to-site tunnels between offices.

              __ __ __ VPN Tunnel

              office Router P \
                               \
                                \
                                 \
                                  \
                                   \ 
                                    \
    office Router K __ __ __ __ __ __ Router      Host (Head Quarter)    
                                    /
                                   /
                                  /
                                 /
                                /
                               /
              office Router Q /

There are 6 tech methods to create VPN:
  
    * PPTP
    * IPsec
    * GRE
    * MPLS
    * VPDN
    * MPPE (Microsoft)

And we will talk about PPTP & MPPE & GRE & IPsec this 3 methods in this Topic.

# PPTP

       PPTP is described in RFC 2637. It is a network protocol developed by a vendor consortium for transffering data from PCs to enterprise servers using tunneled PPP thru IP network.

       PPTP client software is deployed in windows platform, Cisco added support for Cisco Routes, PIX Firewalls, and VPN concentrators.
       
                                                             ///(avoid text hidden while sliding)
   
# MPPE

    MPPE converts PPP packets into a encrypeted form. It is used for creating VPNs over dial-up networks, it is part of PPTP client VPN solution.

                                                             ///(avoid text hidden while sliding)
                                                             
# GRE

      GRE is described in RFC 1701, it is Cisco tunneling protocol that encapsuates entire packets into new IP headers, creating a virtual PTP (point-to-point) link between 2 Cisco Routers, GRE was created to tunnel IP and packets types.

      The new header has the source and destination addr of the tunnel end points. The virtual link acrosses an IP network.

                                                              ///(avoid text hidden while sliding)
                                                              
# IPsec

    IPsec provides a set of security services at the IP layer, the security architecture for IP is defined in RFC 2401. It is an architecture that IPv4 anf IPv6 can use. IPsec is a set of protocols, key mgmt. and algorithm for auth and encrption.

    It using 2 core protocols:

    * IP AH (see RFC 2402)

    * ESP (see RFC 2406)
    
    IPSEC Connection Steps:
    
    1. Init Process (spec of type of traffic to be encrypted)
    2. IKE phase 1
    3. IKE phase 2
    4. Transfer Data
    5. Terminate Tunnel

                                                            ///(avoid text hidden while sliding)
                                                            
>>>
to mentioned above in detail as below

    IP AH provides data-connection integrity and data-origin auth for connectionless, and to make trafic-flow confidetial.
    
     Auth Header Hash (complicated to comprehend)
     
              Encrpted                                   Decrypted
     IP Header + AH + TCP with Data ========> IP Header + AH + TCP with Data
    
>>>

    IKE (Internet Key Exchange) phase1 authenticates the IPsec peers and set up a secure channel between the peers to enable IKE exchanges.

    IKE phase2 means IPsec process negotiates the IPsec SA (Security Associations between 2 systems), when data transfer is completed, we have the payoad of data packets is encrypted using ESP. And tunnel is terminated if the IPsec SA are deleted or their lifetime expire.
    
    ESP Tunnel Model 
         
          New IP Header + IPsec Header + Encryption of IP header & Data
     

                                                            ///(avoid text hidden while sliding)            
___________________________________________________________________________

# DSL (the WAN Accessor!)

ADSL, the word you must hear often, is the most popular tech for residenial customers, it provides fast download speeds with a slower upload speed. SDSL is the most popular tech for enterprises, it provides the same upload && download speeds.

DSL is a tech being used for access WANs, it can provide WAN connectivity for lower cost than other dedicated services.DSL increases connectivity options for fixed remotes access and extranet offices & users. 

# to split DSL in pieces

DSL tech uses existing twisted-pair telephone lines to transport data, such as multi-media and video, to Movie and TVshow subscribers.

It using the infra provided by tele company, and within PTP thru public network.

# Limitation to DSL

* not secure (public intermediate)

* speed (distance) // In case of remote access, suggest using modem than DSL...

* quality (infra)

* always-on

* specified bandwidth

* charge for fixed monthly fee

# Options of DSL (T1/E1)

For some market, private DSL access is available, in this case, the PVCs (Permanent Virtual Circuits) extension is used from existing DSL access device.

DSL got FrameRelay & T1/E1 tech, save much cost while comparing to leased line.

Options for speed:

    ADSL
    SDSL
    ISDL (ISDN facility)
    HDSL faster than SDSL
    VDSL faster than ADSL

# Why use DSL

DSL is favorable based on cost for equal bandwidth while comparing to Dial-Up Access by Modem || ISDN. But in case of remote access, suggest using modem than DSL.

# Data Flow


                                               --- DSLAMs -------|
                                                                 |
                                                                 |
                                                                 |
       LAN - Router --- DSL|Modem|ISDN|Leased Line --- CO --- DSL Terminator --- Internet (WAN)                                            /
                                                   /
                                                  /
                                                 /
                                                /
             Cellphone is also a kind of Modem /
                                                       
                                                       ///(avoid text hidden while sliding)            

