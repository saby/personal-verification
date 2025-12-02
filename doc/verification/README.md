## Проверка частного лица

Запустить проверку.

**URL** : `/start-verification`

**Обязательные параметры** :
- `name(str)` - Имя
- `surname(str)` - Фамилия

**Необязательные параметры** :
- `task_id(str)` - uuid запроса
- `patronymic(str)` - отчество
- `gender(str)` - пол. Возможные значения (m/f)
- `birth_date(str)` - дата рождения в формате yyyy-mm-dd
- `birth_place(str)` - место рождения
- `citizenship(str)` - код гражданства
- `citizenship_title(str)` - название гражданства
- `passport_series(str)` - серия паспорта
- `passport_number(str)` - номер паспорта
- `passport_issuer(str)` - подразделение, выдавшее паспорт
- `passport_issue_date(str)` - дата выдачи паспорта в формате yyyy-mm-dd
- `passport_department_code(str)` - код подразделения, выдавшего паспорт
- `insurance_number(str)` - номер СНИЛС
- `taxpayer_number(str)` - ИНН
- `medical_book_number(str)` - номер медицинской книжки
- `foreign_identity_series(str)` - серия иностранного паспорта
- `foreign_identity_number(str)` - номер иностранного паспорта
- `work_permit_series(str)` - серия разрешения на работу/патент
- `work_permit_number(str)` - номер разрешения на работу/патент
- `work_permit_blank_series(str)` - серия бланка разрешения на работу/патент
- `work_permit_blank_number(str)` - номер бланка разрешения на работу/патент
- `work_permit_doc_type(int)` - тип документа (0 - разрешение на работу/1 - патент)
- `diplomas([str])` - документы об образовании (списком). Следующие поля должны быть json-строкой.
  - `education_type(int)` - вид образования (3 - Среднее профессиональное образование, 5 - Высшее образование, 7 - Профессиональная переподготовка, 14 - Профессиональное обучение)
  - `document_type(str)` - тип документа тип документа (0 - диплом, 1 - аттестат, 2 - свидетельство, 3 - удостоверение, 4 - сертификат, 5 - справка)
  - `series(str)` - серия бланка
  - `number(str)` - номер бланка
  - `surname(str)` - фамилия при получении 
  - `register_number(str)` - регистрационный номер сертификата (указывается только для education_type = 7)
  - `issue_date(str)` - дата выдачи в формате yyyy-mm-dd
  - `organization_title(str)` - название образовательной организации

**Метод** : `POST`

**Формат ответа**

```json
{
  "type": "object",
  "properties": {
    "task_id": {
      "description": "Идентификатор проверки. В дальнейшем по нему можно получить результаты проверки, или перезапустить проверку",
      "type": "str"
    },
    "errors": {
      "description": "Список некорректных документов",
      "type": "list",
      "items": [
        {
          "type": "object",
          "properties": {
            "document_type": {
              "description": "Тип документа. Возможные значения: «passport», «insurance», «taxpayer», «work_permit», «foreign_identity», «medical_book», «diplomas»",
              "type": "str"
            },
            "value": {
              "description": "Идентификатор документа, например серия и номер",
              "type": "str"
            },
            "reason": {
              "description": "Причина некорректности данных документа",
              "type": "str"
            }
          }
        }
      ]
    }
  }
}
```

**Пример запроса**  
url - https://api.sbis.ru/pv/start-verification  
body(в понятном виде) - 
> {  
    'name': 'Патроним',  
    'surname': 'Квантов',  
    'diplomas': '[{"education_type": 3, "document_type": 0, "series": "161461", "number": "461451", "surname": "Квантов", "register_number": "72542543", "issue_date": "2010-05-10", "organization_title": "УКГУ Квантозоводска"}, {"education_type": 3, "document_type": 2, "series": "15215", "number": "51453421", "surname": "Квантов", "register_number": "314513541", "issue_date": "2003-06-24", "organization_title": "УКМ Квантозоводска"}]'  
}

**Пример ответа**

```json
{
  "task_id": "22200000-0000-0000-0000-000000000000",
  "errors": [
    {
      "document_type": "passport",
      "reason": "Количество символов серии и номера паспорта должно быть равно 10.",
      "value": "43215;123456"
    },
    {
      "document_type": "insurance",
      "reason": "Количество символов СНИЛС должно быть равно 11.",
      "value": "333"
    },
    {
      "document_type": "taxpayer",
      "reason": "Количество символов ИНН для физ. лиц должно быть равно 12.",
      "value": "4098921036343"
    },
    {
      "document_type": "medical_book",
      "reason": "Номер медицинской книжки не может начинаться с нуля",
      "value": "0"
    },
    {
      "document_type": "diplomas",
      "reason": "Не удалось найти образовательную организацию по её названию",
      "value": "777666;654321"
    }
  ]
}
```

**Пример запроса**  
url - https://api.sbis.ru/pv/start-verification  
body(в понятном виде) - 
>{
  "name": "Арсений",
  "surname": "Чемоданов",
  "passport_series": 56783,
  "passport_number": 638456,
  "insurance_number": 87544499369,
  "taxpayer_number": 173933080991,
  "medical_book_number": 123
} 

**Пример ответа**

```json
{
  "task_id": null,
  "errors": [
    {
      "document_type": "passport",
      "reason": "Проверка с такими данными уже существует.",
      "value": "56783;638456",
      "task_id": "00000000-0000-0000-0000-000000000001"
    },
    {
      "document_type": "insurance",
      "reason": "Проверка с такими данными уже существует.",
      "value": "87544499369",
      "task_id": "00000000-0000-0000-0000-000000000002"
    },
    {
      "document_type": "taxpayer",
      "reason": "Проверка с такими данными уже существует.",
      "value": "173933080991",
      "task_id": "00000000-0000-0000-0000-000000000002"
    },
    {
      "document_type": "medical_book",
      "reason": "Проверка с такими данными уже существует.",
      "value": "123",
      "task_id": "00000000-0000-0000-0000-000000000003"
    }
  ]
} 
```
**Поянения**
Saby не выполняет повторную проверку документов с одинаковыми номерами в разных операциях. Возможны две ситуации:
дубли в новой проверке — вы создаете новую проверку для одного физлица и указываете документ, который уже был обработан в проверке другого физлица. В этом случае проверка не будет запущена, чтобы не расходовать лицензию. Информация о дублирующихся документах поступит в поле errors;
дубли в существующей проверке — вы добавляете документы в уже созданную проверку. Если добавляемый документ уже был использован в проверке другого физлица, то текущая проверка запустится, но данный документ проверяться не будет. Система сообщит о нем в поле errors, и лицензия на него не израсходуется.

**Обязательные параметры для каждого вида проверки**

Проверка паспорта

- `passport_number`
- `passport_series`
- `passport_issuer`

Проверка СНИЛС

- `insurance_number`
- `birth_date`

Разрешение/патент на работу

- `work_permit_doc_type`
- `work_permit_series`
- `work_permit_number`
- `work_permit_blank_series`
- `work_permit_blank_number`
- `foreign_identity_number`

Медицинская книжка

- `medical_book_number`

Дипломы

- `series`
- `number`
- `issue_date`
- `organization_title`

ИНН

- `taxpayer_number`

Самозанятость

- `taxpayer_number`

Проверки бизнеса

- `taxpayer_number`

Проверки прокуратурой

- `taxpayer_number`

Розыск

- `birth_date`

Терроризм

- `birth_date`

Долги по налогам

- `taxpayer_number`

Долги по исполнительным листам

- `passport_number`
- `passport_series`
- `insurance_number`
- `taxpayer_number`

Организация-банкрот

- `taxpayer_number`

Личное банкротство

- `taxpayer_number`

Дисквалификация

- `taxpayer_number`

***

Получить результат проверки.

**URL** : `/check-verification`

**Обязательные параметры** :
- `task_id(str)` - результат метода start-verification (task_id)

**Метод** : `GET`

**Формат ответа**

```json
{
  "type": "object",
  "properties": {
    "task_id": {
      "description": "Идентификатор проверки",
      "type": "str"
    },
    "last_verification_date_time": {
      "description": "Дата и время последней выполненной проверки",
      "type": "str"
    },
    "person": {
      "description": "Поданные данные на проверку",
      "type": "object",
      "properties": {
        "name": {
          "description": "Имя",
          "type": "str"
        },
        "surname": {
          "description": "Фамилия",
          "type": "str"
        },
        "patronymic": {
          "description": "Отчество",
          "type": "str"
        },
        "gender": {
          "description": "Пол (0 - мужчина/1 - женщина)",
          "type": "str"
        },
        "birth_date": {
          "description": "Дата рождения",
          "type": "str"
        },
        "birth_place": {
          "description": "Место рождения",
          "type": "str"
        },
        "passport_series": {
          "description": "Серия паспорта",
          "type": "str"
        },
        "passport_number": {
          "description": "Номер паспорта",
          "type": "str"
        },
        "passport_issuer": {
          "description": "Подразделение, выдавшее паспорт",
          "type": "str"
        },
        "passport_issue_date": {
          "description": "Дата выдачи паспорта",
          "type": "str"
        },
        "passport_department_code": {
          "description": "Код подразделения, выдавшего паспорт",
          "type": "str"
        },
        "insurance_number": {
          "description": "Номер СНИЛС",
          "type": "str"
        },
        "taxpayer_number": {
          "description": "ИНН",
          "type": "str"
        },
        "citizenship": {
          "description": "Код гражданства",
          "type": "str"
        },
        "citizenship_title": {
          "description": "Название гражданства",
          "type": "str"
        },
        "foreign_identity_series": {
          "description": "Серия иностранного паспорта",
          "type": "str"
        },
        "foreign_identity_number": {
          "description": "Номер иностранного паспорта",
          "type": "str"
        },
        "work_permit_series": {
          "description": "Серия разрешения на работу/патент",
          "type": "str"
        },
        "work_permit_number": {
          "description": "Номер разрешения на работу/патент",
          "type": "str"
        },
        "work_permit_blank_series": {
          "description": "Серия бланка разрешения на работу/патент",
          "type": "str"
        },
        "work_permit_blank_number": {
          "description": "Номер бланка разрешения на работу/патент",
          "type": "str"
        },
        "work_permit_doc_type": {
          "description": "Тип документа (0 - разрешение на работу/1 - патент)",
          "type": "str"
        },
        "diplomas": {
          "description": "Документы об образовании (списком)",
          "type": "array",
          "items": [
            {
              "type": "object",
              "properties": {
                "education_type":{
                  "description": "Вид образования",
                  "type": "int"
                },
                "document_type":{
                  "description": "Тип документа [диплом|аттестат|свидетельство|удостоверение|сертификат|справка]",
                  "type": "int"
                },
                "series":{
                  "description": "Серия бланка",
                  "type": "str"
                },
                "number":{
                  "description": "Номер бланка",
                  "type": "str"
                },
                "surname":{
                  "description": "Фамилия при получении ",
                  "type": "str"
                },
                "register_number":{
                  "description": "Регистрационный номер сертификата",
                  "type": "str"
                },
                "issue_date":{
                  "description": "Дата выдачи",
                  "type": "str"
                },
                "organization_title":{
                  "description": "Название образовательной организации",
                  "type": "str"
                }
              }
            }
          ]
        }
      }
    },
    "verifications": {
      "type": "object",
      "properties": {
        "passport": {
          "description": "Проверка паспортных данных",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки. Смотреть описание ниже",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            }
          }
        },
        "taxpayer": {
          "description": "Проверка налогов",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            }
          }
        },
        "insurance": {
          "description": "Проверка страхования",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            }
          }
        },
        "self_employed": {
          "description": "Проверка частного предпринимателя",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            }
          }
        },
        "diplomas": {
          "description": "Проверка дипломов",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "array",
              "items": [
                {
                  "type": "object",
                  "properties": {
                    "status": {
                      "description": "Статус",
                      "type": "bool"
                    },
                    "title": {
                      "description": "Название",
                      "type": "str"
                    }
                  }
                }
              ]
            }
          }
        },
        "work_permit": {
          "description": "Проверка разрешения на работу",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            }
          }
        },
        "business": {
          "description": "Проверка бизнеса",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "array",
              "items": [
                {
                  "type": "object",
                  "properties": {
                    "company_name": {
                      "description": "Название компании",
                      "type": "str"
                    },
                    "inn": {
                      "description": "ИНН",
                      "type": "str"
                    },
                    "kpp": {
                      "description": "КПП",
                      "type": "str"
                    },
                    "kind_of_activity": {
                      "description": "Вид деятельности",
                      "type": "str"
                    },
                    "company_place": {
                      "description": "Город/регион нахождения компании",
                      "type": "str"
                    },
                    "state": {
                      "description": "Состояние компании. Смотреть описание ниже",
                      "type": "str"
                    },
                    "registration_date": {
                      "description": "Дата регистрации",
                      "type": "str"
                    },
                    "liquidation_data": {
                      "description": "Дата ликвидации",
                      "type": "str"
                    },
                    "is_our_company": {
                      "description": "Принадлежность к компании",
                      "type": "bool"
                    },
                    "is_accurate_match": {
                      "description": "Надежное сопоставление",
                      "type": "bool"
                    },
                    "actual_record": {
                      "description": "Актуальность записи",
                      "type": "bool"
                    },
                    "proceeds": {
                      "description": "Выручка",
                      "type": "float"
                    },
                    "last_reporting_date": {
                      "description": "Дата последнего отчета",
                      "type": "str"
                    },
                    "actual_to": {
                      "description": "Актуален до",
                      "type": "str"
                    },
                    "violation_messages": {
                      "description": "Список нарушений организации",
                      "type": "array",
                      "items": [
                        {
                          "type": "str"
                        }
                      ]
                    },
                    "founders": {
                      "description": "Учредители",
                      "type": "array",
                      "items": [
                        {
                          "type": "object",
                          "properties": {
                            "actual": {
                              "description": "Актуальность",
                              "type": "str"
                            },
                            "date_from": {
                              "description": "Дата от",
                              "type": "str"
                            },
                            "name": {
                              "description": "Имя",
                              "type": "str"
                            },
                            "share": {
                              "description": "Доля",
                              "type": "str"
                            },
                            "date_to": {
                              "description": "Дата до",
                              "type": "str"
                            }
                          }
                        }
                      ]
                    },
                    "managers": {
                      "description": "Управляющие",
                      "type": "array",
                      "items": [
                        {
                          "type": "object",
                          "properties": {
                            "actual": {
                              "description": "Актуальность",
                              "type": "str"
                            },
                            "date_from": {
                              "description": "Дата от",
                              "type": "str"
                            },
                            "name": {
                              "description": "Имя",
                              "type": "str"
                            },
                            "share": {
                              "description": "Доля",
                              "type": "str"
                            },
                            "date_to": {
                              "description": "Дата до",
                              "type": "str"
                            }
                          }
                        }
                      ]
                    }
                  }
                }
              ]
            }
          }
        },
        "disqualification": {
          "description": "Проверка дисквалификации",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "object",
              "properties": {
                "reason_text": {
                  "description": "Причины",
                  "type": "str"
                },
                "reason_article": {
                  "description": "Статья причины",
                  "type": "str"
                },
                "org_title":  {
                  "description": "Название организации",
                  "type": "str"
                },
                "position": {
                  "description": "Должность",
                  "type": "str"
                },
                "start_date": {
                  "description": "Дата начала дисквалификации",
                  "type": "str"
                },
                "end_date": {
                  "description": "Дата окончания дисквалификации",
                  "type": "str"
                }
              }
            }
          }
        },
        "person_bankruptcy": {
          "description": "Проверка банкротство физ. лица",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "start_date": {
              "description": "Дата начала банкротства",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            }
          }
        },
        "taxes": {
          "description": "Проверка долгов платежей",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "object",
              "properties": {
                "sum": {
                  "description": "Сумма",
                  "type": "float"
                },
                "date": {
                  "description": "Дата",
                  "type": "str"
                },
                "title": {
                  "description": "Название",
                  "type": "str"
                },
                "actual": {
                  "description": "Актуальность",
                  "type": "bool"
                },
                "termination_date": {
                  "description": "Дата завершения",
                  "type": "str"
                },
                "termination_reason": {
                  "description": "Причина завершения",
                  "type": "str"
                }
              }
            }
          }
        },
        "writs": {
          "description": "Проверка долгов в ФССП",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "object",
              "properties": {
                "sum": {
                  "description": "Сумма",
                  "type": "float"
                },
                "date": {
                  "description": "Дата",
                  "type": "str"
                },
                "title": {
                  "description": "Название",
                  "type": "str"
                },
                "actual": {
                  "description": "Актуальность",
                  "type": "bool"
                },
                "termination_date": {
                  "description": "Дата завершения",
                  "type": "str"
                },
                "termination_reason": {
                  "description": "Причина завершения",
                  "type": "str"
                }
              }
            }
          }
        },
        "courts": {
          "description": "Проверка судов",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "array",
              "items": [
                {
                  "type": "object",
                  "properties": {
                    "role": {
                      "description": "Роль",
                      "type": "str"
                    },
                    "date": {
                      "description": "Дата",
                      "type": "str"
                    },
                    "sum": {
                      "description": "Сумма",
                      "type": "float"
                    },
                    "case_category": {
                      "description": "Код категории дела. Возможные значения: 0, 1, 2, 3",
                      "type": "float"
                    },
                    "case_category_name": {
                      "description": "Название категории дела. Возможные значения: Административное, Банкротное, Гражданское, Судебное поручение",
                      "type": "str"
                    },
                    "status": {
                      "description": "Статус",
                      "type": "str"
                    },
                    "case_number": {
                      "description": "Номер дела",
                      "type": "str"
                    },
                    "exact_match": {
                      "description": "True - точное сопоставление по ФИО и ИНН, False - не точное, по ФИО, может быть тезка",
                      "type": "bool"
                    },
                    "case_link": {
                      "description": "Cсылка на суд в картотеке арбитражных дел",
                      "type": "str"
                    },
                    "decision_name": {
                      "description": "Решение по делу",
                      "type": "str"
                    },
                    "stage_quantity": {
                      "description": "Количество этапов",
                      "type": "int"
                    },
                    "duration": {
                      "description": "Длительность",
                      "type": "str"
                    },
                    "defendants": {
                      "description": "Список ответчиков",
                      "type": "array",
                      "items": [
                        {
                          "type": "str"
                        }
                      ]
                    },
                    "plaintiffs": {
                      "description": "Список истцов",
                      "type": "array",
                      "items": [
                        {
                          "type": "str"
                        }
                      ]
                    }
                  }
                }
              ]
            }
          }
        },
        "terrorism": {
          "description": "Проверка принадлежности к террористам",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            }
          }
        },
        "wanted": {
          "description": "Проверка розыска",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "object",
              "properties": {
                "initiator": {
                  "description": "Инициатор розыска",
                  "type": "str"
                },
                "gender": {
                  "description": "Пол (m/f)",
                  "type": "str"
                },
                "nationality":  {
                  "description": "Национальность",
                  "type": "str"
                },
                "birth_place": {
                  "description": "Место рождения",
                  "type": "str"
                },
                "special_signs":  {
                  "description": "Особые приметы",
                  "type": "object"
                },
                "photo": {
                  "description": "Фото",
                  "type": "str"
                },
                "additional_info":  {
                  "description": "Дополнительная информация",
                  "type": "str"
                }
              }
            }
          }
        },
        "sanctions": {
          "description": "Проверка наличия в санкционных списках",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "object",
              "properties": {
                "countries": {
                  "description": "Страны наложившие санкции",
                  "type": "list",
                  "items": [
                    {
                      "type": "str"
                    }
                  ]
                },
                "exact_match": {
                  "description": "Точное совпадение",
                  "type": "bool"
                }
              }
            }
          }
        },
        "foreign_agents": {
          "description": "Проверка наличия в реестре иноагентов",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            }
          }
        },
        "medical_book": {
          "description": "Проверка медицинской книжки",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "object",
              "properties": {
                "hygienic_training_entries": {
                  "description": "записи о гигиеническом обучении в медкнижки (пример https://lmk.cgon.ru/search/?serialnumb=1)",
                  "type": "list",
                  "items": [
                    {
                      "type": "object",
                      "properties": {
                        "program_name": {
                          "description": "Тип программы",
                          "type": "str"
                        },
                        "program_group": {
                          "description": "Название програмы",
                          "type": "str"
                        },
                        "date_to": {
                          "description": "Действует до",
                          "type": "str"
                        }
                      }
                    }
                  ]
                }
              }
            }
          }
        },
        "prosecutor_inspections": {
          "description": "Проверка прокуратурой",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "object",
              "properties": {
                "inspections": {
                  "description": "список проверок прокуратурой",
                  "type": "list",
                  "items": [
                    {
                      "type": "object",
                      "properties": {
                        "date": {
                          "description": "дата прошедшей проверки/дата планируемой проверки",
                          "type": "str"
                        },
                        "inspection_type": {
                          "description": "тип инспекции",
                          "type": "str"
                        },
                        "supervisor": {
                          "description": "орган проводивший проверку",
                          "type": "str"
                        }
                      }
                    }
                  ]
                }
              }
            }
          }
        },
        "trust_loss": {
          "description": "Проверка на наличие в реестре уволенных в связи с утратой доверия",
          "type": "object",
          "properties": {
            "status": {
              "description": "Статус проверки",
              "type": "int"
            },
            "status_title": {
              "description": "Текст статуса проверки",
              "type": "str"
            },
            "current_request_time": {
              "description": "Дата/время проверки",
              "type": "str"
            },
            "previous_status": {
              "description": "Прошлый статус проверки",
              "type": "int"
            },
            "previous_status_title": {
              "description": "Текст прошлого статус проверки",
              "type": "str"
            },
            "previous_request_time": {
              "description": "Дата/время прошлой проверки",
              "type": "str"
            },
            "last_update_status": {
              "description": "Последние дата/время изменение статуса",
              "type": "str"
            },
            "data": {
              "description": "Дополнительные данные",
              "type": "object",
              "properties": {
                "registry_date": {
                  "description": "дата занесения в реестр уволенных в связи с утратой доверия",
                  "type": "str"
                },
                "act_date": {
                  "description": "дата увольнения",
                  "type": "str"
                },
                "organ": {
                  "description": "место работы",
                  "type": "str"
                },
                "position": {
                  "description": "должность",
                  "type": "str"
                },
                "act": {
                  "description": "нарушение какого нормативного акта привело к увольнению",
                  "type": "str"
                }
              }
            }
          }
        }
      }
    }
  }
}
```

**Статусы проверок**:  
Общие (код присутствуют во всех проверках, но текстовый статус может меняться в зависимости от типа проверки):

    -1 - Фиктивный статус, проверка отсутствует  
    None - Проверка выполняется (только длительные)  
    0 - Нет проблем  
    1 - Проблемы  
    999 - Не смогли выполнить проверку (сервис не доступен/внутренняя ошибка)  
    1000 - Не смогли выполнить проверку (данные для проверки не были предоставлены)  

passport - проверка паспорта:

    -1 - Ошибка на сервисе СМЭВ  
    0 - Паспорт действителен  
    1 - Паспорт недействителен  
    2 - Наступил срок замены паспорта  
    3 - Паспорт принадлежит другому лицу  
    4 - Владелец паспорта мертв  
    5 - Неверный код подразделения паспорта  
    6 - Неверная дата выдачи паспорта  
    7 - Паспорт в списке недействительных  
    8 - Ошибка на сервисе СМЭВ  
    302 - Данных о паспорте в ИС ГУВМ МВД нет  
    601 - Истёк срок действия паспорта  
    602 - Паспорт заменен на новый  
    603 - Паспорт выдан с нарушением  
    604 - Паспорт числится в розыске  
    605 - Паспорт изъят или уничтожен  
    606 - Владелец паспорта мертв  
    607 - Паспорт недействителен, технический брак  
    608 - Прекращено гражданство РФ  
    609 - Паспорт утрачен  
    999 - Не смогли проверить нахождение в розыске. Сервис МВД недоступен

taxpayer - проверка:

    2 - ИНН не найден

insurance - проверка СНИЛС:

    0 - СНИЛС действителен  
    1 - СНИЛС недействителен  
    3 - СНИЛС принадлежит другому лицу  
    999 - СНИЛС не проверен  

self_employed - проверка статуса самозанятого:

    0 - Не является самозанятым  
    1 - Является самозанятым  
    999 - Не смогли проверить наличие статуса самозанятого. Сервис ФНС не доступен

diplomas - проверка дипломов:

    -1 - Не проверен  
    0 - Действителен  
    1 - Отсутствуют сведения  
    999 - Не удалось проверить действительность  

work_permit - проверка патента/разрешения на работу:

    -1 - Не проверен/Не проверено  
    0 - Действителен/Действительно  
    1 - Патент отозван/Разрешение на работу отозвано  
    2 - Патент недействителен/Разрешение на работу недействительно  
    999 - Не удалось проверить действительность/Не удалось проверить действительность  

business - проверка управления/учреждения бизнеса:

    1 - Руководитель  
    2 - Учредитель  
    3 - Есть собственный бизнес  
    4 - Массовый руководитель  
    5 - Массовый учредитель  
    6 - Массовый руководитель и учредитель  
    7 - Конфликт интересов  
    8 - Конфликт интересов  
    9 - Конфликт интересов  
    10 - Был руководителем  
    11 - Был учредителем  
    12 - Был собственный бизнес  

disqualification - проверка дисквалификации от управления:

    1 - Дисквалифицирован  
    2 - Отрицает причастность   
    3 - Дисквалифицирован и отрицает причастность  

person_bankruptcy - проверка банкротства физического лица:

    0 - Не банкрот  
    1 - Бывший банкрот  
    2 - Заявлен на банкротство  
    3 - Банкрот  
    4 - Риск банкротства  
    5 - Предбанкрот  
    999 - Не смогли проверить банкротство физического лица  

taxes - проверка долгов по налогам:

    0 - Нет долгов  
    1 - Долги  
    999 - Не смогли проверить наличие налогов  

writs - проверка долгов по исполнительным листам:

    0 - Нет долгов  
    1 - Долги  
    999 - Не смогли проверить наличие исполнительных листов  

courts - проверка наличия дел в общих и арбитражных судах:

    0 - Не найдены дела в судах  
    1 - Есть суды  
    999 - Не смогли проверить дела в судах  
    None - Ищем дела в судах  

terrorism - проверка террористов/экстремистов:

    1 - Числится в списке террористов, экстремистов  
    999 - Не смогли проверить наличие в списке террористов и экстремистов  

wanted - проверка розыска:

    1 - В розыске  
    999 - Не смогли проверить нахождение в розыске  

sanstions - нахождение в санкционных списках:

    0 - Не числится в санкционных списках
    1 - Числится в санкционных списках
    999 - Не смогли проверить наличие в санкционных списках

foreign_agents - нахождение в реестре иноагентов:

    0 - Не числится в реестре иноагентов
    1 - Числится в реестре иноагентов
    999 - Не смогли проверить наличие в реестре иноагентов

medical_book - медицинские книжки:

    0 - Медицинская книжка действительна
    1 - Медицинская книжка недействительна
    999 - Медицинская книжка не проверена

prosecutor_inspections - проверки прокуратурой:

    0 - Нет проводимых проверок
    1 - В отношении лица проводятся проверки
    999 - Не смогли проверить наличие проверок прокуратуры

trust_loss - утрата доверия:

    0 - Нет в реестре лиц, уволенных в связи с утратой доверия
    1 - Уволен в связи с утратой доверия
    999 - Не смогли проверить нахождения в реестре уволенных в связи с утратой доверия


**Поле state**

    0: 'ликвидирован',
    1: 'действующий',
    2: 'в состоянии реорганизации',
    3: 'неопределенно',
    4: 'в состоянии ликвидации',


*** 

Получить результат проверки в формате PDF

**URL** : `/check-verification/pdf`

**Обязательные параметры** :
- `task_id(str)` - результат метода start-verification (task_id)

**Метод** : `GET`

**Формат ответа**

```json
PDF-file
```

***

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
