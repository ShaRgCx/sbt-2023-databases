## DragonFly
Является AP

Inconsistent. Работает асинхронно, поэтому допускается несогласовнность данных.

Availability. Данные всегда доступны.

Partition Tolerance. Масштабируется на несколько нод

## ScyllaDB
Является AP. 

Inconsistent, так как каждое изменение копирается на зеркала и может случиться ситуация, когда запрос поступил на зеркало, но новые данные не успели скопироваться с хоста, возникнет непостоянство данных.

Availability. Наличие зеркал гарантирует доступность данных в случае падения ноды.

Partition Tolerance. Может масштабироваться на несколько кластеров.
## ArenadataDB
Является CP.

Consistent. Явялется ACID-системой с уровнем изоляции транзакций - Serializable.

Unavailability. Из-за наличия транзакций, необходима согласованность данных, что может занимать время.

Partition Tolerance. Допускает распределенное хранение на нодах с зеркалами.

## Источники 
https://www.scylladb.com/glossary/cap-theorem/

https://dragonflydb.io/features

https://arenadata.tech/products/arenadata-db/

https://docs.arenadata.io/adb/index.html
