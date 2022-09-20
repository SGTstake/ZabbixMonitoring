
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
