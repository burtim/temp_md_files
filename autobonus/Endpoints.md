#Endpoints AutoBonus

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
  "confirmDataPageUrl": "string"
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

###api/user/profile
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

