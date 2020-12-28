# Дипломный проект курса "NDSE - Настройка окружения и Express.js"

Дипломный проект представляет собой службу доставки. Ваша задача заключается в создании работающего бэкэнд-приложения, всеми основными функциями которого можно пользоваться.

## Содержание

1. Приложение должно содержать следующие **базовые** модули:
- 1.1 Пользователи
- 1.2 Объявления
- 1.3 Чат

2. Приложение должно содержать следующие **функциональные** модули:
- 2.1 Регистрация
- 2.2 Аутентификация
- 2.3 Просмотр объявлений
- 2.4 Управление объявлениями
- 2.5 Общение

## 1. Описание базовых модулей

Базовые модули служат для описания бизнес-логики и хранения данных.

### 1.1 Модуль "Пользователи"

Модуль пользователи предназначается для хранения и поиска профилей пользователей.

Модуль пользователи используется функциональными модулями для регистрации и аутентификации.

Данные пользователя должны храниться в MongoDb.

Модель данных пользователя должна содержать следующие поля:

| название     |   тип    | обязательное |
| ------------ | :------: | :----------: |
| email        | `string` |      да      |
| password     | `string` |      да      |
| name         | `string` |      да      |
| contantPhone | `string` |     нет      |

_! уточнить список полей_

#### 1.1.1 Создание пользователя

```js
const user = await UserModule.create(data);
```

_! описать формат данных_

#### 1.1.2 Поиск пользователя

```js
const user = await UserModule.findByEmail(email);
```

_! описать формат данных_

### 1.2 Модуль "Объявления"

Модуль пользователи предназначается для хранения и поиска объявлений.

Модуль объявлений используется функциональными модулями для показа списка объявлений и для размещения и удаления объявлений.

Данные объявлений должны храниться в MongoDb.

Модель данных объявления должна содержать следующие поля:

| название    |    тип     | обязательное |
| ----------- | :--------: | :----------: |
| shortText   |  `string`  |      да      |
| description |  `string`  |     нет      |
| images      | `string[]` |     нет      |
| userId      | `ObjectId` |      да      |
| createdAt   |   `Date`   |      да      |
| tags        | `string[]` |     нет      |

_! уточнить список полей_

#### 1.2.1 Поиск объявлния

```js
const advertisements = await Advertisement.find(params);
```

_! уточнить список параметров_

#### 1.2.2 Создание объявления

```js
const advertisement = await Advertisement.create(params);
```

_! уточнить список параметров_

#### 1.2.3 Удаление объявления

```js
const advertisement = await Advertisement.remove(id);
```

_! уточнить список параметров_

### 1.3 Модуль "Чат"

Модуль "чат" предназначается для хранения чатов и сообщений в чате.

Модуль объявлений используется функциональными модулями для реализации возможности общения пользователей.

Данные чатов должны храниться в MongoDb.

Модель данных чата должна содержать следующие поля:

| название  |           тип            | обязательное |
| --------- | :----------------------: | :----------: |
| users     | [`ObjectId`, `ObjectId`] |      да      |
| createdAt |          `Date`          |      да      |
| messages  |       `Message[]`        |     нет      |

Модель сообщения (Message) должна содержать следующие поля:

| название |    тип     | обязательное |
| -------- | :--------: | :----------: |
| author   | `ObjectId` |      да      |
| sentAt   |   `Date`   |      да      |
| text     |  `string`  |      да      |
| readAt   |   `Date`   |     нет      |

_! уточнить список полей_

### 1.3.1 Отправить сообщение

```js
const message = await Chat.sendMessage(data);
```

_! уточнить список параметров_

### 1.3.2 Подписаться на новые сообщения в чате

```js
Chat.subscribe((message) => {});
```

### 1.3.3 Получить историю сообщений чата

```js
const messages = await Chat.getHistory(params);
```

_! уточнить список параметров_

## 2. Описание функциональных модулей

Функциональные модули предназначены для реализации функций, доступных конечным пользователям.

### 2.1 Регистрация

`POST /api/signup` — зарегистрироваться

Пароль не должен храниться в чистом виде. Его перед отправкой в модуль "Пользователи" нужно хешировать.

Формат данных при отправке `json-объект`. Пример запроса

```json
{
  "email": "kulagin@netology.ru",
  "password": "ad service",
  "name": "Alex Kulagin",
  "contantPhone": "+7 123 456 78 90"
}
```

В ответ приходит либо сообщение об ошибке, либо JSON-объект с данными

```json
{
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "email": "kulagin@netology.ru",
    "name": "Alex Kulagin",
    "contantPhone": "+7 123 456 78 90"
  },
  "status": "ok"
}
```

### 2.2 Аутентификация

`POST /api/signin` — залогиниться

Для реализации аутентификации пользователя должен использоваться механизм сессий и модуль `Passport JS`.

Если пользователь не существует или пароль не совпадает, то нужно выдавать ошибку `Неверный логин или пароль`.

Так как пароль не должен храниться в чистом виде, то его нужно хешировать и сравнивать с хэшем пароля пользователя из модуля "Пользователи".

Формат данных при отправке `json-объект`. Пример запроса:

```json
{
  "email": "kulagin@netology.ru",
  "password": "ad service"
}
```

В ответ приходит либо сообщение об ошибке, либо `json-объект` с данными:

```json
{
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "email": "kulagin@netology.ru",
    "name": "Alex Kulagin",
    "contantPhone": "+7 123 456 78 90"
  },
  "status": "ok"
}
```

```json
{
  "error": "Неверный логин или пароль",
  "status": "error"
}
```

### 2.3 Просмотр объявлений

`GET /api/advertisements` — получить список объявлений.

Эти данные публичные, поэтому аутентификация не требуется.

В ответ приходит либо сообщение об ошибке, либо `json-объект` с данными:

```json
{
  "data": [
    {
      "id": "507f1f77bcf86cd799439012",
      "shortTitle": "Продам слона",
      "description": "kulagin@netology.ru",
      "images": [
        "/uploads/507f1f77bcf86cd799439011/slon_v_profil.jpg",
        "/uploads/507f1f77bcf86cd799439011/slon_v_fas.jpg",
        "/uploads/507f1f77bcf86cd799439011/slon_hobot.jpg"
      ],
      "user": {
        "id": "507f1f77bcf86cd799439011",
        "name": "Alex Kulagin"
      },
      "createdAt": "2020-12-12T10:00:00.000Z"
    }
  ],
  "status": "ok"
}
```

`GET /api/advertisements/:id` — получить данные объявления

Эти данные публичные, поэтому аутентификация не требуется.

В ответ приходит либо сообщение об ошибке, либо `json-объект` с данными:

```json
{
  "data": {
    "id": "507f1f77bcf86cd799439012",
    "shortTitle": "Продам слона",
    "description": "kulagin@netology.ru",
    "images": [
      "/uploads/507f1f77bcf86cd799439011/slon_v_profil.jpg",
      "/uploads/507f1f77bcf86cd799439011/slon_v_fas.jpg",
      "/uploads/507f1f77bcf86cd799439011/slon_hobot.jpg"
    ],
    "user": {
      "id": "507f1f77bcf86cd799439011",
      "name": "Alex Kulagin"
    },
    "createdAt": "2020-12-12T10:00:00.000Z"
  },
  "status": "ok"
}
```

### 2.4 Управление объявлениями

`POST /api/advertisements` — получить список объявлений

Эти данные приватные и требуют проверки аутентификации

Формат данных при отправке `FormData`. Пример запроса:

| Поле        | Тип      |
| ----------- | -------- |
| shotrTitle  | `string` |
| description | `string` |
| images      | File[]   |

Обработка загруженных файлов должна производиться с помощью библиотеки multer.

В ответ приходит либо сообщение об ошибке, либо `json-объект` с данными

```json
{
  "data": [
    {
      "id": "507f1f77bcf86cd799439012",
      "shortTitle": "Продам слона",
      "description": "kulagin@netology.ru",
      "images": [
        "/uploads/507f1f77bcf86cd799439011/slon_v_profil.jpg",
        "/uploads/507f1f77bcf86cd799439011/slon_v_fas.jpg",
        "/uploads/507f1f77bcf86cd799439011/slon_hobot.jpg"
      ],
      "user": {
        "id": "507f1f77bcf86cd799439011",
        "name": "Alex Kulagin"
      },
      "createdAt": "2020-12-12T10:00:00.000Z"
    }
  ],
  "status": "ok"
}
```

Если пользователь не аутентифицирован и пытается создать объявление, то в ответ должен получить `json-объект` с ошибкой и HTTP код 401.

`DELETE /api/advertisements/:id` — удалить объявление

Эти данные приватные и требуют проверки аутентификации.

Если пользователь не аутентифицирован и пытается создать объявление, то в ответ должен получить `json-объект` с ошибкой и HTTP код 401.

Если пользователь аутентифицирован, но не является автором объявления, то в ответ должен получить `json-объект` с ошибкой и HTTP код 403.

### 2.5 Общение

Модуль общение предназначен для онлайн общения пользователей.

Модуль должен использовать библиотеку socket.io.

Для подписки на обновления в чатах модуль должен использовать функционал подписка модуля "Чат".

Сообщение приходящие в `socket`:

- `getHistory` - получить историю сообщений из чата
- `sendMessage` - отправить сообщение пользователю

События отправляемые через `socket`:

- `newMessage` - отправлено новое сообщение

_! уточнить список данных_
