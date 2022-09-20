
#1. Install Zabbix server, frontend, and agent

Zabbix 6.0 LTS version (supported until February, 2027)
```wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-3+ubuntu$(lsb_release -rs)_all.deb
sudo dpkg -i zabbix-release_6.0-3+ubuntu$(lsb_release -rs)_all.deb
sudo apt update
sudo apt -y install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```
#2. Configure database
```
sudo apt install software-properties-common -y
curl -LsS -O https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
sudo bash mariadb_repo_setup --mariadb-server-version=10.8

sudo apt update
sudo apt -y install mariadb-common mariadb-server-10.8 mariadb-client-10.8
```
```
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
#3. Prepare databse for Zabbix
```
sudo mysql_secure_installation
```
```
Enter current password for root (enter for none): Press Enter
Switch to unix_socket authentication [Y/n] y
Change the root password? [Y/n] y
New password: <Enter root DB password>
Re-enter new password: <Repeat root DB password>
Remove anonymous users? [Y/n]: Y
Disallow root login remotely? [Y/n]: Y
Remove test database and access to it? [Y/n]:  Y
Reload privilege tables now? [Y/n]:  Y
```

Create database:
```
sudo mysql -uroot -p'rootDBpass' -e "create database zabbix character set utf8mb4 collate utf8mb4_bin;"
sudo mysql -uroot -p'rootDBpass' -e "grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbixDBpass';"
```
Import schema and data
```
sudo zcat /usr/share/doc/zabbix-sql-scripts/mysql/server.sql.gz | mysql -uzabbix -p'zabbixDBpass' zabbix
```
#4. Enter database password in zabbix configuration file
```
sudo nano /etc/zabbix/zabbix_server.conf

DBPassword=zabbixDBpass
```
5. Configure firewall
```
ufw allow 10050/tcp
ufw allow 10051/tcp
ufw allow 80/tcp
ufw reload
```
#6. Start zabbix server and agent processes
```
sudo systemctl restart zabbix-server zabbix-agent
sudo systemctl enable zabbix-server zabbix-agent
```
#7. Configure PHP for Zabbix frontend
```
sudo nano /etc/zabbix/apache.conf

php_value date.timezone Europe/Amsterdam


sudo systemctl restart apache2
sudo systemctl enable apache2
```
#8. Configure web frontend

Connect to your newly installed Zabbix frontend using URL “http://server_ip_or_dns_name/zabbix” to initiate the Zabbix installation wizard.
