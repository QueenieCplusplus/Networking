# CCDA_SNMP_CDP
簡單網路管理協定

Whether you are going to build a network from scratch or update an exisiting system, taking the time to assess the network-mgmt architecture is a wise approach.

Network mgmt is an essential service in any network, but it requires a complex task to plan & design to ensure the integration of the mgmt system with the network.

There are many features a network-mgmt tool is able to do in high level:

FCAPS

       [1] Fault-mgmt
       [2] Config
       [3] Accounting
       [4] Performance
       [5] Security 

OAM&P

       [1] Operation
       [2] Admin
       [3] Maintenance
       [4] Provisioning
______________________________________________________________

# Diagram shows SNMP


       Enterprsie Console  <------------------------> Enterprise Console
      
     |                                                                 | 
     |                                                                 |
     |                                                                 |
     
     Collection Station                               Collection Station
           (Server)        Failover (Status Poll)            (Server)
     |                                                                 |
     
     SNMP                                                             SNMP
     
     |                                                                  |
     V                                                                  V
     
     Collection Domain                                  Collection Domain

______________________________________________________________

# A Roadmap to Network Mgmt

Components:

              [1]Servers to collect the info
              [2]Agent with Mgmt Tool Device

Constraints:

       The indiscriminate use of SNMP can have an adverse effect on netwrok performance. 
       
       The Message consume link bandwidth, and the formatting if the responses consumes internal CPU and memory resources on the target network devices. 
       
       The timeliness of Event Notification and Reporting granularity are all network-mgmt design decisions that effects the infra and bandwidth.
       
                                                                ///...
CLI:

SNMP is a req-res architecture. The manager issues req from NMS to get data val from agents || set contro val on agents, and agent responds with cfrm or related data.

SNMP allows agnets to generate unsloicited reports, known as traps, when some predefined event occurs.(the event involves crossing some threshold val.)

         get
         set
         trap

______________________________________________________________

# CDP

It means Cisco Dev a propietary protocol that informs other devices of their presence of the network as well as receives info of neiboring devices.
