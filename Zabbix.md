# Домашнее задание к занятию «Система мониторинга Zabbix»

## Выполнил Савицкий Андрей

### Задание 1 

Установите Zabbix Server с веб-интерфейсом.

#### Результат:

1. скриншот авторизации в админке:
<img width="637" alt="скриншот авторизации в админке" src="https://github.com/FoxySOTKA/---/assets/141597247/f3c2073e-8c18-4833-bde2-cd80b3baa0ae">

<img width="571" alt="скриншот авторизации в админке с dashboard" src="https://github.com/FoxySOTKA/---/assets/141597247/ecc6c19f-1b3d-40cf-88fa-2c61b9cba46a">

2.  текст использованных команд для установления Zabbix 6.0 LTS на Debian 12 используя компаненты
Server, Frontend, Agent базу данных PostgreSQL и веб-сервер Apache:
a. Устанавливаем репозиторий Zabbix
 ##### wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-5+debian12_all.deb
 ##### dpkg -i zabbix-release_6.0-5+debian12_all.deb
 ##### apt update
b. Устанавливаем Zabbix сервер и веб-интерфейс
 ##### apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts 
c. Создаем базу данных 
Устанавливаем и запускаем сервер базы данных
 ##### sudo -u postgres createuser --pwprompt zabbix
 ##### sudo -u postgres createdb -O zabbix zabbix
На хосте Zabbix сервера импортируйте начальную схему и данные
 ##### zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
d. Настраиваем базу данных для Zabbix сервера
Отредактируем файл /etc/zabbix/zabbix_server.conf
##### nano /etc/zabbix/zabbix_server.conf
##### DBPassword=123456789
e. Запускаем процессы Zabbix сервера 
Запускаем процессы Zabbix сервера и настраиваем их запуск при загрузке ОС.
 ##### systemctl restart zabbix-server apache2
 ##### systemctl enable zabbix-server apache2

---

### Задание 2 

Установите Zabbix Agent на два хоста.

#### Pезультат:

1.  скриншот раздела Configuration > Hosts:
![Скриншот раздела configuration](https://github.com/FoxySOTKA/---/assets/141597247/5f3462b5-926a-4904-929c-1d4e564970ef)

2.  скриншот лога zabbix agent:
![скриншот лога zabbix agent](https://github.com/FoxySOTKA/---/assets/141597247/edf38ac5-5b09-4c2e-b135-aa560f0f9a42)

3.  скриншот раздела Monitoring > Latest data для обоих хостов:
![Cкриншот раздела Monitoring Latest data для обоих хостов](https://github.com/FoxySOTKA/---/assets/141597247/2b5535ed-140c-4c9e-a029-0d81cf33546a)

4.  текст использованных команд для установления Zabbix Agent на два хоста:
  
a. Устанавливаем Zabbix агент
 ##### apt install zabbix-agent
b. Запускаем процесс Zabbix агента
 ##### systemctl restart zabbix-agent
 ##### systemctl enable zabbix-agent
c. Настраиваем параметры подключения заббикс агента к серверу (прописываем адреса хостов)
 ##### sed -i 's/Server=127.0.0.1/Server=192.168.1.36/g' /etc/zabbix/zabbix_agent.conf
 ##### sed -i 's/Server=127.0.0.1/Server=192.168.1.33/g' /etc/zabbix/zabbix_agent.con
