---
layout: page
ptitle: Network hardware and software discovery
---

Try to detect hardware equipments/nodes from your network and the software running the computer.
### 1. arp-scan
{% highlight sh %}
$ docker run -it --rm=true --privileged --net=host debian:10.1 bash
# apt update && apt install -y arp-scan && /usr/sbin/arp-scan -xl
{% endhighlight %}
will return something like:
{% highlight sh %}
192.168.0.1    64:70:02:e8:95:f0	TP-LINK TECHNOLOGIES CO.,LTD.
192.168.0.4    4c:bd:8f:77:de:ae	D-Link International
192.168.0.5    f8:3f:51:16:7a:5d	GIGA-BYTE TECHNOLOGY CO.,LTD.
192.168.0.6    7c:49:eb:a0:e3:37	(Unknown)
192.168.0.11   0e:80:62:bc:04:24	ASUSTek COMPUTER INC.
{% endhighlight %}

{% highlight json %}
{
  "network_node_cmd": "arp-scan -xl ; nmap -v -sS -O IP ; nmap -sV -sC IP",
    "interfaces_cmd": "ip addr ; lshw",
    "routes_cmd": "ip route",
    "hostname_cmd": "hostname",
    "domain_name_cmd": "cat /etc/resolv.conf",
  "storage_cmd": "lsblk -rb ; lsblk -frb",
  "os_cmd": "https://stackoverflow.com/a/10091465",
  "cpu_cmd": "cat /proc/cpuinfo",
  "system_info_cmd": "dmidecode",
  "base_board_cmd": "dmidecode",
  "chassis_info_cmd": "dmidecode",
  "bios_cmd": "dmidecode",
  "memory_cmd": "dmidecode",
  "active_connections_cmd": "netstat -tapn",
  "file_space_usage_cmd": "cd /some/path && du -bcs *",
  "crontabs_cmd": "cat /etc/crontab && for user in $(getent passwd | cut -f1 -d: ); do echo $user; crontab -u $user -l; done",
  "processes_cmd": "ps aux ; ls /proc/PID/*"
}
{% endhighlight %}

1. **CPU**: `less /proc/cpuinfo`
2. **Motherboard**: `dmidecode | grep "Base Board Information" -A 15`
3. **Video card**: `lspci | grep VGA`
4. **RAM**:
```bash
free -m | grep ^Mem
dmidecode | grep "Memory Device" -A 15
```
5. **HDD**: `smartctl -d ata -a -i /dev/sda | less`
6. **OS**:
```bash
cat /etc/redhat-release
lsb_release -a
```
7. **Monitors**:
```bash
xrandr -q --verbose | grep -w connected
cat /var/log/Xorg.0.log | grep -w Monitor -A 2
```
