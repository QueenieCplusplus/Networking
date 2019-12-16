# Unicast_IP_Route
單播網際網路路由

How IP datagrams are delivered via a network that can consits of multi-segements.

# Routing

It is the IP function responsible for delivering IP datagrams thru a network, in case of a only one segment, then the routing is very simple, the hosts can use link layer modules (network components) to do datagram exchange directly thru the segment.

If multi-segments are interconnected with a single router, the function of routing becomes much complexer, and host shall be aware that the router to be able to use it for their intersegments communication, and router can use its link layer modules to facilitate the hosts' communications.

if muti-segments are interconnected with multi-routers, the function of routing become challenging. Not only must hosts know the presence of the routers, but routers must also know of each other and what segments are availbale behind them.

# Prefix_based Routing

It means IP Routing pay no attention to the host ID, only pay attention to choose which interface the IP datagram to be sent & forward to which next hop router must be received it for further delivery.

# Destination_based Routing

It means src addr are not taken into account.

# IP Routers (forwarder)

they are intrinsic components of IP-based networks. Their main function is forward IP datagrams amongs segments until they arrive the destination.

(TBD)



