# Лекция 13

## Индексы

Какие есть способы получить доступ к записи? 

1) Последовательный поиск

2) Классический индекс(один на таблицу без учёта репликации), позволяет бинарный поиск, если угадали с атрибутом

3) Индекс -- ускоряет доступ, если угадали с атрибутом

Как работает индекс? Нужно упорядочивание. Например, может помочь B+ дерево(это скорее поможет отвечать на вопросы "покажи всё правее *object name*"), R-дерево(это скорее для поиска ближайших точек в метрическом пространстве). Иногда хэшмапа, но реже.
Индекс задаётся по конкретной колонке. Как известно с практик, индекс не всегда выгоден, он может сделать более медленной работу некоторых запросов. К тому же дерево весит какое-то ощутимое пространство.

Интересно, оптимизатор может проигнорировать наличие индекса, потому что это может быть невыгодно.

## B+ дерево

Все значения в индекс, предполагается, что все нелистовые вершины будут помещаться в оперативную память, там хранится информация для навигации по дереву. Кроме того, в листьях хранятся offsetы в файле! Размер данных считаем известным. А что если надо находить записи от одной до другой? Тогда надо бегать вверх по дереву. Но само дерево это не индекс.

## Реализация

Надо думать о параллельности, как минимум, тут нужно позаботиться по мютаксах и т.д. 
Проблема восстанавливаемости, так-то БД гарантируют то, что если внезапно отключится питание, то данные сохранятся.
Надо думать о физическом дизайне данных.

## PostgreSQL

```sql
CREATE INDEX name_ ON table_ USING HASH (column);
```

Индекс строится под конкретные операции. виды индексов: B-tree, Hash, GiST, SP-GiST, GIN(на строках), BRIN(на строках). 

Можем создать индекс на конкатенированном ключе(не все индексы позволяют).

## Индексы и уникальность

```sql 
CREATE UNIQUE INDEX ...
```
-- запрещаем вставлять одинаковые записи.

## Частичные индексы

Полезно задавать индекс на части таблицы.

Пример: у нас 100 Иванов, 2 Петра, 1 Вильгельм в таблице.

Низкоселективный запрос(найти Ивана), высокоселективный запрос(найти Вильгельма).
Теперь, пусть у нас есть жесткий диск и мы заказываем ему данные(в случае Ивана -- их много), т.е. мы будем спамить операциями disk seek. Дизайн БД -- у нас есть файл, побитый на восьмикилобайтные страницы, и куча где Иван. Тут помогает последовательный поиск, работает быстрее чем при disk seek. Т.е. индекс испортил поиск, об этом надо думать. Это важный момент.

Частичный индекс чинит этот момент. У нас не индексируются частые записи(Иваны, например).

Но работают не всегда. Зависит от распределения данных, хорошо, чтобы они не менялись. Поменялись, надо пересоздавать индекс, это работа администратора.

Иногда постгрес глючит и использует индекс там, где не надо, и тут может помочь частичный индекс.

## Транзакции

Транзакция (англ. transaction) — группа последовательных операций с базой данных, которая представляет собой логическую единицу работы с данными. Транзакция может быть выполнена либо целиком и успешно, соблюдая целостность данных и независимо от параллельно идущих других транзакций, либо не выполнена вообще, и тогда она не должна произвести никакого эффекта.

Т.е. мы хотим перемещать БД из одного валидного состояния в другое.

Соответствует AtomacityConsistencyIsolationDurability. A: либо выполняется, либо совсем нет, С: данные переходят из одного состояния в другое, I: конкурирующие потоки выполняются последовательно, изолированно друг от друга, D: когда вырубится свет -- всё будет ОК.

Примеры всяких проблема см. на слайдах. Да, тут много чего надо добавить с слайдов, да. Но слайды хорошие.