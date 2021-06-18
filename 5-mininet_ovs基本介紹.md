# OVS-open vswitch


一種虛擬交換器，可用來作為L2switch，切割網域，QoS或是流量監控，同時支持openFlow協定。

![](Pic/1.png)

* `mn --topo single,2`建立拓樸

### 顯示資訊

`ps -aux | grep controller`查詢controller的id

`kill -9 [controller_id]`可刪除controller的id

`ovs-ofctl show [switch]` 顯示swtich的一些資訊

![](https://github.com/110610531/Mininet_note/blob/main/pic/11.jpg)

`dpid`: data path id 在mininet裡為了區分所以第一台的id是1,第二台是2...

`n_tables:254`:在這ovs環境下支援254個tables

`capabilities`:這台交換機所具備的能力

### 建立規則

`ovs-ofctl add-flow [switch] in_port=1,actions=output:2`建立規則,資料從1號口進去,接著從2號口轉發


建立好規則後打`ovs-ofctl dump-flows [switch]`

![](https://github.com/110610531/Mininet_note/blob/main/pic/12.jpg)

`n_packets=5`有5個封包是因為arp的關係

`ovs-ofctl del-flows [switch] in_port=1`把in_port=1的規則刪除

