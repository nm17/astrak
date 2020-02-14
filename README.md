# astrak-api

#### Мы используем REST в API.

## Работа с Callback для получения событий

Для начала необходимо зарегистрировать наш сервер. Делаем POST запрос на **/events/callback/start** с полями в body:

| Поле     | Тип    | Описание                                |
|----------|--------|-----------------------------------------|
| token    | String | Токен пользователя                      |
| callback | String | URL, на который будут приходить события |

Используем URL в формате https://ip:port/ для вашего же удобства. Вам, конечно, никто не запрещает просто написать туда произвольный текст. Но вы сами прекрасно понимаете, что это совсем не имеет смысла.

Если все успешно, то мы получаем JSON с полем *code: 10*. 

Поднимаем веб-сервер.

На ваш ранее указанный URL будут приходить события в POST формате. Вот пример одного из событий: 
```json
{
  "type": "new_message",
  "event": {
    "message_id": 12,
    "to_id": 1,
    "from_id": 2,
    "created_at": 1581110027,
    "text": "this is a new message!"
  }
}
```

При смене URL Callback сервера приходит событие *'callback_deprecated'*, в котором указаны старый и новый серверы.

Думаю, все понятно. Можно поэкспериментировать.

## Работа с Long Polling для получения событий

Тут всё еще проще. Делаем запрос на **/events/polling** с полями в body: 

| Поле  | Тип    | Описание           |
|-------|--------|--------------------|
| token | String | Токен пользователя |

Ждем ответ сервера. Как только произойдет какое-либо событие, сервер сразу вернет вам ответ. Пример такого события есть выше. События приходят одинаковыми как для Callback, так и для Long Polling.

Почитайте об этих технологиях отдельно.

## Коды ошибок

Небольшая табличка поможет вам при разработке.

| Код | Описание                                  |
|-----|-------------------------------------------|
| -1  | Invalid token                             |
| -2  | Message not sent                          |
| -3  | Parameters are invalid                    |
| -4  | Name cannot be more than 15 characters    |
| -5  | Password cannot be less than 8 characters |
| -6  | User does not exist                       |
