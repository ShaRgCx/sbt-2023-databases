## Установка Redis
Установим и запустим в докере Редис:

![image](https://user-images.githubusercontent.com/78553805/234354265-96f9370a-9d91-4293-8a2d-34a5561977fb.png)

Установим и подключимся в Redisinsight. Проверим, что стандартные команды выполняются:

![image](https://user-images.githubusercontent.com/78553805/234354623-da0c815d-6f22-44c5-b00c-b91fe298f1fa.png)

## Проверка производительности
Сгенерируем при помощи [сайта](https://json-generator.com/) большой json файл для последующей загрузки в нашу базу данных. 

Согласно следующему python скрипту проверим время выполнения запросов на наших данных:
```python
import json
import time
import redis

with open('generated.json') as f:
    js = json.load(f)

db = redis.StrictRedis(
    host='172.17.0.2',
    port=6379
)

def set_string(db, js, it):
    total_time = 0
    for i in range(it):
        start = time.time()
        db.set('string' + str(i), json.dumps(js))
        end = time.time()
        total_time += end - start
    return total_time / it

def set_hset(db, js, it):
    total_time = 0
    for i in range(it):
        start = time.time()
        db.hset('hset' + str(i), str(i), json.dumps(js))
        end = time.time()
        total_time += end - start
    return total_time / it

def set_zset(db, js, it):
    zset_js = {}
    total_time = 0
    for i, el in enumerate(js):
        zset_js[json.dumps(el)] = i
    zset_ttl = 0
    for i in range(it):
        start = time.time()
        zset_ttl = db.zadd('zset' + str(i), zset_js)
        end = time.time()
        total_time += end - start
    return total_time / it, zset_ttl

def set_list(db, js, it):
    total_time = 0
    list_ttl = 0
    list_js = []
    for el in js:
        list_js.append(json.dumps(el))
    for i in range(it):
        start = time.time()
        list_ttl = db.lpush('list' + str(i), *list_js)
        end = time.time()
        total_time += end - start
    
    return total_time / it, list_ttl
    
it = 10    
zset_time, zset_ttl = set_zset(db, js, it)
list_time, list_ttl = set_list(db, js, it)

def read_string(db, it):
    total_time = 0
    for i in range(it):
        start = time.time()
        db.get('string' + str(i))
        end = time.time()
        total_time += end - start
    return total_time / 10

def read_hset(db, it):
    total_time = 0
    for i in range(it):
        start = time.time()
        db.hget('hset' + str(i), str(i))
        end = time.time()
        total_time += end - start
    return total_time / it

def read_zset(db, it, zset_ttl):
    total_time = 0
    for i in range(it):
        start = time.time()
        db.zrange('zset' + str(i), 0, zset_ttl)
        end = time.time()
        total_time += end - start
    return total_time / it

def read_list(db, it, list_ttl):
    total_time = 0
    for i in range(it):
        start = time.time()
        db.lrange('list' + str(i), 0, list_ttl)
        end = time.time()
        total_time = end - start
    return total_time / it
    
print('STRING set time is:', set_string(db, js, it) ,'s')
print('HSET time is:', set_hset(db, js, it) ,'s')
print('ZSET time is:', zset_time, 's')
print('LIST set time is:', list_time,'s')
print('STRING read time is:', read_string(db, it) ,'s')
print('HREAD time is:', read_hset(db, it) ,'s')
print('ZRANGE time is:', read_zset(db, it, zset_ttl), 's')
print('LIST read time is:', read_list(db, it, list_ttl),'s')

db.flushall()
```

Получим следующие результаты:

![image](https://user-images.githubusercontent.com/78553805/234377361-27371c1f-5123-498b-b01b-ab094ce4e0f3.png)

Кстати, вот наши красивые данные в RedisInsight:

![image](https://user-images.githubusercontent.com/78553805/234378876-bfcdaac8-ccd1-4403-a4c1-f1190696852d.png)


## Микровывод
Исходя из полученных данных, можем сделать вывод о том, что лучше всего в тестах на запись и чтение себя показывает list (что неудивительно). 
При этом хуже всего с записью справился HSET, а с чтением STRING.

## Настройка кластера на 3х нодах с отказоустойчивостью и тюн таймаутов

Чтобы не выдумывать велосипед, воспользуемся готовым решением с [гитхаба](https://github.com/vishnudxb/docker-redis-cluster). 

Создаем кластер следующим образом: 

![image](https://user-images.githubusercontent.com/78553805/234379798-060fa709-51c4-46ce-acd2-97ed02c523a2.png)

Для редактирования таймаутов, можнм воспользоваться redis-cli и зайдя на конфиг нужной нойды. Например, вот так:

![image](https://user-images.githubusercontent.com/78553805/234380497-1821909d-0e09-4231-bf32-3649d8060b8b.png)

Убедимся, что все работает:

![image](https://user-images.githubusercontent.com/78553805/234381551-57e8ff6a-0b17-410c-a909-a51e9afe9ed7.png)


Затюним таймауты, подключившись к необходимой ноде, и изменив файл redis.conf
