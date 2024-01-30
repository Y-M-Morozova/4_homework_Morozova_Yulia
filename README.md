**<div align="center"><h2>Морозова Юлия, OTUS, группа Postgre-DBA-2023-11</h2></div>**

**<div align=center><h3>Домашнее задание №4 по теме:</h3></div>**
**<div align=center><h3>«Логический уровень PostgreSQL. Работа с базами данных, пользователями и правами.»</h3></div>**

***

**<h3>Цель:
<br>создание новой базы данных, схемы и таблицы
<br>создание роли для чтения данных из созданной схемы созданной базы данных
<br>создание роли для чтения и записи из созданной схемы созданной базы данных.</h3>**

***
**Выполнение:**

>**1. создайте новый кластер PostgresSQL 14**

cогласно заданию, создаю новый кластер PostgresSQL 14 версии. Буду устанавливать на одной из своих ВМ в ЯО, где уже стоит  кластер PostgresSQL 15 версии,
устанавливаю командой:
``sudo apt install postgresql-14``

логинюсь пользователем postgres ``sudo su postgres`` и проверяю кластеры командой ``pg_lsclusters`` , все ок: порт 5432 версия 15я, а на порту 5433 - версия PostgresSQL 14:

  ![1_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/15fcb9ff-6c9e-4ea0-a278-2a489fdcebf9)

<br/>

>**2. зайдите в созданный кластер под пользователем postgres**

подключаюсь по порту 5433(по которому сервер 14й версии прослушивает соединения):

  ![2_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/eeeb0883-5400-453e-bc31-af8f96c0270e)

<br/>

>**3. создайте новую базу данных testdb**

создаю новую БД ``testdb`` командой: 
```sql 
    CREATE DATABASE testdb;
```

  ![3_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/f64ca04a-c94b-425a-9a7a-5ffbdcd56cde)

  <br/>

>**4. зайдите в созданную базу данных под пользователем postgres**

подключаюсь к БД ``testdb`` как ``postgres``:

  ![4_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/eeabe3dc-a746-49ac-81a6-dde4f0bb80d4)

  <br/>

>**5. создайте новую схему testnm**

создаю схему командой:
```sql
  CREATE SCHEMA testnm;
```

и потом проверяю, все ок:

  ![5_11](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/21c29dd5-bc3f-4b1c-b31a-8003842495bb)

<br/>
  
>**6. создайте новую таблицу t1 с одной колонкой c1 типа integer**

тут задумалась - так как согласно заданию необходимо создать таблицу, но не указано в какой схеме, то получается, что создам таблицу командой без указания схемы и она будет создана по умолчанию, в схеме ``public``... хотя пунктом ранее мы создали еще одну схему. Но принимаю решение создать по умолчанию:

```sql
  CREATE TABLE t1 (c1 integer);    
```
и да, создаю и проверяю - таблица ``t1`` создана в схеме ``public``:

  ![6_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/203ec6cd-422d-4291-8101-858ace625351)

 <br/> 

>**7. вставьте строку со значением c1=1**

вставляю данные командой:

```sql
  insert into t1 (c1) values (1);
```

вставила и проверила, вс ок:

  ![7_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/80fe70b4-9a81-4f3b-b2b5-1fb4726635c7)

<br/> 

>**8. создайте новую роль readonly**

создаю роль командой:

```sql
  create role readonly;
```

  ![8_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/9672f6d7-24a3-4660-8236-d8396bdcc568)

<br/>

>**9. дайте новой роли право на подключение к базе данных testdb**

для этого выполняю команду:

```sql
  grant connect on database testdb to readonly;
```

  ![9_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/4959f081-e84b-48f5-a441-0d535e255e40)

<br/>

>**10. дайте новой роли право на использование схемы testnm**

для этого выполняю команду:

```sql
  grant usage on schema testnm to readonly;
```

<br/>

  ![10_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/e0541132-d453-4a95-b867-7d81a9bed83a)

<br/>

>**11. дайте новой роли право на select для всех таблиц схемы testnm**

выполняю командой:

```sql
  grant select on all tables in schema testnm to readonly;
```

  ![11_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/f1ddba5d-437f-431d-953c-e97fda4d6b44)

<br/>

>**12. создайте пользователя testread с паролем test123**

выполняю командой и потом проверяю:

```sql
  CREATE USER testread PASSWORD 'test123';
```

  ![12_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/1578027f-08d9-46d5-9086-261cc818c0fe)

<br/>

>**13. дайте роль readonly пользователю testread**

выполняю командой и потом проверяю:

```sql
  grant readonly TO testread;
```

  ![13_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/b302c06b-3d5e-49a9-aef1-eef42d8a4768)

<br/>

>**14. зайдите под пользователем testread в базу данных testdb**

открываю еще один терминал и там выполняю команду: ``psql -h 127.0.0.1 -p 5433 -U testread -d testdb``

  ![14_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/8c746925-d206-4a51-b0c6-37941cd5fb14)

<br/>



сделайте select * from t1;
получилось? (могло если вы делали сами не по шпаргалке и не упустили один существенный момент про который позже)
напишите что именно произошло в тексте домашнего задания
у вас есть идеи почему? ведь права то дали?
посмотрите на список таблиц
подсказка в шпаргалке под пунктом 20
а почему так получилось с таблицей (если делали сами и без шпаргалки то может у вас все нормально)
вернитесь в базу данных testdb под пользователем postgres
удалите таблицу t1
создайте ее заново но уже с явным указанием имени схемы testnm
вставьте строку со значением c1=1
зайдите под пользователем testread в базу данных testdb
сделайте select * from testnm.t1;
получилось?
есть идеи почему? если нет - смотрите шпаргалку
как сделать так чтобы такое больше не повторялось? если нет идей - смотрите шпаргалку
сделайте select * from testnm.t1;
получилось?
есть идеи почему? если нет - смотрите шпаргалку
сделайте select * from testnm.t1;
получилось?
ура!
теперь попробуйте выполнить команду create table t2(c1 integer); insert into t2 values (2);
а как так? нам же никто прав на создание таблиц и insert в них под ролью readonly?
есть идеи как убрать эти права? если нет - смотрите шпаргалку
если вы справились сами то расскажите что сделали и почему, если смотрели шпаргалку - объясните что сделали и почему выполнив указанные в ней команды
теперь попробуйте выполнить команду create table t3(c1 integer); insert into t2 values (2);
расскажите что получилось и почему
