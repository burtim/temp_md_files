#Endpoints AutoBonus

## JWT Authorization header
~~~
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoidGltb2ZleWJ1cmt1c2hAZ21haWwuY29tIiwianRpIjoiNjRjYTYyZDMtMjg5OS00NDYyLTliMmYtNTU0MDQ3M2ExYzA4IiwidXNlcklkIjoiMiIsImVtYWlsIjoidGltb2ZleWJ1cmt1c2hAZ21haWwuY29tIiwib3JnYW5pemF0aW9uSWQiOiIxIiwicm9sZXMiOiJjbGllbnQsbWFuYWdlciIsImV4cCI6MTYzNDI4Mjg1MywiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo1OTMzNyIsImF1ZCI6Imh0dHA6Ly9sb2NhbGhvc3Q6NTkzMzcifQ.LLS3ysLfFar0esd8fOqQsJC1A5SkBox3TmhCIncoDjk' \
~~~


---
## Аккаунт

###/ api / account / login
POST (Авторизация пользователя)
```json
На вход принимает
{
  "email": "string",
  "password": "string"
}

отдает
{
  "userId": int,
  "userName": "string",
  "token" : "string",
  "refreshToken": "string"
}
```

### api/account/logout
POST (Выход пользователя)

### api/account/refresh-tokens
POST (Обновить токены)
```json
На вход принимает
{
  "refreshToken": "string"
}

отдает
{
  "refreshToken": "string",
  "jwtToken": "string"
}
```

### api/account/password/restore
POST (Запрос на восстановление пароля (Отправляет письмо на почту и отдает 200))  
__RestorePageUrl__ - ссылка на страницу, где будет форма изменения пароля.
Если __restorePageUrl__ = ___https://autobonus/change___, то на почту придет ссылка
https://autobonus/change?hash={hash}
```json
На вход принимает
{
  "email": "string",
  "restorePageUrl": "string"
}

отдает
200
```

### api/account/password/change
POST (Изменение пароля)  
__hash__ параметр будет в ссылке на почте, брать из get запроса
```json
На вход принимает
{
  "hash": "string",
  "password": "string",
  "passwordConfirm": "string"
}

отдает
200
```

---
## Регистрация КЛИЕНТА

### /api/account/registration/client
POST (Регистрация Клиента)
Описание рпегистрации: Клиент сначала заполняет данные, 
затем все данные отправляются на сервер, по указанному email отправляется код
подтверждения (6-значный). Затем пользователь вводит в форму код подтверждения
и если код верный, то данные пользоателя записываются в БД, пользователь зарегистрирован.
Возвращаются токены.

```json
На вход принимает
{
  "email": "user@example.com",
  "password": "string",
  "passwordConfirm": "string",
  "firstName": "string",
  "lastName": "string",
  "patronymic": "string"
}
```
### api/account/registration/client-confirm
POST (Подтверждение регистрации клиента)
Пользователь(клиент) вводит код подтверждения и отправляет его на сервер. Если код подтвержден, то пользователь регистрируется.
```json
На вход принимает
{
  "code": "string"
}

Отдает
{
  "userId": 0,
  "userName": "string",
  "token": "string",
  "refreshToken": "string"
}
```
---
## Регистрация менеджера

### /api/account/registration/manager
POST (Регистрация Менеджера и организации)
Описание рпегистрации: Менеджер сначала заполняет данные о себе,
затем переходит на вторую страницу и заполняет информацию
о своей организации. Нажимает на Submit и все данные летят на сервер

```json
На вход принимает
{
  "userEmail": "user@example.com",
  "userPassword": "string",
  "userPasswordConfirm": "string",
  "userFirstName": "string",
  "userLastName": "string",
  "userPatronymic": "string",
  "organizationName": "string",
  "organizationINN": "string",
  "organizationPhoneNumber": "string",
  "organizationEmail": "user@example.com",
  "confirmDataPageUrl": "https://autobonus.ru/confirm"
}

отдает если произошла ошибка валидации или не отправилось письмо
{
  "error": "description"
}

Если письмо подтверждения отправлено и валидация пройдена, то отдает
{
"success" : "description"
}
```
### api/account/registration/manager-confirm
POST (Подтверждение регистрации при переходе по ссылке)
В ссылке в __GET__ параметрах будет параметр __token__.
Пользователь на странице вводит свой пароль, который он указывал при регистрации
(На случай если при регистрации он укажет ошибочный Email и
другой человек не смог подтвердить его регистрацию)
```json
На вход принимает
{
  "password": "string",
  "token": "string"
}

Отдает
{
  "userId": 0,
  "userName": "string",
  "token": "string",
  "refreshToken": "string"
}
```


---
## Профиль организации

### api/organization
GET (Получение данных организации для текущего пользователя)
```json
На вход принимает (только строка запроса, ничего в теле нет)
Определяет пользователя (МЕНЕДЖЕРА по его JWT и отдает организацию, которая за ним закреплена)

отдает
{
"inn": "string",
"name": "string",
"phoneNumber": "string",
"email": "string"
}
```


### api/organization
PUT (Изменение данных организации)
```json
На вход принимает
{
  "inn": "string",
  "name": "string",
  "phoneNumber": "string",
  "email": "string"
}

отдает
200 OK || 400 BadRequest
```

---
## Профиль пользователя

### api/user/profile
GET (Получение данных профиля пользователя)
```json
На вход принимает (только строка запроса, ничего в теле нет)
Определяет пользователя и отдает информацию по его профилю

отдает
{
"id": int,
"email": "string",
"phoneNumber": "string",
"firstName": "string",
"lastName": "string",
"patronymic": "string"
}
```

### api/user/profile
PUT (Изменение данных профиля пользователя)
```json
Получает
{
  "id": int,
  "email": "string",
  "phoneNumber": "string",
  "firstName": "string",
  "lastName": "string",
  "patronymic": "string"
}

отдает
200 Ok || 400 BadRequest
```




---
## CRUD СТО

### api/service-station
POST (Создание СТО)
```json
Получает
{
  "name": "СТО у Ашота",
  "address": "г.Ставрополь, ул.Ленина 154",
  "phoneNumber": "89123456789",
  "email" : "ashot@stostav.ru" //Необязательно
}

отдает
200 Ok || 400 BadRequest
```

### api/service-station/{stationId?}
GET (Получение СТО или списка всех СТО)
```json
отдает все СТО если не указывать {stationId}
[
  {
    "id": 2,
    "name": "СТО Routeam",
    "address": "г.Ставрополь, ул.Пушкина 12/2",
    "phoneNumber": "89123456789"
  },
  {
    "id": 5,
    "name": "СТО у Ашота",
    "address": "г.Ставрополь, ул.Ленина 154",
    "phoneNumber": "89123456789",
    "email": "ashot@stostav.ru"
  },
  {
    "id": 7,
    "name": "СТО у Ашота",
    "address": "г.Ставрополь, ул.Ленина 154",
    "phoneNumber": "89123456789"
  }
]

Если указать {stationId} отдает массив с одним элементом
Например: /api/service-station/7 запрос отдает
[
{
"id": 7,
"name": "СТО у Ашота",
"address": "г.Ставрополь, ул.Ленина 154",
"phoneNumber": "89123456789"
}
]
```

### api/service-station/{stationId}
PUT (изменение данных СТО)
```json
Получает
{
  "name": "СТО у Сереги",
  "phoneNumber": "89123456789",
  "email" : "serega@stostav.ru"
}

отдает
200 Ok || 400 BadRequest
```


### api/service-station/{stationId}
DELETE (удаление данных СТО)
```json
отдает
200 Ok || 400 BadRequest
```
---

## Справочник Марки и Модели обслуживаемых авто в СТО

### /api/search/auto?search=_{STRING_SEARCH}_&_type_={ null || "mark" || "model" }
GET (Поиск подходящих марок и моделей по строке)
```json
отдает
[
  {
    "markId": 58,
    "modelId": 632,
    "Mark": "Daewoo",
    "Model": "\"Chairman\""
  },
  {
    "markId": 58,
    "modelId": 633,
    "Mark": "Daewoo",
    "Model": "\"Damas\""
  },
  {
    "markId": 58,
    "modelId": 636,
    "Mark": "Daewoo",
    "Model": "\"G2X\""
  }
]
```


### api/service-station/{stationId}/auto
GET (Получаем справочник обслуживаемых марок и моделей авто)
```json
отдает
200 Ok || 400 BadRequest
```


### api/service-station/{stationId}/auto/mark
POST (Добавление марки авто в справочник СТО)
Просто добаляем марку, никакие модели не добавляет.
```json
Получает
{
  "markId" : int
}

отдает
200 Ok || 400 BadRequest
```


### api/service-station/{stationId}/auto/mark/{serviceMarkId}
DELETE (Удаление марки авто в справочник СТО)  
Удаляем марку и все модели каскадно в справочнике
```json
отдает
200 Ok || 400 BadRequest
```


### api/service-station/{stationId}/auto/model
POST (Добавление модели авто в справочник СТО)  
Если в БД не существовало марки для этой модели,
то марка привяжется к справочнику СТО автоматически.
```json
Получает
{
  "modelId" : int
}

отдает
200 Ok || 400 BadRequest
```


### api/service-station/{stationId}/auto/model/{serviceMarkId}
DELETE (Удаление модели авто в справочник СТО)
Удаляем модель в справочнике
```json
отдает
200 Ok || 400 BadRequest
```
---
# Сущности справочников
## Справочник приложения МАРКИ (CRUD)
### api/dictionary/auto/mark
POST (Добавление сущности в справочник)
```json
принимает
{
  "name": "string"
}
отдает
200 Ok || 400 BadRequest
```
### api/dictionary/auto/mark?_pageNumber_=1&_pageSize_=20
GET (Получение сущностей из справочника с PAGINATION)
```json
отдает
{
  "pageNumber": 1,
  "pageSize": 3,
  "totalRecordCount": 288,
  "records": [
    {
      "id": 1,
      "name": "AC"
    },
    {
      "id": 2,
      "name": "Acura"
    },
    {
      "id": 3,
      "name": "Adler"
    }
  ]
}
```

### api/dictionary/auto/mark/{markId}
GET (Получение одной сущности из справочника)
```json
отдает
{
  "id": 1,
  "name": "AC"
}
```

### api/dictionary/auto/mark/{markId}
PUT (Изменение сущности)
```json
Принимает
{
  "name": "string"
}
```

### api/dictionary/auto/mark/{markId}
DELETE (Удаление сущности)
```json
отдает
200 Ok || 400 BadRequest
```

---

## Справочник приложения МОДЕЛИ (CRUD)
### api/dictionary/auto/model
POST (Добавление сущности в справочник)
```json
принимает
{
  "markId": 0,
  "name": "string"
}
отдает
200 Ok || 400 BadRequest
```
### api/dictionary/auto/model?_pageNumber_=1&_pageSize_=20
GET (Получение сущностей из справочника с PAGINATION)
```json
отдает
{
  "pageNumber": 1,
  "pageSize": 3,
  "totalRecordCount": 3302,
  "records": [
    {
      "id": 3,
      "name": "\"Aceca\"",
      "mark": {
        "id": 1,
        "name": "AC"
      }
    },
    {
      "id": 2,
      "name": "\"Ace\"",
      "mark": {
        "id": 1,
        "name": "AC"
      }
    },
    {
      "id": 1,
      "name": "\"378 GT Zagato\"",
      "mark": {
        "id": 1,
        "name": "AC"
      }
    }
  ]
}
```

### api/dictionary/auto/model/{modelId}
GET (Получение одной сущности из справочника)
```json
отдает
{
  "id": 567,
  "name": "\"C4 Aircross\"",
  "mark": {
    "id": 51,
    "name": "Citroen"
  }
}
```

### api/dictionary/auto/model/{modelId}
PUT (Изменение сущности)
```json
Принимает
{
  "markId": 0,
  "name": "string"
}
```

### api/dictionary/auto/model/{modelId}
DELETE (Удаление сущности)
```json
отдает
200 Ok || 400 BadRequest
```


## Справочник приложения РАБОТЫ/УСЛУГИ (CRUD)
### api/dictionary/work
POST (Добавление сущности в справочник)
```json
принимает
{
  "name": "string",
  "description": "string"
}
отдает
200 Ok || 400 BadRequest
```
### api/dictionary/work?_pageNumber_=1&_pageSize_=20
GET (Получение сущностей из справочника с PAGINATION)
```json
отдает
{
  "pageNumber": 1,
  "pageSize": 20,
  "totalRecordCount": 3,
  "records": [
    {
      "id": 1,
      "name": "Капитальный ремонт двигателя",
      "description": "Замена двигателя!"
    },
    {
      "id": 2,
      "name": "Замена масла",
      "description": "Быстрая процедура, займет 30 минут"
    },
    {
      "id": 3,
      "name": "Покраска авто",
      "description": "Покрасим ваше авто в любой цвет."
    }
  ]
}
```

### api/dictionary/work/{modelId}
GET (Получение одной сущности из справочника)
```json
отдает
{
  "id": 2,
  "name": "Замена масла",
  "description": "Быстрая процедура, займет 30 минут"
}
```

### api/dictionary/work/{modelId}
PUT (Изменение сущности)
```json
Принимает
{
  "name": "string",
  "description": "string"
}
```

### api/dictionary/work/{modelId}
DELETE (Удаление сущности)
```json
отдает
200 Ok || 400 BadRequest
```

## Справочник приложения СТАТУСЫ ЗАЯВОК
### api/dictionary/request-status
GET (Получить все статусы заявок)
```json
отдает
[
  {
    "id": 1,
    "name": "new",
    "alias": "Новая заявка"
  },
  {
    "id": 2,
    "name": "work",
    "alias": "Заявка в работе"
  },
  {
    "id": 3,
    "name": "canceled",
    "alias": "Заявка отменена"
  },
  {
    "id": 4,
    "name": "closed",
    "alias": "Заявка обработана"
  }
]
```
---
## ЗАЯВКИ НА РЕМОНТЫ
### api/repair
POST (Создать заявку на ремонт)
```json
принимает
{
  "description": "string",
  "clientId": 0,
  "autoId": 0,
  "stationId": 0
}
отдает
200 Ok || 400 BadRequest
```

### api/repair/{repairId}
PUT (Изменить заявку (поменять статус или описание или время окончания))
```json
принимает
{
  "endTime": "2021-10-23T16:51:25.479Z",
  "description": "string",
  "statusId": 0
}
отдает
200 Ok || 400 BadRequest
```

### api/repair/{repairId}
DELETE (Удалить заявку)
```json
отдает
200 Ok || 400 BadRequest
```
### api/repair/{repairId}
GET (Полная и подробная информация о ремонте)
```json
отдает
{
  "id": 1,
  "createdTime": "2021-10-22T09:38:57.67627",
  "description": "Двигатель не заводится",
  "client": {
    "clientId": 1,
    "email": "gawadi5940@bomoads.com",
    "firstName": "Тимофей",
    "lastName": "Буркуш",
    "patronymic": "Эдуардович"
  },
  "auto": {
    "createdTime": "2021-10-22T12:37:40.392671",
    "registrationNumber": "Е001КХ26",
    "mileAge": 1000,
    "id": 1,
    "mark": "Lulya",
    "model": "Kebab",
    "year": 2025,
    "vin": "123456235904223534"
  },
  "status": {
    "id": 1,
    "name": "new",
    "alias": "Новая заявка"
  },
  "details": [
    {
      "id": 1,
      "repairId": "1",
      "work": {
        "id": 2,
        "name": "Замена лобового стекла",
        "description": "Описание замена лобового стекла"
      },
      "description": "Делов на пару сек"
    },
    {
      "id": 2,
      "repairId": "1",
      "work": {
        "id": 3,
        "name": "Замена колеса",
        "description": "Описание замена колеса"
      },
      "description": "Иваныч долго мудохался с этим"
    }
  ]
}
```

### /api/station/{stationId}/repair/preview
GET (Preview (Количество заявок по статусам (ПЛАШКА)))
```json
отдает
[
  {
    "count": 3,
    "id": 1,
    "name": "new",
    "alias": "Новая заявка"
  },
  {
    "count": 12,
    "id": 2,
    "name": "record",
    "alias": "Запись на ремонт"
  },
  {
    "count": 1,
    "id": 3,
    "name": "work",
    "alias": "Заявка в работе"
  },
  {
    "count": 7,
    "id": 4,
    "name": "closed",
    "alias": "Заявка обработана"
  },
  {
    "count": 4,
    "id": 5,
    "name": "canceled",
    "alias": "Заявка отменена"
  }
]
```

### api/station/{stationId}/repair?pageNumber=1&pageSize=20;filter={STATUS_NAME || NULL}
GET (Получить заявки СТО с пагинацией и фильтрацией (тут не полная инфа о заявке, модель для таблицы))
```json
{
  "pageNumber": 1,
  "pageSize": 20,
  "totalRecordCount": 2,
  "records": [
    {
      "id": 5,
      "createdTime": "2021-10-22T06:40:41.917939",
      "status": {
        "id": 1,
        "name": "new",
        "alias": "Новая заявка"
      },
      "description": "Что-то стучит",
      "auto": {
        "id": 4,
        "mark": "BURTIMAX",
        "model": "Kalina",
        "year": 0,
        "vin": "11111111111111111"
      }
    },
    {
      "id": 6,
      "createdTime": "2021-10-22T06:44:17.219483",
      "status": {
        "id": 3,
        "name": "work",
        "alias": "Заявка в работе"
      },
      "description": "проблема в двигателе",
      "auto": {
        "id": 5,
        "mark": "Lulya",
        "model": "Kebab",
        "year": 2025,
        "vin": "123456235904223534"
      }
    }
  ]
}
```
---

## ДЕТАЛИЗАЦИЯ РЕМОНТА (УСЛУГИ И РАБОТЫ РЕМОНТА)
### api/repair/{repairId}/detail
POST (Добавить услугу/работу в ремонт)
```json
принимает
{
  "workId": 0, //из справочника
  "description": "string"
}
отдает
200 Ok || 400 BadRequest
```

### api/repair/{repairId}/detail/{detailId}
DELETE (Удалить услугу/работу из ремонта)
```json
отдает
200 Ok || 400 BadRequest
```

### api/repair/{repairId}/detail/{detailId}
PUT (Изменить услугу/работу в ремонте)
```json
принимает
{
  "workId": 0,
  "description": "string"
}
отдает
200 Ok || 400 BadRequest
```
---

## АВТО CRUD
### api/auto
POST (Добавить авто)
```json
принимает
{
  "mark": "string",
  "model": "string",
  "year": 0,
  "vin": "string",
  "registrationNumber": "string",
  "mileAge": 0
}
отдает
200 Ok || 400 BadRequest
```

### api/auto/{autoId}
GET (Получить авто)
```json
отдает
{
  "createdTime": "2021-10-22T12:40:11.979135",
  "registrationNumber": "Е777КХ777",
  "mileAge": 35000,
  "id": 2,
  "mark": "Toyota",
  "model": "Carolla",
  "year": 2020,
  "vin": "123456211111223534"
}
```

### api/auto/{autoId}
PUT (Изменить авто)
```json
принимает
{
  "id": 0,
  "mark": "string",
  "model": "string",
  "year": 0,
  "vin": "string",
  "registrationNumber": "string",
  "mileAge": 0
}
отдает
200 || 400
```

### api/auto/{autoId}
DELETE (Удалить авто)
```json
отдает
200 || 400
```

---

## ПРИВЯЗКА АВТО К КЛИЕНТУ (УПРАВЛЕНИЕ)
### api/client/{clientId}/auto
POST (Добавить авто и привязать к клиенту)
```json
принимает
{
  "mark": "Tesla",
  "model": "Model X",
  "year": 2020,
  "vin": "12345671237654321",
  "registrationNumber": "Н543ЕР126",
  "mileAge": 30000
}
отдает
200 Ok || 400 BadRequest
```

### api/client/{clientId}/auto
GET (Получить все авто клиента)
```json
отдает
[
  {
    "createdTime": "2021-10-21T15:14:26.834902",
    "registrationNumber": "Т003ИМ126",
    "mileAge": 40000,
    "id": 4,
    "mark": "BURTIMAX",
    "model": "Kalina",
    "year": 0,
    "vin": "11111111111111111"
  },
  {
    "createdTime": "2021-10-22T09:41:23.645615",
    "registrationNumber": "Е001КХ26",
    "mileAge": 1000,
    "id": 5,
    "mark": "Lulya",
    "model": "Kebab",
    "year": 2025,
    "vin": "123456235904223534"
  }
]
```

### api/client/{clientId}/auto/{autoId}
GET (Получить авто клиента)
```json
отдает
{
    "createdTime": "2021-10-21T15:14:26.834902",
    "registrationNumber": "Т003ИМ126",
    "mileAge": 40000,
    "id": 4,
    "mark": "BURTIMAX",
    "model": "Kalina",
    "year": 0,
    "vin": "11111111111111111"
}
```

### api/client/{clientId}/auto/{autoId}
PUT (Привязка существующего авто к клиенту)
```json
отдает
200 || 400
```

### api/client/{clientId}/auto/{autoId}
DELETE (Отвязка авто от клиента)
```json
отдает
200 || 400
```
---

## AUTO HISTORY
### api/auto/{autoId}/repair-history
POST (Получить историю ремонтов автомобиля (получает полную информацию о каждом ремонте авто!))
```json
отдает
[
  {
    "id": 5,
    "createdTime": "2021-10-22T06:40:41.917939",
    "description": "Что-то стучит",
    "client": {
      "clientId": 2,
      "email": "timofeyburkush@gmail.com",
      "firstName": "Timofey",
      "lastName": "Burkush",
      "patronymic": "Эдуардович"
    },
    "auto": {
      "createdTime": "2021-10-21T15:14:26.834902",
      "registrationNumber": "Т003ИМ126",
      "mileAge": 40000,
      "id": 4,
      "mark": "BURTIMAX",
      "model": "Kalina",
      "year": 0,
      "vin": "11111111111111111"
    },
    "status": {
      "id": 4,
      "name": "closed",
      "alias": "Заявка обработана"
    },
    "details": []
  }
]
```

### api/auto/{autoId}/owner-history
POST (Получить историю владельцев автомобиля)
```json
отдает
[
  {
    "startTime": "2021-10-25T08:03:58.064185",
    "userId": 2,
    "userFirstName": "Timofey",
    "userLastName": "Burkush",
    "userPatronymic": "Эдуардович",
    "autoId": 7,
    "autoMark": "Tesla",
    "autoModel": "Model X",
    "autoVIN": "12345671237654321"
  },
  {
    "startTime": "2021-10-25T07:59:59.814112",
    "endTime": "2021-10-25T08:03:58.064185",
    "userId": 2,
    "userFirstName": "Timofey",
    "userLastName": "Burkush",
    "userPatronymic": "Эдуардович",
    "autoId": 7,
    "autoMark": "Tesla",
    "autoModel": "Model X",
    "autoVIN": "12345671237654321"
  }
]
```
