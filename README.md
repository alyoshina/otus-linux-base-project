# otus-linux-base-project

## Подготовка

### На машинах установлен static ip
Пример конфиг файла:
```yaml
network:
  ethernets:
    enp0s3:
      dhcp4: false
      addresses: [192.168.0.24/24]
      routes:
        - to: default
          via: 192.168.0.1
      nameservers:
        addresses:
          - 8.8.8.8
```
### Настроена аутентификация по ssh-ключам
```
ssh-keygen -t rsa -C front

ssh-copy-id <user>@<ip>
```

### На машине для запуска восстановления
Директория elk-8.9-deb с пакетами для elk и grafana_11.2.2.deb / 
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









  version: 2
