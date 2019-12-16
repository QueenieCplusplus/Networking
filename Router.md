# CiscoRouter_Static_Routing
靜態路由 (enter route | routing table manually) manually hereby means to do something thru CLI (command line interface), it can also be done using SNMP via agent, such as HP OpenView. Typically, specifying the network prefix (Network ID) and the IP (Internet Protocol) address of the next-hop router en route (french word) creates a Static Route. 

Static Routing is a method of populating routes into the routing table by an administrative action.By default, static routes receive the admin distance od 1, which can be changed to any value in the range from 1 thru 255. Static routes with an admin distance of 255 are never placed into the routing tabel.

A route won't be inserted into the routing table if the route's output interface is logically or physically down.

In case of static routes, the output interface may not be entered explicitly.

Lets Review the most-useful CLI in Internet:

    $ifconfig
    
    $ping (packet in network gropher)
    
    $traceroute
    
# IP Routing Command Step 1

Attention:
Routing of IP Traffic is enabled on Cisco routers by default. If necessary, IP routing can be turned off using the command in global config mode.

     $no ip routing 
     
and using the command to re-enable the IP routing.

    $ip routing
    
# IP Routing Command Step 2 & Routing Table

    R1$show interfaces ethernet 0
    Ethernet0 is up, line protocol is up
 
"Ethernet is up" means the interface is physically up, and "line protocol is up" means the interface is logically up. 

Supposed that the interface is assigned an IP address of 10.1.1.1. We then can verify the IP addr appears in the routing table by using 

        R1$show ip route 
        10.0.0.0/24 is subnetted. 3 subnets
        C 10.1.1.0 is directly connected. en0 
        C 10.2.2.0 is directly connected. TokenRing0
        C 10.0.255.0 is directly connected. Serial1
        
The hosts that are connected to segments interconnected with a single router must have routes pointing to the IP addr assiged to the corresponding router interface.( in other words, all of the hosts on segment can have a single default route pointing to the IP addr of the router's interface attached to the segment.


           PC  -------\  
                       \
           printer -----
                           single default IP addr of - interface - RouterQ  <-> 
           NB ----------
                         /
           other hosts -/ 
 
# How datagram go thru via Static Route Config

                                               Host P
                                     _______________|_
                                     |SegPP: 10.0.2.0/24     
                                    e0
                                     |
                              Router P
                                     |
                                     s0 10.0.255.3
                                     |
                                    /
                                   /
                                  / segPK: 10.0.255.0/24
                                 /
                                /
                               s1 IP: 10.0.255.2
                                |
                         Router K
                                |
                               s0 IP: 10.0.254.2
                                |
                               /
                              /
                             / segKQ: 10.0.254.0/24
                            /  
                           /
                           |
                           s0 IP: 10.0.254.1
                           |
                    Router Q 
                           |
             Host Q        e0
                  |        |
                  ----------
                segQQ: 10.0.1.0/24

>>>

                                           Host P
                                 _______________|_
                                 |SegPP: 10.0.2.0/24     
                                e0
                                                             |
                                                      Router P
                                                             |
                                                             s0 10.0.255.3
                                                             |
                                                            /
                                                           /
                                                          / seg: 10.0.255.0/24
                                                         /
                                                        /
                                                       s1 IP: 10.0.255.2
                            |
                     Router K|P
                            |
                            s0 IP: 10.0.254.2
                            |
                           /
                          /
                         / seg: 10.0.254.0/24
                        /  
                       /
                       |
                       s0 IP: 10.0.254.1
                       |
                Router Q 
                       |
         Host Q        e0
              |        |
              ----------
            seg: 10.0.1.0/24
            
>>>
 
    被設定的路由      設定指令   指向區段   遮罩(Prefix)  目的地位址(路由IP)      
    RouterK(config)$ip route 10.0.2.0 255.255.255.0 10.0.255.3
    
    RouterK(config)$ip route 10.0.1.0 255.255.255.0 10.0.254.1
    
https://github.com/QsCake/CiscoRouter_Static_Routing/blob/master/README.md#ip-route

>>>


                                           Host P
                                _______________|_
                                |SegPPKK: 10.0.2.0/24     
                                e0
                                |   
                         Router K
                                |
                                s0 IP: 10.0.254.2
                                |
                               /
                              /
                             / segKQ: 10.0.254.0/24
                            /  
                           /
                           |
                           s0 IP: 10.0.254.1
                           |
                    Router Q 
                           |
             Host Q        e0
                  |        |
                  ----------
                seg: 10.0.1.0/24
                
# ip route

A router is used to route traffic between 2 remote networks. And if a network has more than one router, the source of routing information than connecting routes becomes necessary.

The main process can be seperated by two, one is to identify the nework prefixes of the networks that are accessible thru other routers. And then create a static route for each of the identified netwrok prefixes using the 

    ip route <remote network adress of the segment> <subnet mask> <IP addr of next-hop router serail interface>
    //...

# config in detail to avoid pitfall

https://github.com/QsCake/CiscoRouter_Static_Routing/blob/master/README.md#host-config
[1] assume that the router Q & router P are config correctly. Then assume that Host Q on segment QQ must be able to communicate with Host P on segment PP, and that, to make it happen, the admin of Router K creates a static route as show as below:


                            <point to remote network address>
                            <point to remote segment>
                            <SegPP>          <subnet mask>    <next-hop router serial IP addr>
                                                 /8               
         Router K(config)$ ip route 10.0.2.0  255.255.255.0  10.0.255.3               

>>>

                                              Host P
                                     _______________|_
                                     |SegPP: 10.0.2.0/24     
                                    e0
                                     |
                              Router P
                                     |
                                     s0 10.0.255.3
                                     |
                                    /
                                   /
                                  / segPK: 10.0.255.0/24
                                 /
                                /
                               s1 IP: 10.0.255.2
                                |
                         Router K
                                |
                               s0 IP: 10.0.254.2
                                |

This route works fine when a datagram from Host Q destined for Host P arrives to Router K. Router K then performs a routing-table lookup that yields the IP addr of router P's interface Serail0, to which router K then forwards the datagram.

Router P identifies the dest IP addr as belonging to ta dirrectly connected network that is available thru interface Ethernet0. Thus, Router P completes the delivery of the datagram to Host P.

After the datagram is delivered to Host P, Host P replies to Host Q with its own datagram, which it is sent back to Router P.

# Host Config (resolve confusion)

Another important consideration is the host config. Static Routing won't work without appropiate routing config on Hosts.

All Hosts must have routing entries pointing to the appropriate interface of the routers.

# Conclusion (resolve confusion)

Usually, confusion occurs because an IP add is often associated with the whole router instead of a particular interface of the router. However, in case of Host, it doesn't matter, due to Host only has one interface.

