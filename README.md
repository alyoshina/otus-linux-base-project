# Автоматизация развертывания web-стенда с системами мониторинга и логирования

Для автоматизация развертывания web-стенда используется ansible \
В проекте:
 - архитектура frontEnd/backEnd web-сервера с балансировкой нагрузки. FrontEnd - nginx, backEnd - apache
 - система мониторинга prometheus + grafana, сбор метрик организован через node_exporter, mysqld_exporter, nginx_exporter
 - система логирования ELK (elasticSearch, logstash, kibana), сбор данных filebeat
 - база данных MySQL, настроена репликация master-slave


<img src="/project_example.png?raw=true"/>


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

