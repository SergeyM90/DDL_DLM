# Домашнее задание к занятию «Работа с данными (DDL/DML)» Sergey Mironov-SYS-20  


# Задание 1

1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.  
1.2. Создайте учётную запись sys_temp.  


MySQL запускается в контейнере, образ mysql:8.0.34-debian

mysqlsh -h localhost -u root -P 3306

\option --persist defaultMode sql
\sql
CREATE USER sys_temp IDENTIFIED BY '123';
SELECT user,host FROM mysql.user;
GRANT ALL PRIVILEGES ON *.* TO sys_temp;
SHOW GRANTS FOR sys_temp;
\quit

mysqlsh -h localhost -u sys_temp -P 3306

mysqlsh -h localhost -u sys_temp -P 3306 < ./sakila-db/sakila-schema.sql 
mysqlsh -h localhost -u sys_temp -P 3306 < ./sakila-db/sakila-data.sql 


1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

![image](https://github.com/SergeyM90/DDL_DLM/assets/84016375/0cf25caf-101d-4acd-9e86-6e260d4ea6e0)


1.4. Дайте все права для пользователя sys_temp.  
1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)  

![image](https://github.com/SergeyM90/DDL_DLM/assets/84016375/82cbec08-c9d4-4537-92ef-4b5b07ce19b8)


1.6. Переподключитесь к базе данных от имени sys_temp.  
Для смены типа аутентификации с sha2 используйте запрос:  
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';  
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.  
1.7. Восстановите дамп в базу данных.  
1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)  

ER диаграмма в Dbeaver.  
![image](https://github.com/SergeyM90/DDL_DLM/assets/84016375/b3335f3c-2f19-4e54-8bd5-a6dd09f6a72c)

ER диаграмма phpmyadmin (phpmyadmin запущен в соседнем контейнере докера в одной сети с mysql)  

![image](https://github.com/SergeyM90/DDL_DLM/assets/84016375/bae21fbd-8015-4997-9148-c4d1da55d000)

Список таблиц базы данных (CLI)

![image](https://github.com/SergeyM90/DDL_DLM/assets/84016375/b0502764-4a79-49f0-b505-2c8f7f0b5684)

Список таблиц с типом

SELECT table_name, table_type, engine
FROM information_schema.tables
WHERE table_schema = 'sakila' ORDER BY table_name;

![image](https://github.com/SergeyM90/DDL_DLM/assets/84016375/2d1c7e12-a7d9-4138-bca1-09661c555388)


Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

# Задание 2

Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)

Название таблицы | Название первичного ключа
customer         | customer_id



select tab.table_schema as database_schema,
    sta.index_name as pk_name,
    sta.seq_in_index as column_id,
    sta.column_name,
    tab.table_name
from information_schema.tables as tab
inner join information_schema.statistics as sta
        on sta.table_schema = tab.table_schema
        and sta.table_name = tab.table_name
        and sta.index_name = 'primary'
where tab.table_schema = 'sakila'
    and tab.table_type = 'BASE TABLE'
order by tab.table_name,
    column_id;

    ![image](https://github.com/SergeyM90/DDL_DLM/assets/84016375/ec6b8f39-4ec8-45a1-9ba8-9cb6dc03743e)
