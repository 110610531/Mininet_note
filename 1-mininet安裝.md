# mininet 安裝

**mininet 介紹**

mininet是一個可以透過一些虛擬終端機、路由器、交換器等連接創建虛擬網路拓樸的平台，因此可以輕易的在自己的個人電腦中創作支援SDN的區域網路，在裡面創造出的虛擬的host並以真實電腦般發送封包，且可以使用SSH(Secure Shell)登錄虛擬host中操作。

**使用環境**

* ubuntu16.04

**mininet install**

```sh
git clone git://github.com/mininet/mininet 
mininet/util/install.sh -a
```

安裝好之後輸入`mn`就可以啟動mininet

![](https://github.com/110610531/Mininet_note/blob/main/pic/1.jpg)

`net`:可以看到各個鏈節訊息

可以試試h1 ping h2

`h1 ping -c 1 h2`

![](Pic/2.jpg)

`xterm`:打開終端機 

`xterm h1 h2`

![](Pic/3.jpg)

離開`exit`
