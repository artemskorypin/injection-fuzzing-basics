## Файлы

- **nosql_mongo_where.txt**  
  Пэйлоады для инъекций через `"$where"` и встроенный JS: always-true, проверки `username == 'admin'`, `isAdmin == true`, `password.match(/.*/)` и т.п. Нужен, когда вход может попасть в `$where` или JS-выражение.

- **nosql_mongo_operators.txt**  
  Базовые Mongo-операторы (`$ne`, `$gt`, `$lt`, `$in`, `$nin`, `$exists`, `$regex`) в JSON- и urlencoded/PHP-формате. Используется для фуззинга “операторных” инъекций в полях запросов.

- **nosql_mongo_auth_bypass_url.txt**  
  Пэйлоады для обхода аутентификации в запросах вида `application/x-www-form-urlencoded`, где `username[$ne]=...` превращается в `{ username: { $ne: ... } }`. Применяется к формам логина.

- **nosql_mongo_auth_bypass_json.txt**  
  То же, но для `application/json`: конструкции вида `{"username":{"$ne":null}, ...}`. Используется против REST/JSON API, где тело запроса напрямую попадает в Mongo-запрос.

- **nosql_mongo_regex_length.txt**  
  Небольшой набор `$regex`-масок вида `.{1}`, `.{2}`, ... для оценки длины значений (например, пароля) по реакции приложения. Полезно, когда NoSQL-инъекция уже подтверждена.

- **nosql_mongo_regex_data.txt**  
  Пэйлоады для извлечения содержимого через `$regex` (посимвольное/поэтапное угадывание, маски `^m`, `^md`, `^mdp` и т.п., в JSON и urlencoded). Для демонстрации и исследования уже найденной инъекции.

- **nosql_mongo_misc_js.txt**  
  Разные JS-пэйлоады для случаев, когда данные попадают внутрь JS-логики (`$where`-строки и т.п.): `&& this.password.match(...)`, `|| this.isAdmin==true`, `|| 1==1` и т.д. Для фуззинга поведения именно JS-части, а не только операторов.

