DNS_Baidu.md
# 关于百度DNS的解析过程

if现在我用一台电脑，通过ISP接入互联网，那么ISP就会分配给我一个DNS服务器（非权威服务器）。
now，我的computer向这台ISPDNS发起请求查询www.baidu.com。
首先，ISPDNS会检查自己的缓存中有没有这个地址，有的话直接返回给我的PC，没有的话，ISPDNS会把请求发送给根服务器（13台）。
根服务器发现是.com结尾的即是.com这个顶级域名下的，就告诉请求者负责解析.com的DNS服务器。（目前百度有4台baidu.com的顶级域名服务器）。
ISPDNS再次向baidu.com这个域的权威服务器发起请求，baidu.com收到后，查一下www这台主机，然后把它的IP返回给IPSDNS，然后IPSDNS把地址返回给我的PC，并且存入告诉cache中，以便再次访问。
//当然这是完美的解析不走，不过百度的DNS没这么简单。
```
[root@zichen star]# nslookup www.baidu.com
Server:        211.140.13.188
Address:    211.140.13.188#53

Non-authoritative answer:
www.baidu.com    canonical name = www.a.shifen.com.
Name:    www.a.shifen.com
Address: 220.181.112.76
Name:    www.a.shifen.com
Address: 220.181.111.111
```
百度有个cname=www.a.shifen.com.的别名，这所怎么一个过程呢？用dig工具跟踪一下。

```
[root@zichen star]# dig +trace www.baidu.com

; <<>> DiG 9.8.2rc1-RedHat-9.8.2-0.2.rc1.fc16 <<>> +trace www.baidu.com
;; global options: +cmd
.            167778    IN    NS    b.root-servers.net.
.            167778    IN    NS    d.root-servers.net.
.            167778    IN    NS    f.root-servers.net.
.            167778    IN    NS    m.root-servers.net.
.            167778    IN    NS    e.root-servers.net.
.            167778    IN    NS    h.root-servers.net.
.            167778    IN    NS    l.root-servers.net.
.            167778    IN    NS    g.root-servers.net.
.            167778    IN    NS    i.root-servers.net.
.            167778    IN    NS    k.root-servers.net.
.            167778    IN    NS    c.root-servers.net.
.            167778    IN    NS    a.root-servers.net.
.            167778    IN    NS    j.root-servers.net.
;; Received 228 bytes from 211.140.13.188#53(211.140.13.188) in 1841 ms--------（1）

com.            172800    IN    NS    a.gtld-servers.net.
com.            172800    IN    NS    b.gtld-servers.net.
com.            172800    IN    NS    c.gtld-servers.net.
com.            172800    IN    NS    d.gtld-servers.net.
com.            172800    IN    NS    e.gtld-servers.net.
com.            172800    IN    NS    f.gtld-servers.net.
com.            172800    IN    NS    g.gtld-servers.net.
com.            172800    IN    NS    h.gtld-servers.net.
com.            172800    IN    NS    i.gtld-servers.net.
com.            172800    IN    NS    j.gtld-servers.net.
com.            172800    IN    NS    k.gtld-servers.net.
com.            172800    IN    NS    l.gtld-servers.net.
com.            172800    IN    NS    m.gtld-servers.net.
;; Received 503 bytes from 198.41.0.4#53(198.41.0.4) in 1884 ms-------------------------（2）

baidu.com.        172800    IN    NS    dns.baidu.com.
baidu.com.        172800    IN    NS    ns2.baidu.com.
baidu.com.        172800    IN    NS    ns3.baidu.com.
baidu.com.        172800    IN    NS    ns4.baidu.com.
;; Received 167 bytes from 192.31.80.30#53(192.31.80.30) in 305 ms-------------------（3）

www.baidu.com.        1200    IN    CNAME    www.a.shifen.com.
a.shifen.com.        86444    IN    NS    ns4.a.shifen.com.
a.shifen.com.        86444    IN    NS    ns7.a.shifen.com.
a.shifen.com.        86444    IN    NS    ns9.a.shifen.com.
a.shifen.com.        86444    IN    NS    ns5.a.shifen.com.
;; Received 194 bytes from 202.108.22.220#53(202.108.22.220) in 68 ms-------------（4）

```

DIG工具会在本地计算机做迭代，然后记录查询的过程。
第一步是我这台PC的ISPDNS获取到13个根服务器的13个IP和主机名【b-j】.root-servers.net。
第二步是向其中的一台根域服务器198.41.0.4发送www.baidu.com的请求，他返回来com.顶级域的服务器的IP（未显示）和名称。
第三步是向com.域的一台服务器192.31.80.30请求www.baidu.com，他返回来baidu.com域发服务器IP（未显示）和名称.
第四步，向百度的顶级域名服务器dns.baidu.com.请求www.baidu.com，他发现这个www有别名叫www.a.shifen.com。
按照一般逻辑，当dns请求到别名时，查询都会终止，而所重新发起查询别名的请求，所以此处应该返回的是www.a.shifen.com.但是为什么返回的是a.shifen.com这个NS呢？
此处我们可以用：

```
[root@zichen star]# dig +trace shifen.com
shifen.com.        172800    IN    NS    dns.baidu.com.
shifen.com.        172800    IN    NS    ns2.baidu.com.
shifen.com.        172800    IN    NS    ns3.baidu.com.
shifen.com.        172800    IN    NS    ns4.baidu.com.
;; Received 170 bytes from 192.26.92.30#53(192.26.92.30) in 325 ms
```

发现shifen.com的顶级域名服务器和baidu.com的域名服务器是同一台！
当 我拿到www.baidu.com的别名www.a.shifen.com的时候，本来要重新到com域查找shifen.com的NS，又因为，两个域 在同一台NS上，所以直接向本机发起了shifen.com域发现请求的www.a.shifen.com是属于a.shifen.com这个域的，于是 就把a.shifen.com的这个NS和IP返回，让我到a.shifen.com这个域的域名服务器上查询www.a.shifen.com。
于是

```
shifen.com.        7200    IN    A    202.108.250.218
shifen.com.        86400    IN    NS    ns3.baidu.com.
shifen.com.        86400    IN    NS    ns1.baidu.com.
shifen.com.        86400    IN    NS    ns2.baidu.com.
shifen.com.        86400    IN    NS    ns4.baidu.com.
;; Received 186 bytes from 220.181.37.10#53(220.181.37.10) in 61 ms
```

拿到一条A记录，最终也就是www.baidu.com的IP地址了。
