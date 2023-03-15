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
- `foreign_identity_series(str)` - серия иностранного паспорта
- `foreign_identity_number(str)` - номер иностранного паспорта
- `work_permit_series(str)` - серия разрешения на работу/патент
- `work_permit_number(str)` - номер разрешения на работу/патент
- `work_permit_blank_series(str)` - серия бланка разрешения на работу/патент
- `work_permit_blank_number(str)` - номер бланка разрешения на работу/патент
- `work_permit_doc_type(int)` - тип документа (0 - разрешение на работу/1 - патент)
- `diplomas([str])` - документы об образовании (списком). Следующие поля должны быть json-строкой.
  - `education_type(str)` - вид образования
  - `document_type(str)` - тип документа (диплом|аттестат|свидетельство|удостоверение|сертификат|справка)
  - `series(str)` - серия бланка
  - `number(str)` - номер бланка
  - `surname(str)` - фамилия при получении 
  - `register_number(str)` - регистрационный номер сертификата
  - `issue_date(str)` - дата выдачи в формате yyyy-mm-dd
  - `organization_title(str)` - название образовательной организации

**Метод** : `POST`

**Формат ответа**

```json
{
  "type": "str",
  "description": "task_id. В дальнейшем по нему можно получить результаты проверки, или перезапустить проверку"
}
```

**Пример запроса**  
url - https://api.sbis.ru/pv/start-verification  
body(в понятном виде) - diplomas={"education_type": "очное", "document_type": "диплом", "series": "161461", "number": "461451",
"surname": "Квантов", "register_number": "72542543", "issue_date": "2010-05-10",
"organization_title": "УКГУ Квантозоводска"}&diplomas=
{"education_type": "заочное", "document_type": "свидетельство", "series": "15215", "number": "51453421",
"surname": "Квантов", "register_number": "314513541", "issue_date": "2003-06-24",
"organization_title": "УКМ Квантозоводска"}&name=Патроним&surname=Квантов

**Пример ответа**

```json
00000000-0000-0000-0000-000000000000
```

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
                  "type": "str"
                },
                "document_type":{
                  "description": "Тип документа [диплом|аттестат|свидетельство|удостоверение|сертификат|справка]",
                  "type": "str"
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
                    "city": {
                      "description": "Город нахождения компании",
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
                    "actual_from": {
                      "description": "Актуален с",
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
    999 - Не удалось проверить паспорт  

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
    999 - Не смогли проверить наличие статуса самозанятого  

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
