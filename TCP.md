# Unix_TCP
Transmission Control Protocol, 利用 TCP 標頭中的目的地通訊阜號，藉此識別接收端應用程式。
IP 欄位則能利用十進位的 6 或是 17 來判定是 TCP 還是 UDP。

Flow Chart:

     PC (Host) <---   | Eth.| IP | TCP port | HTTP & Data |  <--- Web Server
     
     Des Port
     
     port 1024 for browsing github
     port 1025 for browsing google
     port 1026 for Email App
     port 1027 for iMessage App
     port 1028 for using Android Studio App
     port 1029 for using Xcode App
   
Concept：

當 Host 收到網路上任一訊息時，主機需要自行決定由眾多應用程式（程序）中哪個 app 去負責處理收到的資料。


