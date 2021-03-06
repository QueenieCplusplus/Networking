# Unix_Traceroute
Networking, traceroute 

# 傳送流程

>>>
TTL 反應出存活時間，TTL Exceeded 則代表時間逾時。


                TTL=2           TTL-1=1       TTL-1=0 (指出來源)
                TTL=1           TTL-1=0  
               IP addr           IP addr
                 ||               ||
               gateway          gateway
                 |               |
           LAN   |          WAN  |
     PC  ------- | Router -------| Router -------Host
                    |               |
                    |               |
                    ------Router-----
                    
                    
# 終端機命令
 
     $traceroute www.google.com
     
>>>
倘若路由器或防火牆可以過濾 traceroute 命令的訊息，則會造成部分路徑無法顯示。

    traceroute to www.google.com (172.217.27.132), 64 hops max, 52 byte packets
     1  192.168.100.1 (192.168.100.1)  1.892 ms  1.167 ms  1.096 ms
     2  h254.s98.ts.hinet.net (168.95.98.254)  2.125 ms  2.384 ms  2.186 ms
     3  tpe4-3301.hinet.net (168.95.24.58)  5.912 ms  3.103 ms  2.227 ms
     4  tpe4-3201.hinet.net (220.128.11.222)  2.554 ms  16.608 ms  10.385 ms
     5  pcpd-3211.hinet.net (220.128.12.86)  2.678 ms
        pcpd-3211.hinet.net (220.128.12.166)  3.427 ms  2.530 ms
     6  72.14.218.142 (72.14.218.142)  5.092 ms
        72.14.202.178 (72.14.202.178)  3.017 ms  3.627 ms
     7  * * 108.170.244.97 (108.170.244.97)  45.577 ms
     8  209.85.142.13 (209.85.142.13)  4.156 ms
        72.14.238.16 (72.14.238.16)  4.409 ms
        209.85.246.8 (209.85.246.8)  4.325 ms
     9  209.85.142.13 (209.85.142.13)  4.382 ms
        108.170.244.75 (108.170.244.75)  5.274 ms
        209.85.240.15 (209.85.240.15)  4.385 ms
    10  108.170.244.129 (108.170.244.129)  5.856 ms
        tsa03s02-in-f4.1e100.net (172.217.27.132)  3.904 ms  12.608 ms
        
# 運作原理

封包 IP 標持有一 TTL 欄位，凡是創建一新的封包的 origin Host 都會為該封包的 TTL 設定初始值，之後，凡是經手處理過該封包的每一台路由器均會將 TTL-- (遞減)，倘若 TTL 數值到達 0 時，該路由器會丟棄該封包，避免 Loop 迴圈，隨即前述的路由器會透過返回 ICMP 封包的 TTL Exceed 訊息告知 origin Host，而源頭主機則能辨識出該返回訊息的路由器 IP 位址。

traceroute 會依照次序丟出 3 個封包，分別為 TTL =1 、 TTL =2 、 TTL =3 ， 等到目的地主機回覆訊息為止。
        
# traceroute 命令的特點

* 輸出結果的細節更多

* 需要較多的時間建立結果

* 當工程師排除網路問題時，使用 ping 排除（隔離）問題，使用 traceroute 能準確指出路由發生問題的位置。（ 因為 traceroute 能系統地顯示封包行徑 IP 網路的距離。 ）
        
# 與 ping 命令的相同處

* 在路由上，兩者都有標準和進階版本。

* 反向路徑測試效果更好。

# 反向路徑

傳送路徑稱為 forward route, 而反向路徑稱為 reverse route, 即封包返回的路徑。

