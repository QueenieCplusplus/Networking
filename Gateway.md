# Unix_Default_Gateway
Networking, 預設閘道

Flow Chart
以下三台設備都有各自和共享的 IP (即 default gateway) 和 MAC addr, 而其中 PC1 和 PC2 在同一子網路，所以封包可以直接傳送，但是如果需要傳送到非同一子網路的伺服器或其中終端設備，要經過路由器繞送。

            PC1 ---> Remote ---> Router ---> ServerX

             |
             V

            Local

             |
             V

            PC2
            
            

CLI

    $netstat -rn
    
>>>
    
    $netstat -i
    
>>>

    // ARP 能夠偵測同一子網路的設備如 PC 或是 Router 或是另一子網路中 Router 的 ARP 紀錄
    // 藉此檢視 IP (default gateway) 對應的 MAC 位址。
    $arp -a

Concept:

    Host 主機的路由，只要封包的源頭和目的不是同一個子網路時，Local Host 本地主機發送封包到非同一子網路的目的地前，需要先把封包送到它所屬的預設閘道 Default Gateway，即利用 Lan 中 ARP 至此路由的 MAC 位址。
    // 當然，如果是同一子網路，就可以不經過路由器，直接將封包送到另一台 PC 。
    
Options Setup:

1. Host 所在的 LAN 與 Default Gateway 的 LAN 必須在同一 VLAN。

2. Host 及 Default Gateway (即路由) 必須在同一 Subnet。

3. Host 的 Default Gateway 即 Router 的 IP addr。

4. 注意不讓 Port 防護設定導致 LAN 中 SW drop Frames. （其實這句我沒遇過，不太懂...）


