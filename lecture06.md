# Лекиця 6

## Строки

Пишутся в одинарных кавычках, "экспейтятся" двумя.

Регистр важен при сравнении строк, но не все СУБД этому следуют. 

Конкатенация строк через ||. Есть привычные операции upper(s), lower(s), trim(s).

Оператор LIKE позволяет проверять на совместимость с регулярным выражением.

## Сортировка

Можем сортировать строки в таблице с помощью ORDER BY, DESC, ASC.

## null

Критикуется за непонятность семантики, зависит от контекста.

Любая операция(+, -, *, /) с нулом -- нулл. Если сравниваем с нулом, то получаем Unknown. Unknown прокинули во where. Unknown проходит как False и запись не пройдёт.

В случае Distinct и null, null будет равен null.

Offtop: Можем писать SELECT DISTINCT ON (attr1, attr2, ..., attrn), но какой из дубликатов в этом случае выкидывать?(целиком кортежи могут быть разными). Решается это в runtime(не зависимо от данных), надо взять первую по порядку строку(проверяется, есть ли в ORDER BY нужный атрибут или нет).

## Агрегирующие функции

(SUM, AVG, MAX, MIN, COUNT)

## Агрегация с группировкой

Выбираем атрибут, по которому мы сгруппируем строки. Используем GROUP BY.
Мы можем усложнить запрос и добавить туда соединение, а затем группировку. Можем написать HAVING и убираем сгруппированные данные.

## Порядок вычислений

FROM -> WHERE -> GROUP BY -> HAVING -> упорядочивание, применение агрегирующих функций.

## Агрегация и null

Есть правила по работает агрегирующих функций и null.

## Алгебра мультимножеств(продолжение)

Какой-то формализм.

## Нормализация


