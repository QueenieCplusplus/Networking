# IP_Addr
IP address

# Classful is a Supernet

                           8                 16                24              31
           0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 
           |_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|


    ClassA 

           -> Network ID <- ------------------> Host ID <-----------------------

    ClassB

          ----------> Network ID <------------ ----------> Host ID <------------

    ClassC

          ------------------> Host ID <----------------------- -> Network ID <- 

    ClassD

          --------------------------> Multicast Group <-------------------------

    ClassE

          ------------------------------> Reserved <-----------------------------

# Subnet, Continguos Subnetting

                       8                 16                24              31
           0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 
           |_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|


    IP addr

    Mask  

           <---Network ID---> <-------Subnet ID-------> <-------Host ID -------->

# Subnet, NonContinguos Subnetting

                           8                 16                24              31
           0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 8 0 1 2 3 4 5 6 7 
           |_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|


    IP addr

    Mask  

           <-Network ID-> <-Subnet ID-> <-Host ID -> <-Subnet ID-> <-Host ID -> 

# Primary IP adress & Sencondary IP address (temaporary measure)

Secondary IP address were introduced to overcome IP address allocation probles associated with the limitation of the IP address.

It also used in the migration from classful to classless.

Seconday IP address is an additonal IP address that is assigned to an interface of a router or host that has already been assigned an IP addr, this original IP adress is Primary IP address, and there can be multiple secondary IP addr on the same interface, and if the network prefix coincides both of the two IP adrr, then the Host ID of the secondary IP addr must be different from that of the primary IP addr.

Howerver, Secondary IP adress causes many problems...

# Private IP address

this kind of IP address are guaranteed to never appear in the internet, no authority responsible for allocation of IP addr can ever assign any IP addr to networks connected to the internet.

According to RFC 1918, the network prefixes are describes all of the private IP addr

          10.0.0.0/8

          172.16.0.0/12

          192.168.0.0/16


# Moder & Classless IP address

(TBD)
