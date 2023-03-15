# API сервиса «Проверка сотрудников»  

### Авторизация

Подробно об авторизации - [https://sbis.ru/help/integration/api/auth](https://sbis.ru/help/integration/api/auth).
Интересующие пункты: "Добавить приложение" и "Настроить сервисную авторизацию".


###Запрос

Общение с API происходит по url: https://api.sbis.ru/pv/.  
Дальше к адресной строке добавляется метод, например получение результатов проверки (check-verification) - https://api.sbis.ru/pv/check-verification.  
В конце добавляются параметры запроса, например - https://api.sbis.ru/pv/check-verification?task_id=00000000-0000-0000-0000-000000000000.  
Метод http и параметры запроса описаны для каждого метода в отдельном README.  
Во все запросы нужно подавать headers как указано в статье об авторизации.

###Методы

start-verification, check-verification, check-verification/pdf - методы проверок сотрудников [подробное описание](doc/verification/README.md)

check-license - информация о лицензии [подробное описание](doc/license/README.md)
