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

>**15. сделайте select * from t1;**

выполняю:

  ![15_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/81580532-2800-41d9-afad-21b3a0e8d15e)

<br/>


>**16. получилось? (могло если вы делали сами не по шпаргалке и не упустили один существенный момент про который позже)**

Ожидаемо, не получилось! Имею говорящую ошибку об отсутствии прав на эту таблицу у этого пользователя! 

<br/>

>**17. напишите что именно произошло в тексте домашнего задания**

Как я ранее задумалась и писала - ясоздала схему ``testnm`` , а потом создала таблицу t1 в ``public``, так как не было уточнения - в какой схеме создавать таблицу.

<br/>

>**18. у вас есть идеи почему? ведь права то дали?**

А потому, что я создала таблицу в схеме ``public``(по умолчанию, так как приняла решение не указывать схему ``testnm``), а логинилась пользователем ``testread`` , который имеет роль ``readonly``, и поэтому имееет права(наследует от роли) на схему ``testnm`` , а таблица ``t1`` у нас в схеме ``public``.

<br/>

>**19. посмотрите на список таблиц**

  ![19_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/d38ca730-753f-4c57-891e-3d9361a9ce12)

<br/>  

>**20. подсказка в шпаргалке под пунктом 20**

О, да! Кратко и понятно - посмотрела в шпаргалке, да: 

***таблица создана в схеме public а не testnm и прав на public для роли readonly не давали***

<br/> 

>**21. а почему так получилось с таблицей (если делали сами и без шпаргалки то может у вас все нормально)**

 Потому,  что я осознанно сделала выбор - создавать таблицу в схеме по умолчанию, а это ``public``, не указывала схему  при создании таблицы как ``testnm``

<br/>

>**22. вернитесь в базу данных testdb под пользователем postgres**

  ![22_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/eb9c029e-75b2-4ccb-b941-eba30ecab63e)

<br/>

>**23. удалите таблицу t1**

выполняю командой и потом проверяю:

```sql
  drop table t1;
```

  ![23_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/e671cb69-10ae-4acd-b389-aad34e8053d6)

<br/>

>**24. создайте ее заново но уже с явным указанием имени схемы testnm**

выполняю командой и потом проверяю:

```sql
  create table testnm.t1 (c1 integer);
```

  ![24_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/d72c0d57-fc0a-4baa-a97b-0e2d0d421b1d)

<br/>

>**25. вставьте строку со значением c1=1**

выполняю командой и потом проверяю:

```sql
  insert into testnm.t1 (c1) values (1);
```

  ![25_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/ee2d0852-428e-45f7-ad92-387eaeb2be2f)

<br/>

>**26. зайдите под пользователем testread в базу данных testdb**

перехожу во второй терминал(мне так нагляднее) и там выполняю команду: ``psql -h 127.0.0.1 -p 5433 -U testread -d testdb``

  ![26_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/d4ab8e64-4e96-4637-a157-270479f04b1c)

<br/>

>**27. сделайте select * from testnm.t1;**

выполняю:

  ![27_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/86587024-cd3a-4666-a1b7-e31f22ab2425)

<br/>

>**28. получилось?**

Нет, не получилось! Снова получаю ошибку об отсутствии прав на эту таблицу у этого пользователя! 

<br/>

>**29. есть идеи почему? если нет - смотрите шпаргалку**

Я права давала ранее, для таблиц которые существовали на тот момент, а таблицу ``t1`` я создала позднее и права на неё не распространились.

<br/>

>**30. как сделать так чтобы такое больше не повторялось? если нет идей - смотрите шпаргалку**

Чтобы такое не повторялось определю привилегии доступа по умолчанию(для этого есть команда ``alter default privileges``). 
<br>В моем случае в схеме ``testnm`` для селекта для роли ``readonly`` выполню от пользователя ``postgres`` команду:

```sql
  ALTER default privileges in SCHEMA testnm grant SELECT on TABLES to readonly; 
```

  ![30_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/3f44287f-6458-4ff6-aa2a-271477cb5525)

<br/>

>**31. сделайте select * from testnm.t1;**

  ![31_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/963b592b-2ad6-4ef9-a0cc-e524d4f162f9)

<br/>

>**32. получилось?**

Нет!!!! Все та же ошибка - нет прав на эту таблицу!

<br/>

>**33. есть идеи почему? если нет - смотрите шпаргалку**

Я определила привилегии доступа по умолчанию(``alter default privileges``) для таблиц, создаваемых в будущем. А текущие права не меняла. Мне необходимо снова выполнить команду ``grant select`` или пересоздать таблицу ``t1``. Выполняю снова команду от пользователя ``postgres``: 

```sql
  grant select on all tables in schema testnm to readonly;
```

  ![33_2](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/0279381a-0c1a-4a9c-bcbe-80b4201758b1)

<br/>

>**34. сделайте select * from testnm.t1;**
 
Выполняю от пользователя ``testread``:

![34_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/f90e9c28-ea78-4ad3-b5ef-ffec34defb9f)

<br/>

>**35. получилось?**

ДА! Теперь все ок!

<br/>

>**36. ура!**

ДА !!!   :+1:

<br/>

>**37. теперь попробуйте выполнить команду create table t2(c1 integer); insert into t2 values (2);**

Выполняю от пользователя ``testread`` и все ок: таблица создалась, строка вставилась:  
  
  ![37_3](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/29263526-f925-47e8-8302-4456b7896517)

<br/>

>**38. а как так? нам же никто прав на создание таблиц и insert в них под ролью readonly?**

Так как схемы ``$USER`` нет, то таблица по умолчанию создалась в схеме ``public``. А когда я создавала нового пользователя , то ему по умолчанию была присвоена роль ``public``, которая позволяет выполнять все действия в схеме ``public`` в тех базах данных, к которым у пользователя есть доступ. 

<br/>

>**39. есть идеи как убрать эти права? если нет - смотрите шпаргалку**

Чтобы избежать такого поведения необходимо запретить создание объектов в схеме ``public`` и отозвать действующие права в схеме ``public``.
<br>Поэтому выполняю команды (под пользователем ``postgres``):

```sql
  REVOKE CREATE on SCHEMA public FROM public; 
  REVOKE ALL on DATABASE testdb FROM public;
```

  ![39_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/449aa11d-b777-41e6-93a9-be0e4d647ae3)

<br/>

>**40. если вы справились сами то расскажите что сделали и почему, если смотрели шпаргалку - объясните что сделали и почему выполнив указанные в ней команды**

Вот в этом пункте , после долгих размышлений и смущений - честно сознаюсь - пошла смотреть шпаргалку! Спасибо, это было интересно ... и поучительно! Теперь разобралась - пунктом выше написала - в чем тут суть...

<br/>

>**41. теперь попробуйте выполнить команду create table t3(c1 integer); insert into t2 values (2);**

Выполняю от пользователя ``testread``:

  ![40_1](https://github.com/Y-M-Morozova/4_homework_Morozova_Yulia/assets/153178571/22441e2a-5672-478b-b059-1683d5c5317a)

<br/>

>**42. расскажите что получилось и почему**

А вот тут ожидаемо - создать таблицу не получилось(права на создание новых объектов в схеме ``public`` уже отозваны.)
<br>А вот вставить строку - получилось! И это тоже ожидаемо - я смотрела выше свойства таблицы - пользователь ``testread`` является собственником (``owner``) таблицы ``t2`` - он стал им , когда создавал её, поэтому он имеет права на эту таблицу, строку вставить смог!
<br/>

***
