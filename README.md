#  Дипломная работа по профессии «Системный администратор»

Содержание
==========
* [Задача](#задача)
* [Инфраструктура](#инфраструктура)
    * [Сайт](#сайт)
    * [Логи](#логи)
    * [Мониторинг](#мониторинг)
    * [Сеть](#сеть)
    * [Резервное копирование](#резервное-копирование)

---------
## Задача
Ключевая задача — разработать отказоустойчивую инфраструктуру для сайта, включающую мониторинг, сбор логов и резервное копирование основных данных. Инфраструктура должна размещаться в [Yandex Cloud](https://cloud.yandex.com/).


## Инфраструктура
Для развёртки инфраструктуры используем [Terraform](./terraform), а для установки ПО [Ansible](./ansible).

Terraform:

Проверяем правильность синтаксиса файлов Terraform "terraform validate"

![Скриншот-1](./img/dip_1.jpg) 

Просматриваем план создание инфраструктуры "terraform plan"

![Скриншот-2](./img/dip_2.jpg)

И запускаем "terraform apply"

![Скриншот-3](./img/dip_3.1.jpg)

Дожидаемся окончания создания инфраструктуры и вывода информации output

![Скриншот-4](./img/dip_3.jpg)

Заранее подготовленный [output-ansible-hosts](./terraform/output.tf) сохраняем в отдельный файл hosts для ansible и удаляем лишнее из файла

![Скриншот-5](./img/dip_3.2.jpg)

Проверяем доступность созданных ВМ с помощью ansible all -m ping

![Скриншот-6](./img/dip_4.jpg)

Переходим к установке и настройке ПО с помощью Ansible

### Сайт
Запускаем playbook - [webservers-playbook.yml](./ansible/webservers-playbook.yml)

По окончанию выполнения playbook проверяем что сервисы nginx, node_exporter и nginx_logexporter работаю

![Скриншот-7](./img/dip_5.jpg)

Проверяем что наш сайт работает

![Скриншот-8](./img/dip_6.jpg)

![Скриншот-9](./img/dip_7.jpg)

### Логи
Запускаем playbook - [log-playbook.yml](./ansible/log-playbook.yml)

По завершению выполнения playbook проверяем что контейнеры с elasticsearch и kibana работают

![Скриншот-10](./img/dip_8.jpg)

Уставноку filebeat - [log-filebeat-playbook.yml](./ansible/log-filebeat-playbook.yml)

![Скриншот-11](./img/dip_9.jpg)

Заходим в Kibana для проверки что логи nginx с web серверов поступают

![Скриншот-12](./img/dip_10.jpg)

### Мониторинг
Запускаем playbook - [monitoring-playbook.yml](./ansible/monitoring-playbook.yml)

Проверяем что работа playbook завершилась без ошибок

![Скриншот-13](./img/dip_11.jpg)

Заходим в Grafana для проверки работоспособности наших ранее импортированных дашбордов 

NGINX Servers Metrics:

![Скриншот-14](./img/dip_12.jpg)

Node Exporter Full:

![Скриншот-15](./img/dip_13.jpg)

### Сеть

![Скриншот-16](./img/dip_14.jpg)

![Скриншот-17](./img/dip_15.jpg)

![Скриншот-18](./img/dip_16.jpg)

### Резервное копирование

![Скриншот-19](./img/dip_17.jpg)

