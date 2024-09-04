# Домашнее задание к занятию «Работа с данными (DDL/DML)»

Инструкция по выполнению домашнего задания

Сделайте fork репозитория c шаблоном решения к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).

Выполните клонирование этого репозитория к себе на ПК с помощью команды git clone.

Выполните домашнее задание и заполните у себя локально этот файл README.md:

впишите вверху название занятия и ваши фамилию и имя;

в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;

для корректного добавления скриншотов воспользуйтесь инструкцией «Как вставить скриншот в шаблон с решением»;

при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в инструкции по MarkDown.

После завершения работы над домашним заданием сделайте коммит (git commit -m "comment") и отправьте его на Github (git push origin).

Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.

Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

1.2. Создайте учётную запись sys_temp.

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

1.4. Дайте все права для пользователя sys_temp.

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос:
```
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

```

1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

### Ответ 1

1.1 MySQL версии 8.0.32-debian с помощью Docker
```
docker run -dp 3306:3306 \
--name netology-mysql \
-v mysql:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=netology \
mysql:8.0.32-debian
```
1.2. Создайте учётную запись sys_temp.
```
docker exec -it netology-mysql mysql -uroot -p

CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY 'netology';
```

1.3. Запрос на получение списка пользователей в базе данных
```
SELECT user FROM mysql.user;
```
![alt text](https://github.com/Daark46/DDL-DML/blob/main/1.3.png)

1.4.Все права для пользователя sys_temp
```
GRANT ALL PRIVILEGES ON mysql . * TO 'sys_temp'@'localhost';
```
1.5. Запрос на получение списка прав для пользователя sys_temp
```
SHOW GRANTS FOR 'sys_temp'@'localhost';
```
![alt text](https://github.com/Daark46/DDL-DML/blob/main/1.5.png)

1.6. Переподключение к базе данных от имени sys_temp
```
ALTER USER 'sys_temp'@'localhost' IDENTIFIED WITH mysql_native_password BY 'netology';

SELECT user();
```
По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных

```
wget https://downloads.mysql.com/docs/sakila-db.zip

unzip sakila-db.zip
```

1.7. Восстановление дампа в базу данных
```
docker cp ./sakila-db/sakila-schema.sql netology-mysql:/tmp

docker cp ./sakila-db/sakila-data.sql netology-mysql:/tmp

docker exec -it netology-mysql bash

mysql -u root -p sakila < /tmp/sakila-schema.sql 

mysql -u root -p sakila < /tmp/sakila-data.sql
                                          
exit
```
1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)
```
mysql> SHOW DATABASES;

mysql> USE sakila

mysql> SHOW TABLES;
```
![alt text](https://github.com/Daark46/DDL-DML/blob/main/1.8.png)

### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)

```
Название таблицы | Название первичного ключа
customer         | customer_id
```

### Ответ 2
```
mysql>  SELECT TABLE_NAME, COLUMN_NAME FROM INFORMATION_SCHEMA.key_column_usage WHERE table_schema = 'sakila' AND CONSTRAINT_NAME = 'PRIMARY';
```
![alt text](https://github.com/Daark46/DDL-DML/blob/main/2.png)


