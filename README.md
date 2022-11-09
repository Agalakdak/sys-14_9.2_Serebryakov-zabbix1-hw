# Домашнее задание к занятию "9.2. Zabbix. Часть 1" - `Серебряков Руслан`

### Задание 1

####Установите Zabbix Server с веб-интерфейсом.

Приложите скриншот авторизации в админке. Приложите текст использованных команд в GitHub.
---
![Скриншот авторизации в админке] ./img/zabbix_login.jpeg

---
Для установки Zabbix-server, я использовал команды представленные в лекции + сравнивал их с командами предложенными на официальном сайте.

Сначала устанавливаем базу данных postgresql
sudo apt install postgresql

Добавляем репозиторий в текущую папку
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb
Устанавливаем пакет ".deb" который только что скачали.
dpkg -i zabbix-release_6.0-4+debian11_all.deb
Обновляем список репозиториев
apt update

Устанавливаем необходимые пакеты для zabbix-server
sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts nano -y

Создаём пользователя базы данных 
su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD '\'123456789\'';"

Создаём базу данных 
su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"

Далее как я понял мы загружаем скрипт для создания базы данных 
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix 

Ставим пароль для доступа к базе данных (ну по факту, просто прописываем его в DBPassword=***)
sudo nano /etc/zabbix/zabbix_server.conf

Перезапускаем службы
sudo systemctl restart zabbix-server apache2 # zabbix-agentsudo 
Добавляем в автозапуск
systemctl enable zabbix-server apache2 # zabbix-agent
---

### Задание2 

#### Установите Zabbix Agent на два хоста.
#### Приложите скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу. Приложите скриншот лога zabbix agent, где видно, что он работает с сервером. Приложите скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные. Приложите текст использованных команд в GitHub.

![Раздел Configuration > Hosts] ./img/zabbix_hosts.jpeg

![Лог zabbix-agent vm1] ./img/zabbix_host_vm1_log_agent.jpeg
![Лог zabbix-agent nout] ./img/zabbix_host_nout_log_agent.jpeg

![Monitoring > latest data vm1 и nout] ./img/zabbix_monitoring_latest_data.png
![Monitoring > latest nout] ./img/zabbix_monitoring_latest_data_more_info.png
![Лог агента vm1] ./img/zabbix_host_vm1_log_agent.jpeg
![Лог агента nout] ./img/zabbix_host_nout_log_agent.jpeg
 
























