# mininet 繪圖

**ubuntu 取得資料**

`mn --link=tc,bw=10,delay='1ms',loss=0`

使用`tee`指令將處理過程顯示在螢幕上，並且儲存到後面的檔案中

* h1

`iperf -s -i 1 -p 5555 | tee tcp`

`iperf -s -i 1 -p 6666 -u | tee udp`

* h2

`iperf -c 10.0.0.2 -p 5555 -t 100`


`iperf -c 10.0.0.2 -p 6666 -u -b 3M -t 100`

將資料處理成`mytcp`與`myudp`兩個檔案

* mytcp

`cat tcp | grep "sec" | head -n 100 | tr "-" " " | awk '{print $4,$8}' > mytcp`

* myudp

`cat udp| grep "sec" | head -n 100 | tr "-" " " | awk '{print $4,$8}' > myudp`

![](https://github.com/110610531/Mininet_note/blob/main/pic/g1.jpg)

**開始繪圖**

* gnuplot

```sh
plot "mytcp.txt" title "tcp flow" with linespoints, "myudp.txt" title "udp flow" with linespoints

set xrange [0:30]

set xtics 0,5,30

set xlabel "time (sec)"

set yrange [0:10]

set ytics 0,1,10

set ylabel "throughput (Mbps)"

set title "tcp vs. udp

set terminal "gif"

set output "result.gif"

replot
```
![](https://github.com/110610531/Mininet_note/blob/main/pic/0.jpg)
