# Лекция 8

Говорим про нормализацию.

Аксиомы функциональных зависимостей: 1) транзитивность, 2) если a множество атрибутов, а b -- его подмножество, то мы видим функциональную зависимость, 3) можем в существующую ФЗ накинуть атрибутов.
Что можно вычислять? Замыкание атрибутов -- чтобы выяснить, является ли что-то супер ключом.
Каноническое покрытие -- минимальный набор ФЗ, из которого можно вывести все остальные.

## SQL: подзапросы и соединения

Подзапрос -- ещё один запрос внутри другого. Без него некоторые вещи не сделать. Может быть везде: в where, select, having.

## Проверка принадлежности множеству

```sql
(select course_id from db2017)
intersect
(select course_id from db2017)
```
Можно оставить левую часть intersect, а правую добавить в where in. СЕмантика та же.ы

## Синтаксический сахар

Для удобства можно использовать enumerated set: in ('Mozart', 'Einstein').

Ещё можно писать where in (select ...)

## Сравнение множеств

Могли использовать join, а можем и подзапросы. Пример:
```sql
where salary > some(select ...)
```
Вместо some может быть all. Семантика в обоих случая понятна. Аналогично можем писать в having.

C помощью exists можем проверять на пустоту. Но тут важно смотреть, оптимально ли будет это. На слайдах есть пример, где написан корелированный запрос и у нас поиск получается за квадрат.

Ещё один способ проверить множества not exists: (A содержит B) = not exists (B except A). И вообще уметь переписывать запросы полезно, т.к. в разных случаях нужны разные оптимизации.

Уникальность записей можно проверить с помощью предиката unique. unique может быть не везде, для этого пишут
```sql 
where 1 >= (select count(...))
```

## Подзапросы во from

Помогают избавиться от having
```sql 
select a, b from (select ... having ...)  where b > 10293;
```

Если во from есть подзапрос, то он не видит атрибуты соседних подзапросов/таблиц, чтобы были видны есть lateral.
Пример:
```sql
from I1, lateral(... as I1...)

```
Иначе так обратиться не получилось бы. Не везде поддерживается.

## Ключевое слово with

```sql
with tmp_table
....
select ...
from ...
where tmp_table.smt
```

Но не надо думать, что будет создаваться временная таблица. Эта таблица видится ниже в запросах. with таблиц может быть много и мх можно использовать в нескольких местах.

## Скалярные подзапросы

Запросы возвращающие одно значение.