# Домашняя работа 2 по MongoDB
## 1. Установка
Установим mongodb в докер, скачав образ с официального сайта, прокинем другой порт, так как 27017 по какой-то причине оказывается уже занят
![изображение](https://user-images.githubusercontent.com/78553805/229347101-a97c0081-e3a9-49ba-aab3-a342e593ed9c.png)
Зайдем в MongoDB Shell и првоерим, что сервер действительно активен и в текущий момент база данных пуста
![изображение](https://user-images.githubusercontent.com/78553805/229347496-b07bc8bb-0fd0-4ace-b20a-ceedbed43d64.png)
## 2. Вставка данных
Скачаем датасет [Mall_Customers](https://www.kaggle.com/datasets/shwetabh123/mall-customers?resource=download) и заполним базу данными:
![изображение](https://user-images.githubusercontent.com/78553805/229348246-26a38fb2-f102-4516-97a7-160cf93196ab.png)
Проверим в MongoDB Compass, что у нас появилась новая коллекция с базой, заполненной данными:
![изображение](https://user-images.githubusercontent.com/78553805/229348289-e566ecc5-8685-4b7c-8df2-b0b6d6030998.png)
![изображение](https://user-images.githubusercontent.com/78553805/229348331-d4202aea-fce4-4c92-bb08-99107ce2f31d.png)

Загрузим другой [датасет](https://www.reddit.com/r/datasets/comments/1uyd0t/200000_jeopardy_questions_in_a_json_file/)(на этот раз .json) с бОльшим количеством данных для проведения тестов на производительность
![изображение](https://user-images.githubusercontent.com/78553805/229354907-96367753-20ef-4a0f-bf86-f9b5dfb7c354.png)
![изображение](https://user-images.githubusercontent.com/78553805/229354941-9776813c-4ea5-49a8-839e-eb3f7d35ab6f.png)
## 3. Выборка данных

Посмотрим на все вопросы, со стоимостью в $200:
![изображение](https://user-images.githubusercontent.com/78553805/229355040-3f802bbc-ec65-40e4-a22f-25ae00d11ce4.png)

Посмотрим на вопросы, со стоимостью в $200 или $400, из категории History и раунда Jeopardy!:
![изображение](https://user-images.githubusercontent.com/78553805/229355655-ab9f2661-ab22-48ac-b029-39122b6720d7.png)

Добавим новые вопросы в игру:
![изображение](https://user-images.githubusercontent.com/78553805/229356064-abb9ce5a-51ad-4baa-92aa-37b7f732f0c1.png)

Посмотрим, что наши вопросы действительно добавились:
![изображение](https://user-images.githubusercontent.com/78553805/229356118-d5880c99-8389-4087-9a9e-571a5fb8c81c.png)

Обновим стоимость одного из вопросов:
![изображение](https://user-images.githubusercontent.com/78553805/229358714-39126dd8-962c-42f6-b0d9-0f7a7493ed5f.png)

Удалим все наши добавленные вопросы из базы:
![изображение](https://user-images.githubusercontent.com/78553805/229358773-0f444796-68bd-46a9-9e3d-cf124f06152c.png)
## 4. Анализ производительности
Посмотрим на все вопросы из категории History с анализом производительности:
![изображение](https://user-images.githubusercontent.com/78553805/229358904-d1a4cc46-a2da-4481-b6b6-b08429c38b0a.png)
![изображение](https://user-images.githubusercontent.com/78553805/229358915-c572ab91-8d35-4596-a02e-f4ccabb491fc.png)

Добавим индексы по категориям и выполним запрос еще раз:
![изображение](https://user-images.githubusercontent.com/78553805/229359308-efdb3630-4571-4530-9926-e88fca6fea58.png)

Можем видеть, что добавление индексов значительно повысило производительноть запроса значительно увеличилась (executionTime уменьшился в 50+ раз)

