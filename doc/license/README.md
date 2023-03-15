## Информация о лицензии 

Получить параметры лицензии

**URL** : `/check-license`

**Метод** : `GET`

**Формат ответа**

```json
{
  "type": "object",
  "properties": {
    "unique_value_limit": {
      "description": "Количество запросов в лицензии",
      "type": "int"
    },
    "unique_value_count": {
      "description": "Количество сделанных запросов по лицензии",
      "type": "int"
    }
  }
}
```

**Пример ответа**

```json
{
  "unique_value_limit": 1000,
  "unique_value_count": 123
}
```
