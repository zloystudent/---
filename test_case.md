### **Test Suite 1: Управление Пользователями (Users)**

#### **TC_USER_001: Успешное создание пользователя**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_USER_001 |
| **Приоритет** | High |
| **Предусловие** | • Пользователь с логином и email из таблицы данных отсутствует в БД. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Установить тело запроса: `{{request_body}}`<br>5. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `201 Created`.<br>• Тело ответа содержит JSON-объект нового пользователя, включая его `id`. |
| **Верификация (DB)** | • `SELECT * FROM wp_users WHERE user_login = 'testuser_api';` -> 1 строка.<br>•|

**Тестовые данные для TC_USER_001**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `POST` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/users/` |
| `auth_header` | Authorization: Basic [base64-строка от 'admin:password'] |
| `request_body` | `{ "username": "testuser_api", "email": "testuser_api@example.com", "password": "StrongPassword123" }` |

---
#### **TC_USER_002: Успешное обновление данных пользователя**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_USER_002 |
| **Приоритет** | High |
| **Предусловие** | • В БД существует пользователь с ID `[ID_пользователя]`. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Установить тело запроса: `{{request_body}}`<br>5. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `200 OK`.<br>• Тело ответа содержит обновленные данные пользователя. |
| **Верификация (DB)** | • `SELECT display_name FROM wp_users WHERE id = [ID_пользователя];` -> возвращает `Test User Updated Name`. |

**Тестовые данные для TC_USER_002**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `POST` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/users/[ID_пользователя]` |
| `auth_header` |Authorization: Basic [base64-строка от 'admin:password'] |
| `request_body` | `{ "name": "Test User Updated Name" }` |

---
#### **TC_USER_003: Успешное удаление пользователя**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_USER_003 |
| **Приоритет** | High |
| **Предусловие** | • В БД существует пользователь с ID `[ID_пользователя]`.<br>• В БД существует пользователь с ID `1` для переназначения контента. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}?{{query_params}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `200 OK`.<br>• Тело ответа содержит `deleted: true`. |
| **Верификация (DB)** | • `SELECT * FROM wp_users WHERE id = [ID_пользователя];` -> 0 строк. |

**Тестовые данные для TC_USER_003**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `DELETE` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/users/[ID_пользователя]` |
| `query_params` | `force=true&reassign=1` |
| `auth_header` | Authorization: Basic [base64-строка от 'admin:password'] |



---
### **Test Suite 2 — Управление Записями (Posts)**

#### **TC_POST_001: Успешное создание записи**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_POST_001 |
| **Приоритет** | High |
| **Предусловие** | • Запись с заголовком из таблицы данных (`My API Test Post`) отсутствует в БД. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Установить тело запроса: `{{request_body}}`<br>5. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `201 Created`.<br>• Тело ответа содержит JSON-объект новой записи, включая ее `id`. |
| **Верификация (DB)** | • Выполнить `SELECT post_title, post_status FROM wp_posts WHERE id = [ID_созданной_записи];`.<br>• Ожидаемый результат: Запрос возвращает 1 строку со значениями `My API Test Post` и `draft`. |

**Тестовые данные для TC_POST_001**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `POST` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/posts/` |
| `auth_header` | Authorization: Basic `[base64-строка от 'admin:password']` |
| `request_body` | `{ "title": "My API Test Post", "content": "This is content.", "status": "draft" }` |

---
#### **TC_POST_002: Успешное обновление записи**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_POST_002 |
| **Приоритет** | High |
| **Предусловие** | • В БД существует запись с ID `[ID_записи]` и статусом `draft`. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Установить тело запроса: `{{request_body}}`<br>5. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `200 OK`.<br>• Тело ответа содержит JSON-объект обновленной записи со статусом `publish`. |
| **Верификация (DB)** | • Выполнить `SELECT post_status FROM wp_posts WHERE id = [ID_записи];`.<br>• Ожидаемый результат: Запрос возвращает 1 строку со значением `publish`. |

**Тестовые данные для TC_POST_002**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `POST` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/posts/[ID_записи]` |
| `auth_header` | Authorization: Basic `[base64-строка от 'admin:password']` |
| `request_body` | `{ "status": "publish" }` |

---
#### **TC_POST_003: Успешное удаление записи**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_POST_003 |
| **Приоритет** | High |
| **Предусловие** | • В БД существует запись с ID `[ID_записи]`. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}?{{query_params}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `200 OK`.<br>• Тело ответа содержит JSON-объект удаленной записи. |
| **Верификация (DB)** | • Выполнить `SELECT * FROM wp_posts WHERE id = [ID_записи];`.<br>• Ожидаемый результат: Запрос возвращает 0 строк. |

**Тестовые данные для TC_POST_003**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `DELETE` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/posts/[ID_записи]` |
| `query_params` | `force=true` |
| `auth_header` | Authorization: Basic `[base64-строка от 'admin:password']` |

---



### ** Test Suite 3 — Управление Комментариями (Comments)**

#### **TC_COMM_001: Успешное создание комментария**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_COMM_001 |
| **Приоритет** | High |
| **Предусловие** | • В БД существует опубликованная запись с ID `[ID_записи]`, у которой в таблице `wp_posts` поле `comment_status` имеет значение `open`. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Установить тело запроса: `{{request_body}}`<br>5. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `201 Created`.<br>• Тело ответа содержит JSON-объект нового комментария, включая его `id`. |
| **Верификация (DB)** | • Выполнить `SELECT comment_content, comment_post_ID FROM wp_comments WHERE comment_ID = [ID_созданного_комментария];`.<br>• Ожидаемый результат: Запрос возвращает 1 строку со значениями `This is a test comment.` и `[ID_записи]`. |

**Тестовые данные для TC_COMM_001**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `POST` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/comments/` |
| `auth_header` | Authorization: Basic `[base64-строка от 'admin:password']` |
| `request_body` | `{ "post": [ID_записи], "content": "This is a test comment." }` |

---
#### **TC_COMM_002: Успешное обновление комментария**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_COMM_002 |
| **Приоритет** | High |
| **Предусловие** | • В БД существует комментарий с ID `[ID_комментария]`. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Установить тело запроса: `{{request_body}}`<br>5. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `200 OK`.<br>• Тело ответа содержит JSON-объект обновленного комментария. |
| **Верификация (DB)** | • Выполнить `SELECT comment_content FROM wp_comments WHERE comment_ID = [ID_комментария];`.<br>• Ожидаемый результат: Запрос возвращает 1 строку со значением `Updated test comment.`. |

**Тестовые данные для TC_COMM_002**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `POST` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/comments/[ID_комментария]` |
| `auth_header` | Authorization: Basic `[base64-строка от 'admin:password']` |
| `request_body` | `{ "content": "Updated test comment." }` |

---
#### **TC_COMM_003: Успешное удаление комментария**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_COMM_003 |
| **Приоритет** | High |
| **Предусловие** | • В БД существует комментарий с ID `[ID_комментария]`. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}?{{query_params}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `200 OK`.<br>• Тело ответа содержит JSON-объект удаленного комментария. |
| **Верификация (DB)** | • Выполнить `SELECT * FROM wp_comments WHERE comment_ID = [ID_комментария];`.<br>• Ожидаемый результат: Запрос возвращает 0 строк. |

**Тестовые данные для TC_COMM_003**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `DELETE` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/comments/[ID_комментария]` |
| `query_params` | `force=true` |
| `auth_header` | Authorization: Basic `[base64-строка от 'admin:password']` |

---




### ** Test Suite 4 — Управление Рубриками (Categories)**

#### **TC_CAT_001: Успешное создание рубрики**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_CAT_001 |
| **Приоритет** | High |
| **Предусловие** | • Рубрика с именем `API Created Category` отсутствует в БД. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Установить тело запроса: `{{request_body}}`<br>5. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `201 Created`.<br>• Тело ответа содержит JSON-объект новой рубрики, включая ее `id`. |
| **Верификация (DB)** | • Выполнить `SELECT t.name, tt.taxonomy FROM wp_terms t JOIN wp_term_taxonomy tt ON t.term_id = tt.term_id WHERE t.name = 'API Created Category';`.<br>• Ожидаемый результат: Запрос возвращает 1 строку со значениями `API Created Category` и `category`. |

**Тестовые данные для TC_CAT_001**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `POST` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/categories/` |
| `auth_header` | Authorization: Basic `[base64-строка от 'admin:password']` |
| `request_body` | `{ "name": "API Created Category" }` |

---
#### **TC_CAT_002: Успешное обновление рубрики**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_CAT_002 |
| **Приоритет** | High |
| **Предусловие** | • В БД существует рубрика с ID `[ID_рубрики]`. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Установить тело запроса: `{{request_body}}`<br>5. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `200 OK`.<br>• Тело ответа содержит JSON-объект обновленной рубрики. |
| **Верификация (DB)** | • Выполнить `SELECT name FROM wp_terms WHERE term_id = [ID_рубрики];`.<br>• Ожидаемый результат: Запрос возвращает 1 строку со значением `Updated Category Name`. |

**Тестовые данные для TC_CAT_002**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `POST` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/categories/[ID_рубрики]` |
| `auth_header` | Authorization: Basic `[base64-строка от 'admin:password']` |
| `request_body` | `{ "name": "Updated Category Name" }` |

---
#### **TC_CAT_003: Успешное удаление рубрики**

| Атрибут | Описание |
| :--- | :--- |
| **ID** | TC_CAT_003 |
| **Приоритет** | High |
| **Предусловие** | • В БД существует рубрика с ID `[ID_рубрики]`. |
| **Шаги (API Request)** | 1. Сформировать `{{http_method}}` запрос.<br>2. URL запроса: `{{base_url}}{{endpoint}}?{{query_params}}`<br>3. Установить заголовок авторизации: `{{auth_header}}`<br>4. Отправить запрос. |
| **Ожидаемый результат** | • Статус ответа: `200 OK`.<br>• Тело ответа содержит JSON-объект удаленной рубрики. |
| **Верификация (DB)** | • Выполнить `SELECT * FROM wp_terms WHERE term_id = [ID_рубрики];`. Ожидаемый результат: 0 строк.<br>• Выполнить `SELECT * FROM wp_term_taxonomy WHERE term_id = [ID_рубрики];`. Ожидаемый результат: 0 строк. |

**Тестовые данные для TC_CAT_003**

| Параметр | Значение |
| :--- | :--- |
| `http_method` | `DELETE` |
| `base_url` | `http://localhost:8000/index.php?rest_route=` |
| `endpoint` | `/wp/v2/categories/[ID_рубрики]` |
| `query_params` | `force=true` |
| `auth_header` | Authorization: Basic `[base64-строка от 'admin:password']` |

---
