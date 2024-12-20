# Лекция 14

Есть запрос к тому, чтобы хранить данные сложной структуры(древовидные, графовые). Мы хотим делать запросы, что-то анализировать. И таблицы эти случаи не покрывают. Ещё и объектов может быть много атрибутов, хотелось бы иметь динамическое количество.

## JSON

```JSON
{
    "guid": "1234567890",
    "name": "Kago",
    "company": "ltd gmbh",
    "tags": [
        "pro",
        "megapro"
    ]
}
```

Такая структурированная штука, но некоторых полей может и не быть. Например, если чел безработный, то поле компания у него не будет.
Есть аналог -- XML. На его основе делали БД. JSON захотели хранить в БД, язык запросов -- jsonpath. Хранить там можем строки, числа, массивы, джейсоны.

Есть разница между json и jsonb в том, что последний хранится в бинарной форме, можно индексировать. Можем писать всякие запросы.

Концептуальные вещи: можно делать индексы. Фактически, делаем индексацию графа, что сложно.

## jsonpath

У нас есть дерево. Раньше было модно все засовывать в графы, и у нас есть язык для работы с подграфами.

## Массивы в PostgreSQL

Их всегда хочется есть. В постгресе есть динамические массивы и массивы постоянной длинны. Есть БД для массивов.

## HSTORE

Это key-value хранилище в PostgreSQL. Тут на слайдах много примеров.

## Интерфейсы

Мы хотим работать с БД, а JDBC -- интерфейс для них. Мы делаем драйвера для каждой БД. Выглядит так: приложение -> JDBC -> JDBC Driver -> DB.

Как в коде? подключаемся к БД, делаем запрос, получаем ответы, обрабатываем их, закрываем соединение.

## ORM

Идея: забываем SQL, работаем в коде с постоянным хранилищем данных, которые как-то связываются с базой -- находятся, если надо обновляются, и так далее. Сложнее чем сгенерить селект из одной таблицы оно не может. Это не панацея, но и не полный провал, где-то это да и используется.

Как в коде? Есть класс, определяем атрибуты, связываем с базой. Ни одного селекта.