# 8.3 Описание Playbook

### group_vars
*elk_stack_versio*  версия Elasticsearch, Kibana, Filebeat

## Описание Play 

### Install Elasticsearch
#### Download Elasticsearch's rpm
 - загрузка установочного пакета 
#### Install Elasticsearch
 - установка
#### Configure Elasticsearch
 - конфигурирование с помощью шаблона

### install Kibana
#### Download Kibana's rpm
 - загрузка установочного пакета 
#### Install Kibana
 - установка
#### Configure Kibana
 - конфигурирование с помощью шаблона

### install Kibana
#### Download Filebeat's rpm
 - загрузка установочного пакета 
#### Install Filebeat
 - установка
#### Configure Filebeat
 - конфигурирование с помощью шаблона
#### Set filebeat systemwork
 - установка системной службы
#### Load Kibana dashboard
 - установка Dashboard для Kibana

Также прописаны *handlers* для перезапуска службы после выполнения каждой подзадачи.
