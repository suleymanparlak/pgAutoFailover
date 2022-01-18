# pgAutoFailover

Cluster’ımız bir monitör ve üç PostgreSQL olmak üzere dört node’dan oluşacaktır. Konfigürasyon işlemlerinde kullanılmak üzere node’ların hostname ve ip’leri aşağıda belirtilmiştir.

![image](https://user-images.githubusercontent.com/72556168/150009622-af860f1c-2d75-4c3e-85eb-9011ea3d4001.png)

Tüm node’larda PostgreSQL ve PG_Auto_Failover kurulumu için aşağıdaki komutları çalıştırıyoruz.

- curl https://install.citusdata.com/community/deb.sh | sudo bash

- sudo apt-get install postgresql-13-auto-failover-1.5

# Monitor Node (pgcom1)

- sudo su - postgres

- export PGDATA=./monitor

- pg_autoctl create monitor --ssl-self-signed --hostname 10.8.130.171 --auth trust –run
 
Monitör node’unun çalıştığını gördükten sonra ctrl+c ile kapatılır.
 
Postgres kullanıcısından çıkalım ve aşağıdaki komutları yazalım.

 - sudo pg_autoctl -q show systemd --pgdata /var/lib/postgresql/monitor | sudo tee /etc/systemd/system/pgautofailover.service

 - sudo systemctl daemon-reload

 - sudo systemctl enable pgautofailover

 - sudo systemctl start pgautofailover

 - sudo systemctl status pgautofailover

# Node2 (pgcom2)

 - sudo su – postgres

 - export PGDATA=./data

 - pg_autoctl create postgres --hostname 10.8.131.103 --auth trust --ssl-self-signed --monitor 'postgres://autoctl_node@10.8.130.171:5432/pg_auto_failover?sslmode=require’--run

  Servisin çalıştığını teyit ettikten sonra ctrl+c ile kapatılır.

  Postgres kullanıcısından çıkalım ve komutlarımızı yazalım.

 - sudo pg_autoctl -q show systemd --pgdata /var/lib/postgresql/data | sudo tee /etc/systemd/system/pgautofailover.service

 - sudo systemctl daemon-reload

 - sudo systemctl enable pgautofailover

 - sudo systemctl start pgautofailover

 - sudo systemctl status pgautofailover

# Node3 (pgcom3)

 - sudo su – postgres
 
 - export PGDATA=./data
  
 - pg_autoctl create postgres --hostname 10.8.131.118 --auth trust --ssl-self-signed --monitor 'postgres://autoctl_node@10.8.130.171:5432/pg_auto_failover?sslmode=require' –run 

 Servisin çalıştığını teyit ettikten sonra ctrl+c ile kapatılır.

 Postgres kullanıcısından çıkalım ve komutlarımızı yazalım.

 - sudo pg_autoctl -q show systemd --pgdata /var/lib/postgresql/data | sudo tee /etc/systemd/system/pgautofailover.service

 - sudo systemctl daemon-reload

 - sudo systemctl enable pgautofailover

 - sudo systemctl start pgautofailover

 - sudo systemctl status pgautofailover

# Node4 (pgcom4)

 - sudo su – postgres

 - export PGDATA=./data

 - pg_autoctl create postgres --hostname 10.8.132.238 --auth trust --ssl-self-signed --monitor 'postgres://autoctl_node@10.8.130.171:5432/pg_auto_failover?sslmode=require' –run

 Servisin çalıştığını teyit ettikten sonra ctrl+c ile kapatılır.

 Postgres kullanıcısından çıkalım ve komutlarımızı yazalım.

 - sudo pg_autoctl -q show systemd --pgdata /var/lib/postgresql/data | sudo tee /etc/systemd/system/pgautofailover.service

 - sudo systemctl daemon-reload

 - sudo systemctl enable pgautofailover

 - sudo systemctl start pgautofailover
 
 - sudo systemctl status pgautofailover


# Cluster kurulumu tamamlandı.

Monitör node’undan cluster durumuna bakalım. Bunun için Postgres kullanıcısına bağlanıyoruz.

export PGDATA=./monitor

![image](https://user-images.githubusercontent.com/72556168/150010171-e1f77278-fcb0-4f26-83f4-ac9309daa661.png)

Monitor node’unda connection string elde edelim.

![image](https://user-images.githubusercontent.com/72556168/150010192-34e104ae-97b3-41be-9268-19d38c78a054.png)






