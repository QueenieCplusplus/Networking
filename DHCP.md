# Unix_DHCP

生活中，絕大多數 Host 都是使用 DHCP 來取得動態 IP 位址。

# 特色如下：

* 支援行動裝置/ 和一直轉換工作空間的筆電
  人們每次走到新地點，到了新位址時，可能需要借用當下 LAN 的 DHCP 得到 IP，可能是借用當下環境中 LAN 的 DHCP 伺服器的可供租賃的 IP addr，倘若當下 LAN 沒有 DHCP Server 的話，人們就必須手動輸入當下環境網路的資訊（容易出錯）。

* 全自動處理，出租 IP 後，當此一手機或是筆電設備自網路中移除時，DHCP 便將該 IP 位址回收再次利用，可有效運用可用的 IP 位址。

* 設定將儲存於 DHCP Server
  因而避免因手動設定，造成的失誤。
  
* 路由器角度而言，它簡單單純的僅僅需要一道 CLI called，此路由器便可扮演 DHCP 角色了。

      $ip helper-address <server-ip>

# 客戶端 DHCP 設定：

＊ subnet

    DHCP Server 利用此訊息，得知子網路中所有位址，當然 DHCP 不會將子網路 ID 和廣播位址出租出去。

＊ remain IP = Static IP

    網管可以將如上子網路不可以出租的 IP 位址保留為『 靜態IP 』，配置給路由器或是交換機，也可以是伺服器主機位址。

＊ DNS (可以多個)

＊ default gateway

    該子網路中路由器的 IP 位址。
    
# DHCP setup for Router：

    ip dhcp excluded-address <ip range>
    ip dhcp included-address <ip range>

lease time

    _ day _h _min

domain name

next server, 與 IP 電話有關，此命令用來設定 UCM （統合通訊管理員）。

# DHCP CLI：

    // 查看出租給每個用戶端的 IP 位址狀態資訊。
    $show ip dhcp binding
    
>>>
    
    // 列出 IP 位址設定範圍，以及每個 pool 曾有的最大出租數量。
    $show ip dhcp pool <pool name>
    
>>>

    $show ip dhcp server statistics


