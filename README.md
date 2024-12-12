# Репозиторий pymongo-api

Репозиторий содержит стуктуру директорий c вариантами реализаций приложения и структурную схему cheme.drawio. 
Результатом запуска является развертывание сервиса управления данными на http://<ip виртуальной машины>:8080.

## Задание1. Проектирование
На основе шаблона необходимо создать схему итогового решения.

1. Схема с шардированием с двумя шардами MongoDB  https://disk.yandex.ru/d/bF0TX8WzjefvVA 
2. Репликацию MongoDB  https://disk.yandex.ru/d/PKYlawIhY0Q9Lw
3. Кэширование с Redis https://disk.yandex.ru/d/SPwsyLQ0cYrtsg 

Для каждого этапа представлены отдельные схемы draw.io


## Заданиея 2,3 и 4. Реализация шардирования, репликации и кэширования
 
Репозиторий содержит стуктуру директорий, в каждой из которых находятся исполняемые файлы запуска compose.yaml приложения, 
в соответствии с заданиями 2,3 и 4.

```sh
/mongo-sharding
  /scripts
  compose.yaml
/mongo-sharding-repl
  /scripts
  compose.yaml
/sharding-repl-cache
  /scripts
  compose.yaml
```
В соотвествующих директориях /scrips находятся исполняемые файлы для запуска приложений.

### Как запустить

Необходимо перейти в директорию с соответвующим compose.yaml файлом в корне

```shell
cd sharding-repl-cache
```

Запустить формирование контейнеров

```shell
docker compose up -d
```

Инициазировать инстансы, запустив скрипт

```shell
./scripts/mongo-init.sh
```

Наполнить данными и вывести результат в консоль, запустить скрипт

```shell
./scripts/load-output.sh
```

Результат запуска в консоли

```shell
[direct: mongos] somedb> NOTE: helloDoc count=1000
[direct: mongos] somedb> shard1 [direct: primary] test> OUTPUT: shard1 repl count=3
shard1 [direct: primary] test> shard1 [direct: primary] test> switched to db somedb
shard1 [direct: primary] somedb> NOTE: shard1_db1 doc count=492
shard1 [direct: primary] somedb> shard1 [direct: secondary] test> switched to db somedb
shard1 [direct: secondary] somedb> NOTE: shard1_db2 doc count=492
shard1 [direct: secondary] somedb> shard1 [direct: secondary] test> switched to db somedb
shard1 [direct: secondary] somedb> NOTE: shard1_db3 doc count=492
shard1 [direct: secondary] somedb> shard2 [direct: primary] test> 
shard2 [direct: primary] test> OUTPUT: shard2 repl count=3
shard2 [direct: primary] test> shard2 [direct: primary] test> switched to db somedb
shard2 [direct: primary] somedb> NOTE: shard2_db1 doc count=508
shard2 [direct: primary] somedb> shard2 [direct: secondary] test> switched to db somedb
shard2 [direct: secondary] somedb> NOTE: shard2_db2 doc count=508
shard2 [direct: secondary] somedb> shard2 [direct: secondary] test> switched to db somedb
shard2 [direct: secondary] somedb> NOTE: shard2_db3 doc count=508
```

При необходимости очистить базу

```shell
./scripts/db-drop.sh
```

### Если вы запускаете проект на локальной машине

Откройте в браузере http://localhost:8080

### Если вы запускаете проект на предоставленной виртуальной машине

Узнать белый ip виртуальной машины

```shell
curl --silent http://ifconfig.me
```

Откройте в браузере http://<ip виртуальной машины>:8080

```sh
{
  "mongo_topology_type": "Sharded",
  "mongo_replicaset_name": null,
  "mongo_db": "somedb",
  "read_preference": "Primary()",
  "mongo_nodes": [
    [
      "mongos_router",
      27020]
  ],
  "mongo_primary_host": null,
  "mongo_secondary_hosts": [],
  "mongo_address": [
    "mongos_router",
    27020],
  "mongo_is_primary": true,
  "mongo_is_mongos": true,
  "collections": {
    "helloDoc": {
      "documents_count": 1000
    }
  },
  "shards": {
    "shard1": "shard1/shard1_db1:27018,shard1_db2:27028,shard1_db3:27038",
    "shard2": "shard2/shard2_db1:27019,shard2_db2:27029,shard2_db3:27039"
  },
  "cache_enabled": true,
  "status": "OK"
}
```

### Доступные эндпоинты

Список доступных эндпоинтов, swagger http://<ip виртуальной машины>:8080/docs

## Задание5. Service Discovery и балансировка с API Gateway

В результате выполнения задания составлен вариант схемы с горизонтальным масштабированием приложения.

https://disk.yandex.ru/d/icYvV0iHpHIigw

Добавлен сервис API Gateway для балансировки и Consul для реализации Service Discovery.


## Задание6. Service Discovery и балансировка с API Gateway
В результате выполнения задания составлен вариант  схемы, на котором реализовано использование CDN. 

https://disk.yandex.ru/d/87Zz_08JEPeOqA

Добавлен облачный сервис CNN.


## Итоговая схема

Итоговая структурна схема  https://disk.yandex.ru/d/Wc8xrvjQJKvTYQ


