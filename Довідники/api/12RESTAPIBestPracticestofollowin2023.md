# 12 REST API Best Practices to follow in 2023

https://josipmisko.com/posts/rest-api-best-practices

By **Josip Miskovic**•Jan 30, 2023

Як розробник, ви розумієте важливість надійного, безпечного та легкого у використанні API. Дотримуючись найкращих практик REST API, ви можете переконатися, що ваш API відповідає цим стандартам.

Я розробляю API більше 10 років, і ось мій список **найважливіших передових практик REST API**:

## 1. Дотримуйтеся правил іменування URI

Ідентифікатори URI в REST API повинні відповідати певним умовам іменування для **узгодженості та ясності**.

| Convention                                 | Description                                                  | Example                                                      |
| ------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Use Lowercase letters                      | URI мають починатися з літери та використовувати лише малі літери. | `/users`, `/orders`                                          |
| Use Plural nouns                           | Іменники у множині слід використовувати в URI для ідентифікації колекцій ресурсів. | `/users`, `/orders`                                          |
| Hyphen separate resource names             | Літерали/вирази в шляхах URI мають бути розділені дефісом (`-`) для зручності читання. | `/user-info`, `/order-history`                               |
| Underscore separate query strings          | Літерали/вирази в рядках запиту мають бути розділені підкресленням (`_`). | `/users?sort_by=name`,  `/orders?filter_by_status=completed` |
| Use sub-resource collections for relations | Колекції підресурсів повинні існувати безпосередньо під окремим ресурсом, щоб передати зв’язок з іншою колекцією ресурсів. | `/users/{user_id}/orders`,  `/orders/{order_id}/items`       |
| Use top-level resources (when possible)    | Ми повинні прагнути до обмеженого розміщення ресурсів.       | `/items/{item_id}`                                           |

## 2. Використовуйте методи HTTP для передачі намірів

Одним із ключових принципів REST API є використання **стандартних методів HTTP** для передачі мети запиту.

| HTTP Method | Description             |
| ----------- | ----------------------- |
| GET         | Отримати ресурс         |
| POST        | Створити новий ресурс   |
| PUT         | Оновити існуючий ресурс |
| PATCH       | Частково оновити ресурс |
| DELETE      | Видалити ресурс         |

**Read more**: [PATCH VS PUT](https://josipmisko.com/posts/patch-vs-put-rest-api)

------

**Не використовуйте дієслова в URI.**

Ось приклад того, що робити, а що не робити:

| Incorrect            | Correct         |
| -------------------- | --------------- |
| GET /getAllUsers     | GET /users      |
| POST /createUser     | POST /users     |
| PUT /updateUser/1    | PUT /users/1    |
| DELETE /deleteUser/1 | DELETE /users/1 |

## 3. Використовуйте відповідну структуру даних

Це поширена помилка, що REST API має використовувати структуру JSON. Але REST — це тільки *ресурс*. Таким ресурсом може бути зображення JPEG, документ HTML або будь-яка структура даних.

Дізнайтеся, що вам підходить. Багато інструкцій щодо API компанії змушують використовувати JSON.

### Валідна схема JSON

Якщо ви використовуєте JSON, дотримуйтесь цих практичних порад:

1. Валідна [схема JSON](https://json-schema.org) має використовуватися як для запиту, так і для тіла відповіді.
2. Додайте заголовок «Content-Type» зі значенням «application/json» у всі запити та відповіді під час надсилання даних JSON.
3. Використовуйте JSON навіть під час передачі повідомлень про помилки. Не просто повертайте простий текст або HTML.

JSON (JavaScript Object Notation) — це полегшений формат обміну даними, представлений у грудні 1999 року як частина стандарту ECMA-262 3rd Edition. Він розроблений таким чином, щоб людям було легко читати й писати.

## 4. Виберіть угоду про іменування полів JSON (і дотримуйтесь її)

Стандарт JSON не нав’язує правила іменування полів, але найкраще **вибрати одне й дотримуватися його**.

| Convention | Description                                                  | Example     |
| ---------- | ------------------------------------------------------------ | ----------- |
| snake_case | Малі літери зі знаком підкреслення, що розділяють слова      | "user_name" |
| camelCase  | Мала перша літера першого слова, велика перша літера наступних слів | "userName"  |
| PascalCase | Написання першої літери кожного слова з великої літери       | "UserName"  |
| kebab-case | Малі літери з дефісом, що розділяє слова                     | "user-name" |

camelCase і snake_case є найпоширенішими. Я віддаю перевагу camelCase і не рекомендую PascalCase або kebab-case.

## 5. Спілкуйтеся за допомогою кодів статусу HTTP

Коди стану є стандартною частиною протоколу HTTP, і вони дозволяють швидко передавати інформацію про результат запиту.

Використовуючи коди статусу HTTP у своїх відповідях API, ви допомагаєте своїм клієнтам налагодити обробку помилок.

Інструменти моніторингу, такі як Application Insights і App Dynamics, використовують коди статусу HTTP, щоб зрозуміти поточний стан і справність API. Наприклад, якщо ви використовуєте код стану в діапазоні 200, навіть якщо операція не вдається, інструмент моніторингу не знатиме, що щось пішло не так.

Ось таблиця зі списком найважливіших кодів стану HTTP:

| HTTP Status Code          | Description                                                  |
| ------------------------- | ------------------------------------------------------------ |
| 200 OK                    | The request was successful and the response body contains the requested data. |
| 201 Created               | The request was successful and a new resource was created.   |
| 204 No Content            | The request was successful but there is no representation to return (i.e. the response is empty). |
| 400 Bad Request           | The request was invalid or cannot be served. The request should not be repeated without modification. |
| 401 Unauthorized          | The request requires authentication and the user has not provided valid credentials. |
| 403 Forbidden             | The server understands the request, but it refuses to authorize it. |
| 404 Not Found             | The requested resource could not be found but may be available in the future. |
| 500 Internal Server Error | An internal server error occurred while processing the request. |

**Read more**: [HTTP Bad Request vs Not Found](https://josipmisko.com/posts/http-bad-request-vs-not-found)

## 6. Не підтримувати стан

REST API не повинен підтримувати стан на сервері. Це відповідальність клієнта.

Це важливо, оскільки дає змогу API кешувати, масштабувати та відокремлювати від клієнта.

Наприклад, API електронної комерції може використовувати файли cookie для підтримки стану кошика для покупок. Однак такий підхід порушує ключовий принцип RESTful API — вони повинні бути **без стану**.

## 7. Версії ваших API

Контроль версій API важливий, оскільки він забезпечує зворотну сумісність, дає змогу запроваджувати нові функції без поломки існуючих клієнтів і дозволяє поетапно розгортати зміни.

Ось найпопулярніші способи версії REST API:

| Versioning Method     | Description                                                  | Format                                           |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------ |
| URL path              | Номер версії включено в URL-шлях, наприклад "api/v1/resources" | api/v1/resources                                 |
| URL query parameter   | Номер версії входить до URL-адреси як параметр, наприклад "api?version=1" | api?version=1                                    |
| Custom request header | Спеціальний заголовок, наприклад "X-API-Version", використовується для вказівки номера версії | X-API-Version: 1                                 |
| Content negotiation   | Клієнт вказує потрібну версію через використання заголовка «Accept» у запиті | Accept: application/vnd.company.resource-v1+json |

Метод керування версіями **URL-шляху** є найпопулярнішим, оскільки легко побачити, яку версію API ви використовуєте, просто подивившись на URL-адресу.

Майте на увазі, що різні клієнти можуть використовувати різні версії вашого API, тому обслуговування збільшується.

### Коли я маю представити нову версію кінцевої точки API?

Нова версія кінцевої точки API повинна бути представлена, коли є зміни, які не є зворотно сумісними з попередніми версіями.

Це може включати зміни формату запиту чи відповіді, зміни ресурсу, до якого здійснюється доступ, або зміни очікуваної поведінки кінцевої точки. Крім того, може бути представлена нова версія, якщо знадобиться капітальна зміна або переробка кінцевої точки.

Важливо встановити версії API, щоб існуючі клієнти могли продовжувати працювати з попередніми версіями, а нові клієнти могли скористатися новими функціями.

## 8. Ретельно задокументуйте свій API

Ретельне документування API передбачає кілька ключових кроків:

1. Створіть стислу документацію API: додайте всі кінцеві точки з їхніми тілами запиту та відповіді.
2. Надайте приклади коду: додайте приклади на кількох мовах програмування, щоб продемонструвати, як можна використовувати API.
3. Використовуйте чіткі та зрозумілі повідомлення про помилки: чітко повідомляйте, які помилки можуть виникнути та як їх усунути.
4. Пропонуйте інтерактивну документацію: використовуйте такі інструменти, як Swagger UI або Postman, щоб надати розробникам інтерактивну документацію.
5. Безпека документів: поясніть, як можна захистити API і який тип автентифікації потрібен.
6. Поясніть обмеження швидкості: чітко повідомте будь-які обмеження швидкості та те, як вони застосовуються.
7. Пропонуйте детальні примітки до випуску: тримайте розробників у курсі будь-яких змін або оновлень API.
8. Надайте ресурси підтримки: запропонуйте такі ресурси, як форум розробників або центр підтримки, щоб допомогти розробникам із будь-якими запитаннями.
9. Регулярно оновлюйте документацію: регулярно оновлюйте документацію, щоб переконатися, що вона залишається точною та актуальною.

Розгляньте можливість надання інтерактивної документації, наприклад Swagger або Postman, щоб допомогти розробникам зрозуміти, як використовувати ваш API.

## 9. Використовуйте послідовні повідомлення про помилки

У більшості випадків кодів стану HTTP недостатньо, щоб пояснити, що пішло не так.

Щоб допомогти користувачам API, додайте структуроване повідомлення про помилку JSON.

Відповідь має містити таку інформацію:

1. Код помилки: машиночитаний код помилки, який ідентифікує конкретну причину помилки.
2. Повідомлення про помилку: зрозуміле для людини повідомлення, яке містить детальне пояснення помилки.
3. Контекст помилки: додаткова інформація, пов’язана з помилкою, наприклад ідентифікатор запиту, параметри запиту, які спричинили помилку, або поля в запиті, які спричинили помилку.
4. Посилання на помилки: URL-адреси ресурсів або документації, які надають додаткову інформацію про помилку та способи її вирішення.
5. Відмітка часу: час, коли сталася помилка.

Визначте, що вам підходить, і дотримуйтеся цього. Кожне повідомлення про помилку у вашому API має мати однаковий формат.

Наприклад: json

```json
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
    "errorCode": "INVALID_INPUT",
    "message": "The input provided is invalid.",
    "details": [
        {
            "field": "firstName",
            "issue": "required field is missing"
        },
        {
            "field": "lastName",
            "issue": "required field is missing"
        }
    ],
    "request_id": "5124129901",
    "timestamp": "2022-12-10T13:46:20.857Z"
}
```

## 10. Надайте гіпермедійні посилання

Найкраща практика HATEOAS для REST API стверджує, що API не повинен визначати фіксовані імена ресурсів або ієрархії, оскільки це створює зв’язок між клієнтом і сервером.

Натомість сервер повинен надати клієнту інструкції щодо створення відповідних URI за допомогою типів носіїв і зв’язків посилань, дозволяючи серверу контролювати власний простір імен. Це гарантує, що структура ресурсу не визначається позасмуговою інформацією, а взаємодія керується гіпертекстом.

Рой Філдінг, розробник принципів REST, [стверджує](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven), що REST API **повинні керуватися гіпертекстом **.

Приклад: json

```json
{
  "articles": [
    {
      "id": 1,
      "title": "How to Build a RESTful API",
      "author": "John Doe",
      "date": "2022-01-01",
      "links": [
        {
          "rel": "self",
          "href": "https://api.example.com/articles/1"
        },
        {
          "rel": "author",
          "href": "https://api.example.com/authors/1"
        }
      ]
    },
    {
      "id": 2,
      "title": "Building Scalable APIs",
      "author": "Jane Doe",
      "date": "2022-02-01",
      "links": [
        {
          "rel": "self",
          "href": "https://api.example.com/articles/2"
        },
        {
          "rel": "author",
          "href": "https://api.example.com/authors/5121"
        }
      ]
    }
  ]
}
```

## 11. Розгляньте можливість застосування обмеження швидкості для викликів API

Обмеження швидкості — це метод обмеження кількості запитів API, які клієнт може зробити за певний період часу.

Відповідь API зазвичай містить інформацію про ліміт швидкості та поточне використання клієнтом, щоб клієнт міг відповідно скоригувати свою поведінку.

Код статусу HTTP "429 Too Many Requests" і заголовок "Retry-After" зазвичай використовуються для обмеження частоти викликів API.

- Код статусу помилки **429 Too Many Requests** вказує на те, що користувач надіслав занадто багато запитів за певний проміжок часу, і API тимчасово заблокував подальші запити від цього користувача.
- Заголовок **Retry-After** означує проміжок часу, який користувач має чекати, перш ніж робити додаткові запити.
- Заголовок **Retry-Remaining** означує кількість викликів, що залишилися, протягом певного періоду часу.

Робоча група HTTP [запропонувала проект](https://www.ietf.org/archive/id/draft-polli-ratelimit-headers-02.html) для RateLimit-Limit, RateLimit-Remaining і RateLimit-Reset поля заголовка для передачі квот запитів у HTTP. Ці заголовки дозволяють серверам повідомляти поточні квоти запитів, а клієнтам – формувати свою політику запитів, щоб уникнути придушення.

## 12. Використовуйте параметри запиту

Параметри запиту дозволяють надавати додаткову інформацію в URL-адресі HTTP-запиту, щоб контролювати відповідь, яку повертає сервер.

Використовуйте параметри запиту, щоб:

| Action   | Description                                                 | Example                                                 |
| -------- | ----------------------------------------------------------- | ------------------------------------------------------- |
| Filter   | Повертати лише відповідні результати для конкретного запиту | `/users?name={name}`                                    |
| Sort     | Упорядкуйте результати певним чином                         | `/users?sort_by=age`                                    |
| Paginate | Розділіть результати на керовані частини або сторінки       | `/users?page={page_number}&per_page={results_per_page}` |

## Який ваш рівень зрілості API?

[Модель зрілості Річардсона](https://martinfowler.com/articles/richardsonMaturityModel.html) — це шкала, яка вимірює зрілість дизайну RESTful API.

Він переходить від рівня 0 (найгірший) до рівня 4 (найкращий).

Ось більше про рівні:

- **Рівень 0: The Swamp of POX (Plain Old XML)** - цей рівень представляє базове використання HTTP як транспортного протоколу для обміну корисними навантаженнями XML. Немає поняття REST або його обмежень.
- **Рівень 1: Ресурси** - API розроблено навколо ресурсів, кожен ресурс ідентифікується за URI.
- **Рівень 2: дієслова HTTP** – на цьому рівні наголошується на використанні методів HTTP для виконання операцій над ресурсами відповідно до обмеження REST щодо використання єдиного інтерфейсу. API також може почати включати гіпермедійні посилання для представлення зв’язків між ресурсами.
- **Рівень 3: Елементи керування гіпермедіа** – на цьому рівні API використовує елементи керування гіпермедіа, щоб управляти станом програми та дозволяти клієнту взаємодіяти з API у більш відокремлений спосіб. Клієнт більше не залежить від попереднього знання структури API, натомість використовує гіпермедійні посилання для навігації API.
- **Рівень 4: мережа, керована гіпермедіа** - це найвищий рівень зрілості та представляє API, який повністю охоплює обмеження REST гіпермедіа як механізму стану програми. Клієнт взаємодіє з API виключно за допомогою елементів керування гіпермедіа, що робить API більш гнучким і стійким до змін.

Ці рівні є дуже корисним інструментом для оцінки того, наскільки добре ви застосували найкращі практики.

### Чи має ваш API бути рівня 4, щоб вважатися REST?

Ні, досягти рівня 4 дуже важко.

Зрештою, «принципи» та «найкращі практики» не висічені в камені.

Використовуйте найкращий розсуд.

Є так багато реальних сценаріїв, робіть те, що найкраще для вашого проекту.

**Опубліковано:** 30 січня 2023 р

https://josipmisko.com/posts/rest-api-best-practices

By **Josip Miskovic**•Jan 30, 2023