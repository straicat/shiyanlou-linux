# Linux DHCP服务部署与配置

DHCP用于动态分配IP地址与网关等参数。

DHCP工作流程

安装dhcp：

``` Bash
# CentOS
$ sudo yum install dhcp
# Ubuntu
$ sudo apt-get install isc-dhcp-server
```

配置文件：`/etc/dhcp/dhcpd.conf`

基本配置示例：

```
# 注意不要遗漏结尾处的分号(;) 
# 根据IP，配置子网和掩码
subnet 10.29.113.0 netmask 255.255.255.0 {
  # 分配的地址段（可以有多个）
  range 10.29.113.1 10.29.113.100;
  # 配置网关
  option routers 10.29.113.1;

  # 绑定静态地址
  host test {
    hardware ethernet 02:42:c0:a8:00:03;
    fixed-address 10.29.113.2;
  }
}
```

重启`dhcpd`来使得配置生效。

---

配置DHCP客户端：

```
# eth0改为相应的接口名
# CentOS
# /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
ONBOOT=yes
BOOTPROTO=dhcp
# Ubuntu
# /etc/network/interfaces
auto eth0
iface eth0 inet dhcp
```

然后重启网络服务（`networking`）生效。


```

