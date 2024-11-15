# Автоматизация развертывания web-стенда с системами мониторинга и логирования

Для автоматизация развертывания web-стенда используется ansible \
В проекте:
 - архитектура frontEnd/backEnd web-сервера с балансировкой нагрузки. FrontEnd - nginx, backEnd - apache
 - система мониторинга prometheus + grafana, сбор метрик организован через node_exporter, mysqld_exporter, nginx_exporter
 - система логирования ELK (elasticSearch, logstash, kibana), сбор данных filebeat
 - база данных MySQL, настроена репликация master-slave


<img src="/project_example.png?raw=true"/>

## Требования
Для запуска playbook требуется root-доступ, используется с параметром «become: yes»

## Переменные
Доступные переменные перечислены ниже вместе со значениями по умолчанию (см. `defaults/main.yml` в ролях)
```yaml
nginx_port: 80
apache_port: 4444
```

```yaml
mysql_daemon: mysql
dbname: testdb
mysql_root_user: root
mysql_root_password: 
mysql_repl_user: repl

mysqld_exporter_user: mysqld_exporter
mysqld_exporter_password: 
```

```yaml
kibana_port: 5601
```

```yaml
prometheus_port: 9090
node_exporter_port: 9100
mysql_exporter_port: 9101
nginx_exporter_port: 9102

grafana_port: 3000
grafana_user: "admin"
grafana_password: "admin"
```
```yaml
elk_deb_dir: /home/liliya/elk-8.9-deb/
grafana_deb_path: /home/liliya/grafana_11.2.2_amd64-224190-c9d6aa.deb
```

## Подготовка

### На машинах установлен static ip
Пример конфиг файла:
```yaml
network:
  ethernets:
    enp0s3:
      dhcp4: false
      addresses: [<static_ip>/24]
      routes:
        - to: default
          via: <route_ip>
      nameservers:
        addresses:
          - 8.8.8.8
```
### Настроена аутентификация по ssh-ключам
Для доступа узла управления с ansible к хост-машинам.
```
ssh-keygen -t rsa -C front

ssh-copy-id <user>@<ip>
```

### На машине для запуска восстановления
Директория elk-8.9-deb с пакетами для elk и grafana_11.2.2.deb \
Устанолен ansible версии 10.6.0 \
Команды для установки последней версии ansible
```
apt update
apt upgrade -y
add-apt-repository --yes --update ppa:ansible/ansible
apt install ansible
```
В github добавлен открытый ssh-ключ

-------------------------------

## Восстановление

Получение копии git-репозитория:
```
git clone git@github.com:alyoshina/otus-linux-base-project.git
```
Из директории с проектом запустить:
```
ansible-playbook main.yml --extra-vars "@vars.yml" -kK --ask-vault-pass
```

