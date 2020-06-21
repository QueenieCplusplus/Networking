# VLAN
Networking, 虛擬區域網路

定義上來說，請先了解虛擬網路 LAN，其為包括同一 Broadcast 領域中的所有設備，這些設備包含了使用者設備如手機或筆電、交換機、伺服器、電纜、無線AP、列印機、IP 電話，這代表了當同一廣播領域中某一設備發出廣播 frames 時，所有同一領域的設備都會得到 copy 複本。

若要分割或是建立兩個以上不同的廣播領域，則可以利用 VLAN trunk 技術對作用域進行分隔（分割）。

>>>

         Vlan ID 1  ------------  SW  ------------ Vlan ID 1005
         A Broadcast Area                          Another Broadcast Area
         
                                 Router
                                  
______________________________________________________________________________

減少 CPU 耗損與對 Security 設計上的優勢：

* 減少每一作用域中設備數量，減少接收廣播訊框數量，降低每台設備所需要面臨的 CPU 消耗量。

* 減少訊框 copies，降低安全風險。

* 如上，尤其針對需要對發送機密資料的主機的安全機制設計的考量。

* 依照管理區分，分隔使用者，（且不受地理限制），是較為彈性的規劃。

* 將故障 failout 的設備限制在某一廣播領域，對排除問題速度上較為有保障。

* 減少 spanning tree protocol 工作量。

# STP

生成樹協定

https://zh.wikipedia.org/wiki/生成树协议

# VTP

虛擬區域網路主幹中繼協定

https://en.wikipedia.org/wiki/VLAN_Trunking_Protocol

# Access

        Notebook ---- IP Phone ---------------------------- SW (+ PBX)
         fiber optic jumper wire     Unshield Twisted Pair
              (光纖跳接線)                  (數據傳輸線)


# Intent

          SW | Fa0/5 interface ------------------------- |SW 
          
          trunk: daynamic auto                           dynamic desirable, on

# Trunking

使用 802.1Q 協定，發送 frames 到其他交換機，在 forward frames 抵達目的地前，其訊框的 header 會插入 4 bytes 資訊。

# 802.1Q

                Des | Src | tag | type | Data | FCS
                
                tag includes:
                type | prority | flag | Vlan ID

# CLI

下列命令能識別出使用 trunking 協定的 SW 介面，即目前正以 VLAN trunk 運作的介面。

         $show vlan brief
>>>

         $show vtp status
         
>>>

         $show interfaces trunk
         
>>>

         $show interfaces switchport
