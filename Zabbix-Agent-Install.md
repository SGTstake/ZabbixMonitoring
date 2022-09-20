
1. Install zabbix agent 6.0 LTS

```
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-1+ubuntu20.04_all.deb
sudo dpkg -i zabbix-release_6.0-1+ubuntu20.04_all.deb
sudo apt update

sudo apt install zabbix-agent
```

2. Configure zabbix-agent
```
sudo nano /etc/zabbix/zabbix-agentd.conf
```

Edit lines:
```
Add zabbix server ip depending using checks from zabbix server or when agent sending data to zabbix server
Server=127.0.0.1
or
ServerActive=127.0.0.1

Set vps hostname
Hostname=Zabbix server
```

Add cosmos node monitoring settings:
```
UserParameter=disk.get[*],/bin/cat /sys/class/block/$1/stat | sed -e 's/^\s*//' -e "s/[[:space:]]\+/ /g"
UserParameter=latest_block_height,curl -s localhost:26657/status | grep latest_block_height | awk '{ print substr( $2, 2, length($2)-3 ) }'
UserParameter=catching_up,curl -s localhost:26657/status | grep catching_up | awk '{ print substr( $2, 1, length($2) ) }'
UserParameter=voting_power,curl -s localhost:26657/status | grep voting_power | awk '{ print substr( $2, 2, length($2)-2 ) }'
UserParameter=moniker,curl -s localhost:26657/status | grep moniker | awk '{ print substr( $2, 2, length($2)-3 ) }'
UserParameter=chainid,curl -s localhost:26657/status | grep network | awk '{ print substr( $2, 2, length($2)-3 ) }'
UserParameter=n_peers,curl -s localhost:26657/net_info | grep n_peers | awk '{ print substr( $2, 2, length($2)-3 ) }'
```
