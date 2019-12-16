# half_duplex
半雙工

電腦通訊是依靠電磁波 Electromagnetic Wave 此媒介將訊號從 UART (universal async receiver/transmitter) 將資料在兩端編解碼後轉譯給人類閱讀和操作等資料使用。

傳輸方式有 guided 和 wireless 兩種方式，傳遞媒介則有 twisted pair(electromagnetic wave), coaxial cable 和 optical fiber (microwave) 三種，目前最常見的媒介是最後一個即光纖材質的媒介或是無線通訊，而 wireless 應用在導航和定位與廣播的無線電波 (radio) 等應用的設備其實還是使用如上述的電磁波。

在一些機構因為資源和安全和速度等目的的關係，可以做點對點這種 leased link 專線的網路傳輸方案，在兩設備中提供一條連線，但這些在時間和金錢成本規劃上比較浩大，在現實生活中，比較時常被考慮的方案是多點傳輸，多點表示了同一上述媒介，被多設備所共用。

PTP DSL(not Leased Line)using twisted pair https://github.com/QsCake/CCDA_VPN_DSL/blob/master/README.md#dsl

duplex 則是 simplex 的對應詞，是指兩端點的 UART 『同時可以成為接收方與傳輸方』，而 go 語言的 goroutine 則在通訊中使用了 half-duplux 半雙工的技術，就是兩設備或兩端點同時送出訊號 emit signal，但不同時傳送 transmit，所以還是有點像是肉眼看不到差異的非同步(非字面上同步的概念)。

不過真正的同步要共用資源，會使用鎖，反而產生等待的時間，所以目前還是推薦使用非同步搭配原子操作 atomic。 https://codertw.com/程式語言/511579/
