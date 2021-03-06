# 1C API
***
## Процедуры с автомобилями

### Добавить автомобиль и привязать к владельцу. ClientId сообщает сам клиент.
POST (api/1c/auto?stationId={STATION_ID})
```json lines
//принимает
{
  "clientId" : 0, //Идентификатор клиента
  "mark": "string", //*** Марка авто (производитель)
  "model": "string", //*** Модель авто
  "year": 0, //Год производства
  "vin": "string", //*** VIN
  "registrationNumber": "string", //регистрационный номер
  "mileAge": 0, //Пробег авто
}
//отдает созданный объект
{
  "Id" : 0, //система выдает уникальный ID
  "mark": "string", //*** Марка авто (производитель)
  "model": "string", //**
  * Модель авто
  "year": 0, //Год производства
  "vin": "string", //*** VIN
  "registrationNumber": "string", //регистрационный номер
  "mileAge" : 0, //необязательное поле, может не быть информации о пробеге
}

//В случае ошибки отдает код 400 и строку ошибки
```

### Получить автомобили клиента или автомобиль
GET (api/1c/auto?stationId={STATION_ID}&clientId={CLIENT_ID}&vin={AUTO_VIN})
```json
//Удачная операция отдает код 200 и результат (не все параметры могут прийти!)
  [
    {
      "Id" : 0, //система выдает уникальный ID
      "mark": "string", //*** Марка авто (производитель)
      "model": "string", //*** Модель авто
      "year": 0, //Год производства
      "vin": "string", //*** VIN
      "registrationNumber": "string", //регистрационный номер
      "mileAge" : 0 //необязательное поле, может не быть информации о пробеге
    },
    {
      "Id" : 0, //система выдает уникальный ID
      "mark": "string", //*** Марка авто (производитель)
      "model": "string", //*** Модель авто
      "year": 0, //Год производства
      "vin": "string", //*** VIN
      "registrationNumber": "string", //регистрационный номер
      "mileAge" : 0 //необязательное поле, может не быть информации о пробеге
    }
  ]

//Если указан параметр AUTO_ID то отдает
//отдает (не все параметры могут прийти! Например, нет отчества или клиент не заполнил имя и фамилию)
{
  "Id" : 0, //система выдает уникальный ID
  "mark": "string", //*** Марка авто (производитель)
  "model": "string", //*** Модель авто
  "year": 0, //Год производства
  "vin": "string", //*** VIN
  "registrationNumber": "string", //регистрационный номер
  "mileAge" : 0 //необязательное поле, может не быть информации о пробеге
}

//В случае ошибки отдает код 400 и строку ошибки
```


### Изменить характеристики авто.
PUT (api/1c/auto?stationId={STATION_ID}&vin={AUTO_VIN})
> Поля необязательны, следует отправлять только изменяемые поля
```json
//принимает
{
  "mark": "string", //Марка авто (производитель)
  "model": "string", //Модель авто
  "year": 0, //Год производства
  "vin": "string", //VIN
  "registrationNumber": "string", //Регистрационный номер
  "mileAge" : 0 //пробег
}
 
//Удачная операция отдает код 200 и результат
{
  "Id" : 0, //система выдает уникальный ID
  "mark": "string", //*** Марка авто (производитель)
  "model": "string", //*** Модель авто
  "year": 0, //Год производства
  "vin": "string", //*** VIN
  "registrationNumber": "string", //регистрационный номер
  "mileAge" : 0 //необязательное поле, может не быть информации о пробеге
}

//В случае ошибки отдает код 400 и строку ошибки
```

### Удалить авто
PUT (api/1c/auto?stationId={STATION_ID}&vin={AUTO_VIN})
```json
//Удачная операция отдает код 200

//В случае ошибки отдает код 400 и строку ошибки
```

### Передать авто другому 
клиенту
PUT (api/1c/auto/bind?stationId={STATION_ID}&vin={AUTO_VIN}&clientId={CLIENT_ID})
```json
//Удачная операция отдает код 200

//В случае ошибки отдает код 400 и строку ошибки
```

## Процедуры для работы с владельцами (КЛИЕНТАМИ)

### Добавление владельца (Клиента)
> Пользователь мобильного приложения (клиент) уже автоматически является владельцем, его добавлять не нужно

### Удаление владельца (Клиента)
> Нельзя сделать этот метод, так как придется удалить аккаунт клиента.


### Получить владельца (клиента)
GET (api/1c/client?stationId={STATION_ID}&clientId={CLIENT_ID})
```json
//Отдает 200 и
  {
    "clientId": 2, 
    "email": "ivan@gmail.com",
    "phoneNumber": "89881234567",
    "firstName": "Иван",
    "lastName": "Иванов",
    "patronymic": "Иванович"
  }

//В случае ошибки отдает код 400 и строку ошибки
```

## Процедуры для работы с заказ-нарядами

### Отправить заказ-наряд (добавить/изменить) 
POST (api/1c/repair)  
//Если существует заказ-наряд, у которого DocumentId совпадает, то заказ-наряд будет изменен,  
//Если не существует, то будет добавлен.
```json
//принимает
{
  "repairRequestId" : "string", //необязательный параметр (заказ-наряд может быть создан без заявки)
  "clientId" : 0, //УИН клиента
  "vin" : "string", //VIN авто
  "stationId" : "string", //Идентификатор СТО
  "documentId" : "string", //Уникальный идентификатор документа
  "totalSum" : 0, //Общая сумма ремонта
  "comment" : "string", //комментарий к заказ-наряду
  "mileAge" : 0, //пробег
  "materials" : [ //табличная часть (товары или материалы)
    {
      "name" : "string", //наименование товара
      "vendorCode" : "string", //артикул товара
      "manufacturer" : "string", //производитель
      "amount" : 0, //количество
      "price" : 0, //цена товара (материала)
      "sum" : 0 //Конечная стоимость товара
    },
    {
      "name" : "string", //наименование товара
      "vendorCode" : "string", //артикул товара
      "manufacturer" : "string", //производитель
      "amount" : 0, //количество
      "price" : 0, //цена товара (материала)
      "sum" : 0 //Конечная стоимость товара
    },
    ... //Другие товары
  ],
  "works" : [
    {
      "name" : "string", //наименование работы
      "amount" : 0, //количество
      "coefficient" : 0.5, //коэффициент
      "price" : 0, //цена работы
      "sum" : 0 //Конечная стоимость работы
    },
    {
      "name" : "string", //наименование работы
      "amount" : 0, //количество
      "coefficient" : 0.5, //коэффициент
      "price" : 0, //цена работы
      "sum" : 0 //Конечная стоимость работы
    },
    ... //Другие работы
  ]
}
//Отдает код 200

//В случае ошибки отдает код 400 и строку ошибки
```

### Удалить заказ-наряд
DELETE (api/1c/repair?stationId={STATION_ID}&documentId={DOCUMENT_ID})
```json
//Удачная операция отдает код 200

//В случае ошибки отдает код 400 и строку ошибки
```

### Получение новых заявок на ремонт.
GET (api/1c/request/new?stationId={STATION_ID})
```
[
    {
        "id": 4,
        "createdTime": "2021-11-03T09:17:26.684741",
        "repairMileAge": 0,
        "client": {
            "clientId": 2,
            "email": "Kdiyor@mail.ru",
            "phoneNumber": "+7 (962) 331-00-33",
            "firstName": "Diyor",
            "lastName": "Khanazarov",
            "patronymic": "Shonazarovich"
        },
        "auto": {
            "createdTime": "2021-10-31T19:42:59.475664",
            "registrationNumber": "A872SH 77",
            "id": 13,
            "mark": "AMC",
            "model": "Hornet",
            "year": 2020,
            "vin": "KJDHUS932917371JB"
        },
        "status": {
            "id": 1,
            "name": "new",
            "alias": "Новая заявка"
        }
    }
    ...
]
```

### Создание заявки на ремонт.
GET (api/1c/request/new?stationId={STATION_ID})
```json
{
    "description" : "string",
    "clientId" : 0,
    "vin" : "string"
}
```


## Операции по программе лояльности

### Получить настройки программы лояльности СТО
GET (api/1c/loyalty/settings?stationId={STATION_ID})
```json
//Удачная операция отдает код 200
{
  "stationId": 3,
  "debitPercent": 20,
  "creditPercent": 10,
  "lifeDays": 7
}

//В случае ошибки отдает код 400 и строку ошибки
```

### Получить баланс счета лояльности клиента для СТО.
GET (api/1c/loyalty/balance?stationId={STATION_ID}&clientId={ClientId})
```json
//Удачная операция отдает код 200
{
  "date": "2021-11-19T00:00:00+03:00",
  "balance": 0
}

//В случае ошибки отдает код 400 и строку ошибки
```

### Начислить/Списать со счета лояльности клиента для СТО.
POST (api/1c/loyalty/operation)
```json
//Принимает
{
  "stationId" : 0,
  "clientId" : 0,
  "documentId" : "string", //необязательное поле (идентификатор документа 1С)
  "description" : "string", //Описание операции
  "value": 330, //Сумма операции (Положительное число)
  "isAddOperation": true, //Тип операции (true - начисление/ false - списание)
  "lifeDays": 1 //Это поле при начислении бонусов, Вреся жизни бонусов в днях.
}

//Удачная операция отдает код 200

//В случае ошибки отдает код 400 и строку ошибки
```

### Получить историю операций по счету лояльности клиента для СТО. (С пагинацией)
GET (api/1c/loyalty/operation/history?pageNumber=1&pageSize=20&clientId=2&stationId=2)
```json
//Удачная операция отдает код 200
{
  "pageNumber": 1,
  "pageSize": 20,
  "totalRecordCount": 6,
  "records": [
    {
      "description": "Начисление баллов",
      "change": 500,
      "date": "2021-11-16T09:02:03.950216+03:00",
      "documentId" : 0 //Необязательное поле, операция может быть не привязана к документу.
    },
    {
      "description": "Начисление баллов",
      "change": 500,
      "date": "2021-11-16T09:02:33.09406+03:00"
    },
    {
      "description": "Списание баллов",
      "change": -300,
      "date": "2021-11-16T09:02:54.02801+03:00",
      "documentId" : 0 //Необязательное поле, операция может быть не привязана к документу.
    },
    {
      "description": "Начисление баллов",
      "change": 180,
      "date": "2021-11-16T09:03:18.686328+03:00"
    },
    {
      "description": "Списание баллов",
      "change": -279,
      "date": "2021-11-16T09:04:03.155741+03:00"
    },
    {
      "description": "Списание баллов",
      "change": -139,
      "date": "2021-11-16T09:07:50.820007+03:00",
      "documentId" : 0 //Необязательное поле, операция может быть не привязана к документу.
    }
  ]
}

//В случае ошибки отдает код 400 и строку ошибки
```

## Добавление профиля клиента (Регистрация через СТО)
> Регистрация через СТО проходит по следующему алгоритму.
1) Клиент в мобильном приложении проходит быструю регистрацию (Без указания ФИО, без добавления авто). Пользователь вводит только ЛОГИН(телефон или емайл) и ПАРОЛЬ. Затем подтверждает свой телефон или емайл и получает уникальный ключ ClientId.
2) Клиент сообщает свой ClientId оператору 1С
3) Оператор 1С заполняем всю информацию о клиенте, затем с помощью ClientId добавляет всю информацию в профиль клиента (отправляет серверу).
4) Регистрация через СТО прошла успешно, все данные синхронизированы.

### Добавить всю информацию о клиенте в одном запросе.
Добавить ФИО, Автомобили, Телефон, Email, Стартовый баланс по счету лояльности.
POST (api/1c/client?stationId={STATION_ID}&clientId={ClientId})
```json
//Принимает (Все поля необязательны, какая информация придет, такую и сохраним.)
{
  "firstName" : "string", //Имя
  "lastName" : "string", //Фамилия
  "patronymic" : "string", //Отчество
  "email" : "string", //Емайл
  "phoneNumber" : "string", //Телефон
  "startBalance" : 0, //Начислить стартовые баллы на счет лояльности 
  "autos" : [
    {
      "mark": "string", //*** Марка авто (производитель)
      "mark": "string", //*** Марка авто (производитель)
      "model": "string", //*** Модель авто
      "year": 0, //Год производства
      "vin": "string", //*** VIN
      "registrationNumber": "string", //регистрационный номер
      "mileAge": 0, //Пробег авто
    },
    {
      "mark": "string", //*** Марка авто (производитель)
      "model": "string", //*** Модель авто
      "year": 0, //Год производства
      "vin": "string", //*** VIN
      "registrationNumber": "string", //регистрационный номер
      "mileAge": 0, //Пробег авто
    }
  ]
}


//В случае ошибки отдает код 400 и строку ошибки
```

## Авторизация для работы с сервисом  
> Прежде чем отправлять запросы сервису, необходимо авторизоваться.  
> Авторизация основана на JWT токенах.  
> Jwt token истекает через 24 часа. RefreshToken истекает через 168 часов.
> Для осуществление запросов процедур необходимо добавить заголовок авторизации (Bearer [token])
> Ниже описаны методы авторизации.

### Авторизация пользователя
POST (/api/account/login)
```json
//На вход принимает
{
  "email": "string",
  "password": "string"
}

//отдает
{
  "userId": 0,
  "userName": "string",
  "token" : "string",
  "refreshToken": "string"
}

//В случае ошибки отдает код 400 и строку ошибки
```

### Выход пользователя
POST (api/account/logout)
```json
//Удачная операция отдает код 200

//В случае ошибки отдает код 400 и строку ошибки
```

### Обновить токены (когда jwt истек, а refresh еще нет, чтобы заново не авторизовываться)
POST (api/account/refresh-tokens)
```json
//На вход принимает
{
  "refreshToken": "string"
}

//отдает
{
  "refreshToken": "string",
  "jwtToken": "string"
}

//В случае ошибки отдает код 400 и строку ошибки
```
