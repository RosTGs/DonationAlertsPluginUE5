![GitHub](https://img.shields.io/github/license/ufna/DonationAlerts)

# DonationAlerts UE5.4

DonationAlerts integration plugin for Unreal Engine 5.

Minimum supported version: UE5.4. The plugin is intended for UE5.4 projects and should be compatible with newer UE5 releases unless noted otherwise.

This plugin allows stream viewers to make changes to the gameplay on the go: switch weapons, alter character behavior or environmental factors. It’s up to the developer to choose the elements that can be made interactive. DonationAlerts will be providing developers with all the necessary information: donation value, message text, sender name, currency, date and time of donation. With this data at hand, the game could choose one of the scenarios to run.

Documentation: https://www.donationalerts.com/apidoc#plugins_and_libraries

## Инструкция по использованию (RU)

1. Установите плагин:
   - Скопируйте папку `DonationAlerts` в каталог `Plugins` вашего проекта Unreal Engine.
   - Убедитесь, что в файле `DonationAlerts.uplugin` указана версия, совместимая с вашей версией UE (минимум 5.4).
2. Включите плагин в проекте:
   - Откройте Unreal Editor → **Edit** → **Plugins**.
   - Найдите **DonationAlerts** в списке и включите его.
   - Перезапустите редактор, если потребуется.
3. Подготовьте ключи доступа DonationAlerts:
   - Войдите в кабинет DonationAlerts и получите необходимые ключи/токены для интеграции.
   - Сохраните их в безопасном месте.
4. Настройте плагин в проекте:
   - Откройте **Project Settings** и найдите раздел, связанный с DonationAlerts.
   - Укажите полученные ключи/токены и сохраните настройки.
5. Подключите обработку событий донатов:
   - В вашей логике игры (Blueprint или C++) подпишитесь на события донатов.
   - Используйте полученные данные (сумма, сообщение, имя отправителя, валюта, дата/время), чтобы запускать нужные игровые сценарии.
6. Проверьте работу:
   - Запустите проект и отправьте тестовый донат из кабинета DonationAlerts.
   - Убедитесь, что событие корректно обрабатывается в игре.


## Plugins & Libraries
Позволяет легко авторизовать игрока и отправлять custom alerts. Плагин доступен на GitHub:

Пожалуйста, зарегистрируйте ваше OAuth приложение и настройте плагин:
https://www.donationalerts.com/application/clients

* How to authenicate user:
  <img width="1257" height="686" alt="ue4_set_up_plugin" src="https://github.com/user-attachments/assets/232a344a-8213-4aff-95cf-ad3363beabee" />
  <img width="1578" height="746" alt="ue4_user_auth" src="https://github.com/user-attachments/assets/5a77cbe2-3e71-402c-994f-2687479df1cb" />
* How to send custom alert:

<img width="1163" height="481" alt="ue4_send_custom_alert" src="https://github.com/user-attachments/assets/22c79b0e-9feb-416e-b860-d129c28ae184" />
* How to react to donation events:

<img width="867" height="554" alt="ue4_receive_donation" src="https://github.com/user-attachments/assets/3655e15e-c75b-4e59-8ec4-4a528c8c5878" />

* How to react to donation goal events:
<img width="814" height="551" alt="ue4_receive_donation_goal_update" src="https://github.com/user-attachments/assets/9a3b365f-08ca-49b2-88c1-20c41c81dfc4" />

* How to react to poll events:
<img width="1491" height="561" alt="ue4_receive_poll_update" src="https://github.com/user-attachments/assets/68df4e04-7b60-474c-97d6-c8313cb8749a" />

---




## DonationAlerts Public API (RU)

Публичный API DonationAlerts организован по принципам REST. Наш API использует предсказуемые URL’ы с ориентацией на ресурсы, возвращает ответы в JSON, и применяет стандартные HTTP коды ответов, аутентификацию и HTTP-методы.

Публичная точка входа DonationAlerts API для всех HTTP-методов:
`https://www.donationalerts.com/api/v1`
если не указано иначе.

Все ответы нашего API предоставляются в формате JSON, и в некоторых случаях может возвращаться код `204 No Content` с пустым телом ответа.

### Пример запроса

```bash
curl \
    -X GET https://www.donationalerts.com/api/v1/alerts/donations \
    -H "Authorization: Bearer <token>"
```

### Пример ответа

```http
HTTP/1.1 200 OK
Server: nginx/1.13.5
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive
Date: Sat, 1 Sep 2019 18:42:33 GMT
Content-Language: en_US
{
    "data": [
        {
            "id": 30530030,
            "name": "donation",
            "username": "Ivan",
            "message": "Hello!",
            "amount": 500,
            "currency": "RUB",
            "is_shown": 1,
            "created_at": "2019-09-29 09:00:00",
            "shown_at": null
        }
    ],
    "links": {
        "first": "https://www.donationalerts.com/api/v1/alerts/donations?page=1",
        "last": "https://www.donationalerts.com/api/v1/alerts/donations?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "https://www.donationalerts.com/api/v1/alerts/donations",
        "per_page": 30,
        "to": 1,
        "total": 1
    }
}
```

### Пагинация

По причинам производительности DonationAlerts public API использует пагинацию в выводе ответа. Это сделано потому, что возвращать весь набор данных может быть допустимо для одних запросов, но слишком затратно для других, которые возвращают очень большой объём данных.

Методы API, которые поддерживают пагинацию, будут возвращать части `meta` и `links` в теле ответа.

#### meta

| Параметр     | Описание                         |
| ------------ | -------------------------------- |
| current_page | Индекс текущей страницы          |
| from         | Индекс первого элемента          |
| last_page    | Общее количество страниц         |
| path         | Текущий путь                     |
| per_page     | Количество элементов на странице |
| to           | Индекс последнего элемента       |
| total        | Общее количество элементов       |

#### links

| Параметр | Описание                                                    |
| -------- | ----------------------------------------------------------- |
| first    | Ссылка на первую страницу                                   |
| last     | Ссылка на последнюю страницу                                |
| prev     | Ссылка на предыдущую страницу или null, если предыдущей нет |
| next     | Ссылка на следующую страницу или null, если следующей нет   |

Чтобы указать страницу, добавьте параметр `page` в query строку.

Пример запроса страницы 2:

```bash
curl \
    -X GET https://www.donationalerts.com/api/v1/alerts/donations?page=2 \
    -H "Authorization: Bearer <token>"
```

### Коды ответов

DonationAlerts использует стандартные HTTP коды ответов, чтобы обозначить успех или ошибку API-запроса. В целом:

* коды `2xx` означают успех;
* коды `4xx` означают ошибку на стороне клиента (например, не передан обязательный параметр, платёж не прошёл и т.п.);
* коды `5xx` означают ошибку на серверах DonationAlerts.

| Статус                    | Описание                                                    |
| ------------------------- | ----------------------------------------------------------- |
| 200 OK                    | Стандартный ответ для успешных HTTP-запросов                |
| 201 Created               | Запрос выполнен, в результате создан новый ресурс           |
| 202 Accepted              | Запрос принят в обработку, но обработка не завершена        |
| 204 No Content            | Сервер успешно обработал запрос и не возвращает контент     |
| 400 Bad Request           | Сервер не может обработать запрос из-за ошибки клиента      |
| 401 Unauthorized          | Требуется авторизация, и она не прошла или не предоставлена |
| 404 Not Found             | Ресурс не найден, но может появиться в будущем              |
| 500 Internal Server Error | Общая ошибка сервера при непредвиденной ситуации            |

#### Пример ошибки

```http
HTTP/1.1 401 Unauthorized
Server: nginx/1.13.5
Content-Type: application/json
{
    "message": "Unauthenticated."
}
```

### Лимиты запросов (Rate limits)

Все API-запросы ограничены лимитами. Мы ограничиваем запросы к нашим HTTP API методам для каждого приложения до **60 запросов в минуту**, то есть **1 запрос в секунду**.

### Локализация (i18n)

Некоторые API поддерживают интернационализацию и требуют указания локали. Все поддерживаемые локали перечислены ниже:

* `be_BY` — Belarusian;
* `de_DE` — German;
* `en_US` — English (USA);
* `es_ES` — Spanish;
* `es_US` — Spanish (USA);
* `et_EE` — Estonian;
* `fr_FR` — French;
* `he_HE` — Hebrew;
* `it_IT` — Italian;
* `ka_GE` — Georgian;
* `kk_KZ` — Kazakh;
* `ko_KR` — Korean;
* `lv_LV` — Latvian;
* `pl_PL` — Polish;
* `pt_BR` — Portuguese (Brazil);
* `ru_RU` — Russian;
* `sv_SE` — Swedish;
* `tr_TR` — Turkish;
* `uk_UA` — Ukrainian;
* `zh_CN` — Chinese.

### Валюты

Некоторые API требуют ввод валюты или возвращают валюту в ответе.

#### Поддерживаемые валюты ввода

* `EUR` — Euro;
* `USD` — US Dollar;
* `RUB` — Russian Ruble;
* `BRL` — Brazilian Real;
* `TRY` — Turkish Lira.

#### Поддерживаемые валюты вывода

* `EUR` — Euro;
* `USD` — US Dollar;
* `RUB` — Russian Ruble;
* `BYN` — Belarusian Ruble;
* `KZT` — Tenge;
* `UAH` — Hryvnia;
* `BRL` — Brazilian Real;
* `TRY` — Turkish Lira.

### Подпись запроса (Request signature)

Некоторые API требуют подпись запроса.

Подпись запроса — это строка, захешированная SHA256, сформированная из **значений параметров запроса**, отсортированных **по алфавиту по имени параметра** (каждое значение интерпретируется как строка), и с добавлением **API client secret key** в конец.

Например, если параметры запроса содержат:

```
foo=xyz&bar=abc
```

Тогда подпись должна быть сгенерирована так:

```
SHA256( abc + xyz + <API client secret> )
```

## Centrifugo (Real-time уведомления)

Мы используем Centrifugo для доставки real-time уведомлений, когда происходит определённое событие. Он работает как отдельный сервер и отвечает за постоянные подключения пользователей приложения.

WebSocket endpoint подключения Centrifugo:

`wss://centrifugo.donationalerts.com/connection/websocket`

Подписки на приватные каналы Centrifugo должны быть корректно подписаны API-приложением. Для подробной информации прочитайте специальную главу документации Centrifugo про подписки на приватные каналы.

### Подключение к Centrifugo WebSocket и получение UUIDv4 Client ID

Чтобы подключиться к Centrifugo WebSocket Server, сначала нужно открыть соединение с WebSocket endpoint. Когда соединение открыто, нужно отправить сообщение на WebSocket сервер. Это сообщение должно содержать ID сообщения и токен соединения пользователя Centrifugo, полученный ранее запросом `/user/oauth`. Сообщение должно быть в JSON и выглядеть так:

```json
{
    "params": {
        "token": "<socket_connection_token>"
    },
    "id": 1
}
```

В ответ вы получите сообщение с сгенерированным Client ID Centrifugo в форме UUIDv4 и версией Centrifugo. Пример ответа:

```json
{
    "id": 1,
    "result": {
        "client": "d558c046-c679-43e3-a62d-65989ab55f7c",
        "version": "2.2.1"
    }
}
```

### Подписка на приватные каналы и получение connection tokens

После получения UUIDv4 Client ID Centrifugo вы можете подписываться на нужные каналы. Подписка выполняется таким запросом:

`https://www.donationalerts.com/api/v1/centrifuge/subscribe`

#### Query String

| Параметр | Описание                                                    |
| -------- | ----------------------------------------------------------- |
| channels | Массив имён приватных каналов, на которые нужно подписаться |
| client   | UUIDv4 client ID Centrifugo, полученный при подключении     |

Ответ содержит массив подписанных каналов и токены, которые можно использовать для подключения к соответствующим приватным каналам.

### Подключение к приватным каналам

Чтобы подключиться к подписанным каналам, нужно отправить ещё одно сообщение через ранее открытое WebSocket соединение. Каждое сообщение должно содержать имя канала и токен подключения и выглядеть так:

```json
{
    "params": {
        "channel": "$alerts:donation_<user_id>",
        "token": "<connection_token>"
    },
    "method": 1,
    "id": 2
}
```

После этого вы получите подтверждение успешного подключения к каналу:

```json
{
   "result": {
        "type": 1,
        "channel": "$alerts:donation_3",
        "data": {
            "info": {
                "user": "1",
                "client": "d558c046-c679-43e3-a62d-65989ab55f7c"
            }
        }
    }
}
```

Теперь вы готовы получать real-time уведомления о событиях.

## Авторизация (OAuth 2.0)

Мы используем протокол авторизации OAuth 2.0, чтобы выдавать доступ к данным пользователя. Если вы не знакомы с концепцией OAuth 2.0, сначала прочитайте RFC 6749. Все методы DonationAlerts public API требуют авторизации.

Access token — это то, что приложения используют, чтобы делать API-запросы от имени пользователя. Access token представляет собой авторизацию конкретного приложения на доступ к определённым частям данных пользователя. Access token должен храниться конфиденциально при передаче и при хранении. Единственные стороны, которые должны видеть access token: само приложение, сервер авторизации и сервер ресурсов. Приложение должно обеспечить, чтобы хранилище access token было недоступно другим приложениям на том же устройстве. Access token можно использовать только по https соединению, так как передача по незашифрованному каналу позволяет третьим лицам легко перехватить токен.

Scope — это механизм OAuth 2.0 для ограничения доступа приложения к аккаунту пользователя. Приложение может запросить один или несколько scope, эта информация будет показана пользователю на экране согласия, и выданный приложению access token будет ограничен одобренными scope.

Спецификация OAuth позволяет серверу авторизации или пользователю изменять набор scope, выданных приложению, относительно запрошенных. Однако на практике многие сервисы так не делают. OAuth не определяет конкретные значения scope, так как они зависят от внутренней архитектуры сервиса и его потребностей.

DonationAlerts предоставляет 3rd-party сервисам следующие scope:

| Scope                    | Описание                              |
| ------------------------ | ------------------------------------- |
| oauth-user-show          | Получить данные профиля               |
| oauth-donation-subscribe | Подписаться на новые donation alerts  |
| oauth-donation-index     | Просматривать донаты                  |
| oauth-custom_alert-store | Создавать кастомные алерты            |
| oauth-goal-subscribe     | Подписаться на обновления донат-целей |
| oauth-poll-subscribe     | Подписаться на обновления опросов     |

### Authorization Code Grant

Тип grant `authorization code` используется потому, что он оптимизирован для server-side приложений, где исходный код не является публичным, и можно сохранять конфиденциальность client secret. Это redirection-based flow, то есть приложение должно уметь взаимодействовать с user-agent и получать коды авторизации, которые передаются через user-agent.

#### Шаги

* Регистрация приложения
* Запрос авторизации
* Получение access token
* Обмен authorization code на access token

Разработчик должен зарегистрировать приложение здесь, чтобы получить токены. После регистрации сервис выдаёт “client credentials” — client identifier и client secret.
`client_id` — публичная строка, которая используется API для идентификации приложения и построения URL авторизации.
`client_secret` — используется для подтверждения личности приложения при запросе доступа к аккаунту пользователя и должен храниться в секрете.

Пользователь получает ссылку авторизации (или редирект) на:

`https://www.donationalerts.com/oauth/authorize`

#### Query String

| Параметр           | Описание                                                       |
| ------------------ | -------------------------------------------------------------- |
| client_id          | Client ID, полученный от DonationAlerts                        |
| redirect_uri       | URL, куда пользователь будет отправлен после авторизации       |
| response_type=code | Указывает, что приложение запрашивает authorization code grant |
| scope              | Список scope, разделённый пробелами                            |

Когда пользователь нажимает ссылку, он сначала логинится (если ещё не вошёл). Потом сервис покажет экран, где пользователь разрешает или запрещает доступ приложения к аккаунту.

Если пользователь разрешает доступ, сервис перенаправляет user-agent на redirect URI приложения (указанный при регистрации) вместе с authorization code. Код будет в параметре `code`.

Authorization code нужно обменять на access token по адресу:

`https://www.donationalerts.com/oauth/token`

#### Query String

| Параметр                      | Описание                         |
| ----------------------------- | -------------------------------- |
| grant_type=authorization_code | Тип grant                        |
| client_id                     | ID приложения DonationAlerts     |
| client_secret                 | Секрет приложения DonationAlerts |
| redirect_uri                  | URL редиректа после авторизации  |
| code                          | Authorization code               |

### Refresh Token Grant

Тип grant `refresh_token` используется, чтобы обменять refresh token на access token, когда access token истёк.

Сервис сгенерирует и вернёт новый access token, если данные валидны. Сервер также может выдать новый refresh token, но если ответ не содержит новый refresh token, старый refresh token останется действительным.

`https://www.donationalerts.com/oauth/token`

#### Query String

| Параметр                 | Описание                                    |
| ------------------------ | ------------------------------------------- |
| grant_type=refresh_token | Тип grant                                   |
| refresh_token            | Refresh token, полученный от DonationAlerts |
| client_id                | ID приложения DonationAlerts                |
| client_secret            | Секрет приложения DonationAlerts            |
| scope                    | Список scope, разделённый пробелами         |

### Implicit Grant

Implicit grant похож на authorization code grant. Но токен возвращается клиенту без обмена authorization code. Этот grant чаще всего используется для JavaScript или mobile приложений, где client credentials нельзя безопасно хранить.

#### Шаги

* Регистрация приложения
* Запрос авторизации
* Получение access token

Пользователь получает запрос авторизации (или редирект) на:

`https://www.donationalerts.com/oauth/authorize`

#### Query String

| Параметр            | Описание                                                 |
| ------------------- | -------------------------------------------------------- |
| client_id           | Client ID, полученный от DonationAlerts                  |
| redirect_uri        | URL, куда пользователь будет отправлен после авторизации |
| response_type=token | Указывает, что приложение запрашивает access token       |
| scope               | Список scope, разделённый пробелами                      |

Если пользователь разрешает доступ, сервис перенаправляет user-agent на redirect URI вместе с access token. Access token будет в параметре `access_token` в hash-части URL.

## API v1 методы

### Получить профиль пользователя

Получает информацию профиля пользователя. Требует авторизацию пользователя со scope `oauth-user-show`.

`https://www.donationalerts.com/api/v1/user/oauth`

```bash
curl \
    -X GET https://www.donationalerts.com/api/v1/user/oauth \
    -H "Authorization: Bearer <token>"
```

### Список донатов (donation alerts list)

Получает массив объектов списка донат-алертов пользователя. Требует авторизацию со scope `oauth-donation-index`.

`https://www.donationalerts.com/api/v1/alerts/donations`

```bash
curl \
    -X GET https://www.donationalerts.com/api/v1/alerts/donations \
    -H "Authorization: Bearer <token>"
```

## Custom Alerts

Custom alerts — это алерты с полностью кастомизируемым контентом, которые позволяют разработчику создавать уникально оформленные алерты и отправлять их в трансляцию стримера.

Требуется, чтобы у стримера была вариация виджета Alerts с типом **"Custom alerts"**, чтобы кастомные алерты отображались.

### Отправить кастомный алерт

Отправляет кастомный алерт авторизованному пользователю. Требует scope `oauth-custom_alert-store`.

`https://www.donationalerts.com/api/v1/custom_alert`

#### Query String

| Параметр    | Описание                                                                    |
| ----------- | --------------------------------------------------------------------------- |
| external_id | Уникальный ID алерта (до 32 символов), генерируется разработчиком           |
| header      | Строка заголовка (до 255 символов), будет отображаться как header           |
| message     | Строка сообщения (до 300 символов), будет внутри message box                |
| is_shown    | 0 или 1. Определяет, должен ли алерт быть показан. Значение по умолчанию: 0 |
| image_url   | URL изображения (до 255 символов), будет показано вместе с алертом          |
| sound_url   | URL звука (до 255 символов), будет проигран при показе алерта               |

## Advertisement → Merchandises

Merchandises API — это набор методов API, который позволяет мерчанту продавать свои товары через стримеров DonationAlerts по модели распределения доходов.
Используя предоставленные методы API, можно создавать и обновлять товары, а также уведомлять DonationAlerts, когда происходит новая продажа.

Обратите внимание, что доступ к этому API предоставляется по запросу. Для деталей по интеграции свяжитесь с нами: [business@donationalerts.com](mailto:business@donationalerts.com).

Словарь терминов:

* Merchant — сервис или онлайн-магазин, интегрированный с Merchandise API DonationAlerts;
* Merchandise — продукт, доступный для покупки в онлайн-магазине Merchant;
* Broadcasting Platform — сервис, который позволяет пользователям вести прямые трансляции медиаконтента (например, Twitch или YouTube);
* Streamer — пользователь, который ведёт прямую трансляцию на Broadcasting Platform и зарегистрирован в DonationAlerts как стример;
* Customer — пользователь, который намерен приобрести Merchandise у Merchant. Чаще всего Customer — зритель трансляции Streamer.

Merchandises API требует взаимодействия **server-to-server** для обмена данными между DonationAlerts и Merchant.

Чтобы начать продажи товаров, Merchant должен быть интегрирован в DonationAlerts. Процесс интеграции может отличаться, так как в основном зависит от возможностей и предпочтений Merchant. Например, можно создавать, обновлять или «обновлять или создавать» товары через API, либо мы можем полностью вести это с нашей стороны.
Но во всех случаях требуется интеграция с **Send Sale Alerts API** как часть минимальной интеграции с Merchandises API, чтобы DonationAlerts мог получать уведомления о новых продажах.

Как только товары интегрированы, можно начинать продажи. Изображение ниже иллюстрирует весь процесс — от начала процедуры покупки до отображения нового алерта о продаже товара на трансляции Streamer:

1. Customer посещает трансляцию Streamer на Broadcasting Platform в своём веб-браузере или приложении.
2. Далее Customer переходит на страницу товара (product webpage) Merchandise, размещённую в онлайн-магазине Merchant. Это может быть сделано напрямую из Broadcasting Platform, если Streamer разместил там URL товара (2.a), либо через донат-страницу Streamer на DonationAlerts (2.b, 2.c), так как все товары Streamer перечислены там. Опционально URL товара может содержать user ID DonationAlerts или промокод Streamer в query-параметрах, чтобы Merchant мог легко определить, чья реферальная ссылка была использована.
3. В онлайн-магазине Merchant Customer проходит обычный процесс оформления заказа и перенаправляется к Payment Processor для завершения покупки.
4. После покупки Customer перенаправляется обратно в онлайн-магазин Merchant, а Payment Processor уведомляет Merchant о новой покупке.
5. Merchant уведомляет DonationAlerts о новой покупке с помощью Send Sale Alerts API.
6. После уведомления о новой продаже DonationAlerts добавляет выручку на баланс Streamer в DonationAlerts и генерирует новый merchandise sale alert, который отправляется в виджет алертов Streamer.
7. Виджет алертов Streamer получает merchandise sale alert и отображает его, а программа для трансляции Streamer захватывает содержимое виджета алертов и передаёт его на Broadcasting Platform вместе с другим медиаконтентом.

### Создать merchandise

Создаёт новый товар (merchandise). Этот API является частью Merchandise Advertisement API.

`https://www.donationalerts.com/api/v1/merchandise`

#### Query String

| Параметр               | Описание                                                                                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| merchant_identifier    | ID мерчанта на DonationAlerts                                                                                                                                           |
| merchandise_identifier | Уникальный ID товара (до 16 символов), генерируется мерчантом                                                                                                           |
| title                  | Массив строк названия товара (до 1024 символов) для разных локалей. Минимально требуется title для локали en_US                                                         |
| is_active              | 0 или 1. Определяет, доступен ли товар для покупки. Значение по умолчанию: 0                                                                                            |
| is_percentage          | 0 или 1. Определяет, являются ли price_service и price_user суммами в валюте currency или вычисляются как процент от общей суммы продажи. Значение по умолчанию: 0      |
| currency               | Одна из доступных валют товара. Все расчёты выполняются согласно этому значению                                                                                         |
| price_user             | Сумма выручки, добавляемая стримеру за каждую продажу                                                                                                                   |
| price_service          | Сумма выручки, добавляемая DonationAlerts за каждую продажу                                                                                                             |
| url                    | URL страницы товара (до 128 символов). Можно включать шаблоны {user_id} и {user_merchandise_promocode}, которые будут заменены в UI на user ID и promocode пользователя |
| img_url                | URL изображения товара (до 128 символов)                                                                                                                                |
| end_at_ts              | Дата и время, когда товар становится неактивным, в Unix timestamp                                                                                                       |
| signature              | Подпись запроса                                                                                                                                                         |

### Обновить merchandise

Обновляет товар. Этот API является частью Merchandise Advertisement API.

`https://www.donationalerts.com/api/v1/merchandise/{:id}`

#### Query String

| Параметр               | Описание                                                                                                                                                                |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| merchant_identifier    | ID мерчанта на DonationAlerts                                                                                                                                           |
| merchandise_identifier | Уникальный ID товара (до 16 символов), генерируется мерчантом                                                                                                           |
| title                  | Массив строк названия товара (до 1024 символов) для разных локалей. Минимально требуется title для локали en_US                                                         |
| is_active              | 0 или 1. Определяет, доступен ли товар для покупки. Значение по умолчанию: 0                                                                                            |
| is_percentage          | 0 или 1. Определяет, являются ли price_service и price_user суммами в валюте currency или вычисляются как процент от общей суммы продажи. Значение по умолчанию: 0      |
| currency               | Одна из доступных валют товара. Все расчёты выполняются согласно этому значению                                                                                         |
| price_user             | Сумма выручки, добавляемая стримеру за каждую продажу                                                                                                                   |
| price_service          | Сумма выручки, добавляемая DonationAlerts за каждую продажу                                                                                                             |
| url                    | URL страницы товара (до 128 символов). Можно включать шаблоны {user_id} и {user_merchandise_promocode}, которые будут заменены в UI на user ID и promocode пользователя |
| img_url                | URL изображения товара (до 128 символов)                                                                                                                                |
| end_at_ts              | Дата и время, когда товар становится неактивным, в Unix timestamp                                                                                                       |
| signature              | Подпись запроса                                                                                                                                                         |

### Update or Create (обновить или создать)

Комбинированный метод, который позволяет обновить товар или создать, если он ещё не существует. При желании можно использовать вместо отдельных create и update методов, так как он позволяет обновлять товар без необходимости хранить merchandise ID DonationAlerts. Этот API является частью Merchandise Advertisement API.

`https://www.donationalerts.com/api/v1/merchandise/{:merchant_identifier}/{:merchandise_identifier}`

#### Query String

| Параметр      | Описание                                                                                                                                                                |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| title         | Массив строк названия товара (до 1024 символов) для разных локалей. Минимально требуется title для локали en_US                                                         |
| is_active     | 0 или 1. Определяет, доступен ли товар для покупки. Значение по умолчанию: 0                                                                                            |
| is_percentage | 0 или 1. Определяет, являются ли price_service и price_user суммами в валюте currency или вычисляются как процент от общей суммы продажи. Значение по умолчанию: 0      |
| currency      | Одна из доступных валют товара. Все расчёты выполняются согласно этому значению                                                                                         |
| price_user    | Сумма выручки, добавляемая стримеру за каждую продажу                                                                                                                   |
| price_service | Сумма выручки, добавляемая DonationAlerts за каждую продажу                                                                                                             |
| url           | URL страницы товара (до 128 символов). Можно включать шаблоны {user_id} и {user_merchandise_promocode}, которые будут заменены в UI на user ID и promocode пользователя |
| img_url       | URL изображения товара (до 128 символов)                                                                                                                                |
| end_at_ts     | Дата и время, когда товар становится неактивным, в Unix timestamp                                                                                                       |
| signature     | Подпись запроса                                                                                                                                                         |

### Получить user ID по рекламному промокоду

Получает user ID по рекламному промокоду. Этот API является частью Merchandise Advertisement API. Может использоваться, чтобы получить user ID и затем передать его в Send Sale Alerts API.

`https://www.donationalerts.com/api/v1/merchandise/user`

#### Query String

| Параметр  | Описание              |
| --------- | --------------------- |
| promocode | Промокод пользователя |
| signature | Подпись запроса       |

### Создать merchandise sale alert

Создаёт новый алерт о продаже товара. Этот API является частью Merchandise Advertisement API.

`https://www.donationalerts.com/api/v1/merchandise_sale`

#### Query String

| Параметр               | Описание                                                           |
| ---------------------- | ------------------------------------------------------------------ |
| user_id                | User ID DonationAlerts, к которому относится продажа               |
| external_id            | Уникальный ID продажи (до 32 символов), генерируется разработчиком |
| merchant_identifier    | ID мерчанта на DonationAlerts                                      |
| merchandise_identifier | ID товара мерчанта, который купил покупатель                       |
| amount                 | Итоговая сумма продажи                                             |
| currency               | Одна из доступных валют продажи, указывает валюту amount           |
| bought_amount          | Общее количество купленных товаров. Значение по умолчанию: 1       |
| username               | Имя покупателя                                                     |
| message                | Сообщение, отправленное покупателем при покупке товара             |
| signature              | Подпись запроса                                                    |

## Centrifugo Channels (реалтайм-события)

DonationAlerts API предлагает различные Centrifugo каналы для получения real-time уведомлений.

Подробнее про Centrifugo можно прочитать в соответствующем разделе документации Centrifugo.

Каждое сообщение, отправленное в канал, содержит атрибут `reason` в дополнение к исходному ресурсу. Этот атрибут описывает событие, которое произошло при отправке сообщения.

### Donation alerts channel

Подписка на этот канал позволяет получать новые donation alerts пользователя. Требует scope `oauth-donation-subscribe`.

Имя канала Centrifugo — `$alerts:donation_<user_id>`

Сообщения, отправляемые в этот канал, содержат ресурс donation в том же виде, как описано в Donations Alerts List.

### Goals channel

Подписка на этот канал позволяет получать обновления по донат-целям. Требует scope `oauth-goal-subscribe`.

Имя канала Centrifugo — `$goals:goal_<user_id>`

Сообщения содержат ресурс donation goal, представленный как описано ниже.

#### Атрибуты Donation Goal

| Атрибут       | Описание                                                           |
| ------------- | ------------------------------------------------------------------ |
| id            | Уникальный идентификатор цели                                      |
| is_active     | Флаг, показывающий активна ли цель                                 |
| title         | Заголовок цели                                                     |
| currency      | Код валюты цели (ISO 4217)                                         |
| start_amount  | Стартовая сумма цели                                               |
| raised_amount | Текущая собранная сумма, включая start_amount                      |
| goal_amount   | Целевая сумма                                                      |
| started_at    | Дата и время старта цели (формат YYYY-MM-DD HH.MM.SS)              |
| expires_at    | Дата и время завершения цели (формат YYYY-MM-DD HH.MM.SS) или null |

### Polls channel

Подписка на этот канал позволяет получать обновления по опросам. Требует scope `oauth-poll-subscribe`.

Имя канала Centrifugo — `$polls:poll_<user_id>`

Сообщения содержат ресурс poll, представленный как описано ниже.

#### Атрибуты Poll

| Атрибут            | Описание                                                                                                   |
| ------------------ | ---------------------------------------------------------------------------------------------------------- |
| id                 | Уникальный идентификатор опроса                                                                            |
| is_active          | Флаг, показывающий активен ли опрос                                                                        |
| title              | Заголовок опроса                                                                                           |
| allow_user_options | Флаг, разрешает ли опрос донатерам добавлять свои варианты                                                 |
| type               | Тип опроса, определяет как считается победитель: `count` — по количеству донатов; `sum` — по сумме донатов |
| options            | Массив вариантов опроса (Poll Option resource)                                                             |

#### Атрибуты Poll Option

| Атрибут        | Описание                                                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| id             | Уникальный идентификатор варианта                                                                                              |
| title          | Заголовок варианта                                                                                                             |
| amount_value   | Абсолютное значение варианта (число или сумма, зависит от типа)                                                                |
| amount_percent | Процент варианта относительно других                                                                                           |
| is_winner      | Флаг победителя. Опрос может иметь несколько победителей, если максимальный amount_value разделён между несколькими вариантами |



