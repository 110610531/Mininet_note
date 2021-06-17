# mininet腳本執行(bridge)

先下載bridge-utils

`apt install bridge-utils`

### 建立一個基本bridge

* `br1.py`

```sh
#! /usr/bin/env python
from mininet.cli import CLI
from mininet.net import Mininet
from mininet.link import Link,TCLink,Intf

if 'main' == name:
  net = Mininet(link=TCLink)
  h1 = net.addHost('h1')
  h2 = net.addHost('h2')
  h3 = net.addHost('h3')
  br1 = net.addHost('br1')
  net.addLink(h1, br1)
  net.addLink(h2, br1)
  net.addLink(h3, br1)
  net.build()
  h1.cmd("ifconfig h1-eth0 0")
  h2.cmd("ifconfig h2-eth0 0")
  h3.cmd("ifconfig h3-eth0 0")
  br1.cmd("ifconfig br1-eth0 0")
  br1.cmd("ifconfig br1-eth1 0")
  br1.cmd("ifconfig br1-eth2 0")
  br1.cmd("brctl addbr mybr")
  br1.cmd("brctl addif mybr br1-eth0")
  br1.cmd("brctl addif mybr br1-eth1")
  br1.cmd("brctl addif mybr br1-eth2")
  br1.cmd("ifconfig mybr up")
  h1.cmd("ip address add 192.168.10.1/24 dev h1-eth0")
  h2.cmd("ip address add 192.168.10.2/24 dev h2-eth0")
  h3.cmd("ip address add 192.168.10.3/24 dev h3-eth0")
  CLI(net)
  net.stop()
  ```

h1 ping h2跟h3

`h1 ping -c 3 h2`

`h1 ping -c 3 h3`

![](Pic/5.jpg)

> **當 h1 ping h3，h2 不會監聽到任何的訊息**

### 建立兩個bridge

* `br2.py`

```sh
#!/usr/bin/env python
from mininet.cli import CLI
from mininet.net import Mininet
from mininet.link import Link,TCLink

if '__main__' == __name__:
  net = Mininet(link=TCLink)
  h1 = net.addHost('h1')
  h2 = net.addHost('h2')
  h3 = net.addHost('h3')
  h4 = net.addHost('h4')
  br1 = net.addHost('br1')
  Link(h1, br1) 
  Link(h2, br1)
  Link(h3, br1)
  Link(h4, br1)
  net.build()
  h1.cmd("ifconfig h1-eth0 0")
  h2.cmd("ifconfig h2-eth0 0")
  h3.cmd("ifconfig h3-eth0 0")
  h4.cmd("ifconfig h4-eth0 0")
  br1.cmd("ifconfig br1-eth0 0")
  br1.cmd("ifconfig br1-eth1 0")
  br1.cmd("ifconfig br1-eth2 0")
  br1.cmd("ifconfig br1-eth3 0")
  br1.cmd("brctl addbr mybr1")
  br1.cmd("brctl addbr mybr2")
  br1.cmd("brctl addif mybr1 br1-eth0")
  br1.cmd("brctl addif mybr1 br1-eth1")
  br1.cmd("brctl addif mybr2 br1-eth2")
  br1.cmd("brctl addif mybr2 br1-eth3")
  br1.cmd("ifconfig mybr1 up")
  br1.cmd("ifconfig mybr2 up")
  h1.cmd("ip addr add 192.168.10.1/24 brd + dev h1-eth0")
  h2.cmd("ip addr add 192.168.10.2/24 brd + dev h2-eth0")
  h3.cmd("ip addr add 192.168.20.1/24 brd + dev h3-eth0")
  h4.cmd("ip addr add 192.168.20.2/24 brd + dev h4-eth0")
  CLI(net)
  net.stop()
  ```

  特點: h1只能ping h2, h3 只能ping h4

  ![](Pic/6.jpg)

  ### 多加一個route

  * `br3.py`

```sh
  #! /usr/bin/env python
from mininet.cli import CLI
from mininet.net import Mininet
from mininet.link import Link,TCLink,Intf

if 'main' == name:
  net = Mininet(link=TCLink)
  h1 = net.addHost('h1')
  h2 = net.addHost('h2')
  h3 = net.addHost('h3')
  h4 = net.addHost('h4')
  br1 = net.addHost('br1')
  r1 = net.addHost('r1')
  net.addLink(h1, br1)
  net.addLink(h2, br1)
  net.addLink(h3, br1)
  net.addLink(h4, br1)
  net.addLink(br1,r1)
  net.addLink(br1,r1)
  net.build()
  h1.cmd("ifconfig h1-eth0 0")
  h2.cmd("ifconfig h2-eth0 0")
  h3.cmd("ifconfig h3-eth0 0")
  h4.cmd("ifconfig h4-eth0 0")
  br1.cmd("ifconfig br1-eth0 0")
  br1.cmd("ifconfig br1-eth1 0")
  br1.cmd("ifconfig br1-eth2 0")
  br1.cmd("ifconfig br1-eth3 0")
  br1.cmd("ifconfig br1-eth4 0")
  br1.cmd("ifconfig br1-eth5 0")
  br1.cmd("brctl addbr mybr1")
  br1.cmd("brctl addbr mybr2")
  br1.cmd("brctl addif mybr1 br1-eth0")
  br1.cmd("brctl addif mybr1 br1-eth1")
  br1.cmd("brctl addif mybr1 br1-eth4")
  br1.cmd("brctl addif mybr2 br1-eth2")
  br1.cmd("brctl addif mybr2 br1-eth3")
  br1.cmd("brctl addif mybr2 br1-eth5")
  br1.cmd("ifconfig mybr1 up")
  br1.cmd("ifconfig mybr2 up")
  r1.cmd('ifconfig r1-eth0 192.168.10.254 netmask 255.255.255.0')
  r1.cmd('ifconfig r1-eth1 192.168.20.254 netmask 255.255.255.0')
  r1.cmd("echo 1 > /proc/sys/net/ipv4/ip_forward")
  h1.cmd("ip address add 192.168.10.1/24 dev h1-eth0")
  h1.cmd("ip route add default via 192.168.10.254")
  h2.cmd("ip address add 192.168.10.2/24 dev h2-eth0")
  h2.cmd("ip route add default via 192.168.10.254")
  h3.cmd("ip address add 192.168.20.1/24 dev h3-eth0")
  h3.cmd("ip route add default via 192.168.20.254")
  h4.cmd("ip address add 192.168.20.2/24 dev h4-eth0")
  h4.cmd("ip route add default via 192.168.20.254")
  CLI(net)
  net.stop()
  ```

跟上個不同的地方是h1可以ping h2,h3,h4

![](Pic/7.jpg)

### vlan

再跑`vlan.py`之前請先安裝vlan

`apt install vlan`

* `vlan.py`

```sh
#!/usr/bin/env python
from mininet.cli import CLI
from mininet.net import Mininet
from mininet.link import Link,TCLink

if '__main__' == __name__:
  net = Mininet(link=TCLink)
  h1 = net.addHost('h1')
  h2 = net.addHost('h2')
  h3 = net.addHost('h3')
  h4 = net.addHost('h4')
  br1 = net.addHost('br1')
  r1 = net.addHost('r1') 
  Link(h1, br1)
  Link(h2, br1)
  Link(h3, br1)
  Link(h4, br1)
  Link(br1, r1)
  net.build()
  h1.cmd("ifconfig h1-eth0 0")
  h2.cmd("ifconfig h2-eth0 0")
  h3.cmd("ifconfig h3-eth0 0")
  h4.cmd("ifconfig h4-eth0 0")
  r1.cmd("ifconfig r1-eth0 0")
  br1.cmd("ifconfig br1-rth0 0")
  br1.cmd("ifconfig br1-rth1 0")
  br1.cmd("ifconfig br1-rth2 0")
  br1.cmd("ifconfig br1-rth3 0")
  br1.cmd("ifconfig br1-rth4 0")

  # 創造虛擬介面
  br1.cmd("vconfig add br1-eth4 10")
  br1.cmd("vconfig add br1-eth4 20")
  r1.cmd("vconfig add r1-eth0 10")
  r1.cmd("vconfig add r1-eth0 20")

  br1.cmd("brctl addbr mybr10")
  br1.cmd("brctl addbr mybr20")
  br1.cmd("brctl addif mybr10 br1-eth0")
  br1.cmd("brctl addif mybr10 br1-eth1")
  br1.cmd("brctl addif mybr10 br1-eth4.10")
  br1.cmd("brctl addif mybr20 br1-eth2")
  br1.cmd("brctl addif mybr20 br1-eth3")
  br1.cmd("brctl addif mybr20 br1-eth4.20")
  br1.cmd("ifconfig mybr10 up")
  br1.cmd("ifconfig mybr20 up")
  br1.cmd("ifconfig br1-eth4.10 up")
  br1.cmd("ifconfig br1-eth4.20 up")
  r1.cmd("ifconfig r1-eth0.10 up")
  r1.cmd("ifconfig r1-eth0.20 up")
  h1.cmd("ip addr add 192.168.10.1/24 brd + dev h1-eth0")
  h1.cmd("ip route add default via 192.168.10.254")
  h2.cmd("ip addr add 192.168.10.2/24 brd + dev h2-eth0")
  h2.cmd("ip route add default via 192.168.10.254")
  h3.cmd("ip addr add 192.168.20.1/24 brd + dev h3-eth0")
  h3.cmd("ip route add default via 192.168.20.254")
  h4.cmd("ip addr add 192.168.20.2/24 brd + dev h4-eth0")
  h4.cmd("ip route add default via 192.168.20.254")
  r1.cmd("ip addr add 192.168.10.254/24 brd + dev r1-eth0.10")
  r1.cmd("ip addr add 192.168.20.254/24 brd + dev r1-eth0.20")
  r1.cmd("echo 1 > /proc/sys/net/ipv4/ip_forward")
  CLI(net)
  net.stop()
  ```