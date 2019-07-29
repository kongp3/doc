## iptables 做端口转发

### 建立规则

```
$ iptables -t nat -A PREROUTING -p tcp --dport 源端口 -j REDIRECT --to-port 目标端口
```

### 查看规则

```
$ sudo iptables -t nat -L
```

### 删除规则

```
$ iptables -t nat -L --line-numbers

Chain PREROUTING (policy ACCEPT)
num  target     prot opt source               destination         
1    REDIRECT   tcp  --  anywhere             anywhere             tcp dpt:源端口 redir ports 目的端口

$  iptables -t nat -D PREROUTING 1
完成
```



