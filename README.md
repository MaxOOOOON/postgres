# postgres


## ДЗ

    репликация postgres

        настроить hot_standby репликацию с использованием слотов
        настроить правильное резервное копирование

    Для сдачи работы присылаем ссылку на репозиторий, в котором должны обязательно быть 

        Vagranfile (2 машины)
        плейбук Ansible
        конфигурационные файлы postgresql.conf, pg_hba.conf и recovery.conf,
        конфиг barman, либо скрипт резервного копирования.

    Команда "vagrant up" должна поднимать машины с настроенной репликацией и резервным копированием. 
    Рекомендуется в README.md файл вложить результаты (текст или скриншоты) проверки работы репликации и резервного копирования.
        

---

## Выполнение 

recovery.conf использовалась до 11 версии включительно, в следующих версиях настройки устанавливаются в postgresql.conf     


Проверка репликации на мастере

    SELECT * FROM pg_replication_slots;

![](https://github.com/MaxOOOOON/postgres/blob/main/pictures/Screenshot_20211118_234916.png)  

Создание таблицы на мастере

    create database test;

![](https://github.com/MaxOOOOON/postgres/blob/main/pictures/Screenshot_20211119_001020.png)  

Проверка на слейве

    SELECT * FROM pg_stat_wal_receiver;

![](https://github.com/MaxOOOOON/postgres/blob/main/pictures/Screenshot_20211118_235040.png)  

а также проверка, создалась ли таблица

    psql -c "\l"

![](https://github.com/MaxOOOOON/postgres/blob/main/pictures/Screenshot_20211119_001205.png)  

Проверка работы бэкапа 
Используется stream replication backup
Выполнить бэкап

    barman backup master

![](https://github.com/MaxOOOOON/postgres/blob/main/pictures/Screenshot_20211119_001600.png)  

Удаление базы на мастере

    drop database test;

![](https://github.com/MaxOOOOON/postgres/blob/main/pictures/Screenshot_20211119_001446.png) 

Остановка postgres

    systemctl stop postgresql-14.service

Восстановление из бэкапа    

    barman recover --remote-ssh-command "ssh postgres@192.168.255.1"  master 20211118T202912 /var/lib/pgsql/14/data/

![](https://github.com/MaxOOOOON/postgres/blob/main/pictures/Screenshot_20211119_001641.png)  

Запуск postgres

    systemctl start postgresql-14.service

Проверка восстановленной базы

![](https://github.com/MaxOOOOON/postgres/blob/main/pictures/Screenshot_20211119_001909.png)  



Заметки 

Pgtune от leopard. + есть книга 


DCS - хранит кто лидер и конфигурацию кластера  
напр. Consul + Patroni  
демон рядом с postgresql    
взаимодействует с etcd  
демон принимает решение о promotion/demotion

кластер patroni:    
consul  
haproxy 
pgbouncer   
нода Postgresql: pip install patroni, patroni.yml, dir с доступом пользов patroni 



$ barman backup server-a    
$ barman list-backup server-a   
$ barman check server-a     
$ barman show-backup server-a 20180605T053654       


barman recover --remote-ssh-command "ssh postgres@192.168.255.1"  master 20211118T202912 /var/lib/pgsql/14/data/
