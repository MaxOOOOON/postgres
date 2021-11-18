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


Проверка репликации


Создание записи на мастера 

Проверка на слейве



Проверка работы бэкапа 
Используется rsync backup

Удаление файлов


Восстановление из бэкапа






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
