# 8.2 Описание Playbook

### group_vars
*java_oracle_jdk_package* - имя пакета установки Java  
*java_jdk_version* - используемая версия Java  
*elastic_home* - переменная домашнего каталога для Elasticsearch  
*kibana_home* - переменная для домашнего каталога для Kibana  
*elastic_version* - версия Elasticsearch  
*kibana_version* - версия Kibana  

## Описание Play 

### Install Java
#### Set facts for Java 11 vars
 - установка переменных facts java_home
#### Upload .tar.gz file containing binaries from local storage
 - загрузка установочного пакета из /files
#### Ensure installation dir exists
 - создание рабочего каталога
#### Extract java in the installation directory
 - распаковка пакета
#### Export environment variables
 - экспорт переменных окружений из templates  

На каждую подзадачу установлен тэг *java*
### Install Elasticsearch
#### Upload tar.gz Elasticsearch from remote URL
 - загрузка установочного пакета 
#### Create directrory for Elasticsearch
 - создание рабочего каталога
#### Extract Elasticsearch in the installation directory
 - распаковка пакета
#### Set environment Elastic
 - экспорт переменных окружений из templates

На каждую подзадачу установлен тэг *elastic*
### install Kibana
#### Upload tar.gz Kibana from remote URL
 - загрузка установочного пакета 
#### Create directrory for Kibana ({{ kibana_home }})
 - создание рабочего каталога
#### EExtract Kibana in the installation directory
 - распаковка пакета
#### Set environment Kibana
 - экспорт переменных окружений из templates

На каждую подзадачу установлен тэг *kibana*