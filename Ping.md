# Unix_Ping
Network, Packet Internet Gopher 

此命令能測試每個網路設備是否能夠繞送或是轉送封包，網路上常見的繞送問題，可能並非出自設備的根本原因，而是像是介面失去效用、協定問題、功能設定錯誤的問題所造成。

# 傳送流程



       Local Host --- SW --- Router --- WAN --- Router --- SW --- Other Host
       
    packet ICMP|IP  --->>>                               <<<--- IP|ICMP
       
    Echo Request                                                   Echo Reply
    
    

# 發生情境
ping 能夠排除故障疑慮的範圍，縮小問題。發生情境如下：

人們以 Email 或是 TEXT Message 通知聯絡自己，自己用 SSH 連線到 Router 檢測是否能成功下達 ping 命令。

# 操作原理

藉由傳送封包到 IP 位址，測試之間的連接性 connectivity，但需要 req target IP 位址上的裝置能夠回應封包。

此封包夾帶了一層面訊息：『 如果你收到此封包，而且目的就是你的話，煩請回覆。 』，此時來源發送封包的自己 Local Host 期待收到其他 Hosts 回覆訊息，因為倘若一來一往成功的話，代表封包成功地從來源主機到達目的地主機了。

# ICMP 協定

此訊息座落於 IP 層級的標頭上。

* Echo Request

* Echo Reply

# 支援平台

ping 命令支援不同平台（作業系統）及不同裝置。

# 設定選項

* Dest 可以是 DN 或是 IP

* Echo Request 傳送次數 counts

* Wait for Echo Reply 的 timeout|interval 

* Packet Size

# 終端機介面

       ＄ping www.github.com
>>>

       PING github.com (140.82.114.4): 56 data bytes
                                             time to live
       64 bytes from 140.82.114.4: icmp_seq=0 ttl=45 time=265.769 ms
       64 bytes from 140.82.114.4: icmp_seq=1 ttl=45 time=287.306 ms
       64 bytes from 140.82.114.4: icmp_seq=2 ttl=45 time=307.193 ms
       64 bytes from 140.82.114.4: icmp_seq=3 ttl=45 time=325.773 ms
       64 bytes from 140.82.114.4: icmp_seq=4 ttl=45 time=349.076 ms
       64 bytes from 140.82.114.4: icmp_seq=5 ttl=45 time=368.189 ms
       64 bytes from 140.82.114.4: icmp_seq=6 ttl=45 time=387.615 ms

>>>

       --- github.com ping statistics ---
       8 packets transmitted, 7 packets received, 12.5% packet loss
       round-trip min/avg/max/stddev = 265.769/327.274/387.615/40.666 ms

