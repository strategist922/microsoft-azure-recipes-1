# 3.1 建立虛擬機器

在 Microsoft Azure 虛擬機器服務上，可以使用**快速建立**或是從**組件庫（Gallery）**輕鬆建立 Windows Server 或是 Linux 等虛擬機器。

## 事前準備
* 擁有 Microsoft Azure 訂閱帳戶
* 瞭解 Windows Server 或 Linux Server 基本操作

## 快速建立步驟

在 Microsoft Azure 管理後台，按下左下角的**+新增**，選擇 **計算** » **虛擬機器** » **快速建立**，填寫一些資料就可以開始佈建虛擬機器。

![快速建立虛擬機器](https://skgitbook.blob.core.windows.net/azurerecipestw/3-1-1-quick-create-vm-winsrv.png)

**DNS** 欄位是填寫要透過什麼網址來存取或連接虛擬機器，這裡只需要填寫一個名稱如：_abcd_，Microsoft Azure 會配發一個 _\*.cloudapp.net_ 的網域名稱供你使用，所以如果這個欄位填寫了 _abcd_，則虛擬主機的網址就是 _abcd.cloudapp.net_。

**影像**的選單可以選擇要建立的虛擬機器的類型，在這裡你可以選擇各種版本的 Windows Server、Linux Server 來建立虛擬機器。

**大小**就是虛擬機器使用的運算資源規模，這裡就根據需要來選擇，而不同的規模提供不同的 CPU、記憶體、磁碟類型，而也會影響到使用價格，關於不同規模提供的內容、價格資訊可以參考[官網上的定價說明](http://azure.microsoft.com/zh-tw/pricing/details/virtual-machines/)。

**使用者名稱**及**密碼**就是虛擬機器建立後的管理者帳號資訊，這裡密碼欄位會要求較高安全性的密碼規則（英文數字至少有一大寫一小寫，還有一個特殊符號）。

**區域/同質群組**的欄位則是打算將虛擬機器放在哪一座資料中心。

上述欄位都填寫完畢，一切無誤之後，按下右下角的**建立虛擬機器**按鈕，Microsoft Azure 就會開始在指定的資料中心佈建指定的虛擬機器。

## 完整建立步驟

快速建立可以只有一步驟就完成虛擬機器的建立，但有一些操作及設定無法在**快速建立**的畫面中完成，這時可以在新增虛擬機器時選擇**從組件庫**的選項來操作，接下來會跳出一個對話盒一步步完成虛擬機器的建立：

![從組件庫建立虛擬機器](https://skgitbook.blob.core.windows.net/azurerecipestw/3-1-2-create-vm-from-gallery.png)

第一個畫面是選擇要用什麼映像檔來建立虛擬機器，在最左側先選擇類別，像是 MICROSOFT 的 SERVER 系列，或是 UBUNTU、CENTOS 甚至是 ORACLE，然後再從中間的區域選擇版本，在點選不同的版本後，右側的區域就會顯示該映像檔的說明資訊，像是發行商、以及可以在哪些資料中心部署等等。

![設定虛擬機器](https://skgitbook.blob.core.windows.net/azurerecipestw/3-1-3-config-name-scale-credentials.png)

選擇好映像檔後，下一步是設定一些虛擬機器的組態，有的映像檔可選擇**版本發行日期**，像上圖中可以選不同時間點的 Ubuntu 14.04 LTS 釋出的映像檔；而**虛擬機器名稱**的欄位只是為了管理方便給這個虛擬機器的名字，而不是網址；再來就是在**層次**以及**大小**欄位選擇虛擬機器需要的運算資源，資源及價格的差異可以參考[官網上的價格頁面](http://azure.microsoft.com/zh-tw/pricing/details/virtual-machines/)；最後是管理帳號的部份，有的映像檔可以在此介面上傳 SSH 金鑰做為連線認證資訊，也可以輸入密碼（需注意密碼會要求較高的強度）。

![設定虛擬機器](https://skgitbook.blob.core.windows.net/azurerecipestw/3-1-4-config-domain-location-storage.png)

再下一步，首先決定要以這個虛擬機器建立一個新的**雲端服務**，還是要把這台虛擬機器加入其它已經建立的雲端服務下（做負載平衡或是高可用性群組），因為網址是**配給雲端服務而不是配給虛擬機器**，所以只有當建立新的雲端服務時才需輸入 **DNS 名稱** 用來做為連接的資訊；**區域/同質群組/虛擬網路**決定要把虛擬機器佈建在哪個資料中心，或是掛到_同質群組_或_虛擬網路_下；**儲存體帳戶** 的設定是為了存放虛擬機器的磁碟檔案的位置，這裡會搭配 **Microsoft Azure 儲存體服務**來處理，我們會在之後的食譜中詳細介紹，這裡先建立或選擇一個置放虛擬機器磁碟機的位置即可；**可用性設定組**可以選擇要不要建置高可用性（HA, high-availability）架構，在同一個可用性設定組裡的虛擬機器才能做互相備援；最後是設定欲開放的**端點（endpoint）**，因為虛擬機器前都有一個防火牆，如果只是虛擬機器開啟連接埠（port），這個連接埠並不會與網際網路相通，所以必須要在此開啟端點對應虛擬機器的連接埠，以此圖為例，雖然虛擬機器裡跑 SSH 會開 22 這個連接埠，但依然要開一個 22 的端點來對應，這樣才可以透過網際網路來連線存取。（Windows Server 在此會隨機開一個端點對應_遠端桌面_的服務）

![安裝代理程式](https://skgitbook.blob.core.windows.net/azurerecipestw/3-1-5-install-agent.png)

最後一步是確定是否要安裝一些代理程式，或是防毒軟體，最後按下右下的勾勾按鈕，就可以開放佈建虛擬機器了。

![佈建虛擬機器中](https://skgitbook.blob.core.windows.net/azurerecipestw/3-1-6-deploying-vm.png)

等待約數分鐘後，管理後台上顯示該虛擬機器的狀態為**正在執行**時就可以連線進去操作了。

![虛擬機器已經可以使用](https://skgitbook.blob.core.windows.net/azurerecipestw/3-1-7-vm-is-running.png)


## 注意事項
* 虛擬機器一旦建立成功並啟動後，就會開始計算費用，如果測試或使用後暫時不會使用，要在管理介面上將該虛擬機器關機取消配置（不是在虛擬機器中關機），這樣就會暫停計算使用量，也就不會再無端增加費用。
