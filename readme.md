# Injection Fuzzing Basics

Набор аккуратно отфильтрованных словарей для фаззинга актуальных **инъекций** на 24 год:

- SQL / NoSQL
- XSS (reflected / stored / DOM / polyglot)
- командные инъекции
- SSTI / code injection
- десериализация
- XXE / XML
- инъекции заголовков (Host / Origin / CRLF)

Фокус:

- **детект и базовая разведка**, а не “боевые” эксплойты;
- **одна строка = один payload**;
- минимум дубликатов;
- словари удобно использовать в **ZAP**, Burp и других фаззерах.

При разроботке данных словарей использовались следующие источники:

- [SecLists](https://github.com/danielmiessler/SecLists/tree/master)
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master)
- [fuzzdb](https://github.com/fuzzdb-project/fuzzdb/tree/master)
- заводские словари ZAP’а

---

## Структура

### `SQL/`

- отдельные файлы под MySQL, MSSQL, Oracle, PostgreSQL;
- `sqli_general`, `sqli_passive`, `sqli_active` — общие словари для детекта;
- WAF-bypass payload’ы, unicode/whitespace-обходы, polyglot-пейлоады;
- `paths_unix` / `paths_win` — полезные пути для чтения/утечек через SQLi;
- словари для распознавания типа СУБД по ошибкам/поведению.

### `NoSQL/`

- `nosql_mongo_where.txt` — инъекции через `"$where"` и встроенный JS;
- `nosql_mongo_operators.txt` — `$ne`, `$gt`, `$lt`, `$in`, `$nin`, `$exists` и т.п.;
- `nosql_mongo_auth_bypass_url.txt` / `nosql_mongo_auth_bypass_json.txt` — обход логина в URL/JSON;
- `nosql_mongo_regex_length.txt` / `nosql_mongo_regex_data.txt` — извлечение длины и данных через `$regex`;
- `nosql_mongo_misc_js.txt` — разные JS-пэйлоады для Mongo-контекста.

### `XSS/`

- `Polyglot/` — полиглотные XSS-пейлоады.
- `XSS_DOM/` — DOM-based XSS (JS/JSON контекст: `XSS-JSB`, `XSS-JSN`).
- `XSS_special/` — большие/агрессивные списки (fuzzing, no-parens).
- `XSS_stored-reflected/` — контекстные словари:
  - `HTML_ATTR`, `CSS_STYLE`, `XSS-URI`, `XSS-XML`, `XSS_Markdown`.

### `Command_Injection/`

- Unix: базовый `id`/`uname`/`cat`, расширенный набор, no-space обходы, OOB (DNS/HTTP).
- Windows: базовые `dir` и обходы через `%ENV:~start,len%`.
- `shell-delimiters` — разделители/операторы для разрыва команд.

### `Code_Injection/`

- `Server_code_execution.txt` — безопасные “test RCE” (print/console.log/echo 7*7) для разных языков.
- `SSTI/` — полиглотный SSTI и детект движков (Jinja/Twig/Freemarker/ERB/Razor и др.).

### `Deserialization/`

- PHP: простые `serialize()`-структуры для детекта `unserialize()`.
- Java: Base64-похожие Java-serialized “магические” значения (`rO0AB...`).
- .NET: ViewState-паттерны (`/wEPDw...`) для обнаружения десериализации в ASP.NET.

### `Header_injection/`

- `Host--Origin--Forwarded-инъекции/`:
  - внутренние / внешние / «продвинутые» значения `Host`,
  - `Origin` для проверки CORS и доверия к источнику,
  - заголовки семейства `X-Forwarded-*` для проверки поведения прокси и балансировщиков.
- `HTTP Response Splitting -- CRLF Injection/`:
  - добавление собственных заголовков через `\r\n`,
  - разделение ответа и инъекция тела ответа.
- `SMTP Mail Header Injection/`:
  - типичные имена полей, попадающих в SMTP-заголовки (например, `To`, `Cc`, `Bcc`, `Reply-To`, `Subject` и др.),
  - payload’ы с `CRLF` для проверки возможности добавления скрытых получателей, подмены заголовков и других эффектов при небезопасной обработке пользовательского ввода  
    (предназначены для использования на тестовых стендах и в рамках разрешённого тестирования безопасности).


### `XML Injection/`

- `XML_XXE.txt` — компактный набор XXE:
  - классическое XXE (file/http),
  - php://filter / data:// / external DTD,
  - SOAP / CDATA, XInclude,
  - XXE→SSRF, blind/OOB XXE.
  - DoS/Billion Laughs умышленно не включены (это уже эксплуатация, не детект).

---

## Как этим пользоваться

- Подключать файлы как **payload lists** в ZAP/Burp/другие инструменты.
- Выбирать словарь по **типу инъекции и контексту**:
  - SQL/NoSQL, XSS (атрибут/URI/DOM), заголовки, JSON, шаблоны, десериализация и т.п.
- Для более тяжёлых сценариев:
  - использовать эти словари как “детектор”,
  - а затем идти к оригинальным большим коллекциям с тысачами запросов (SecLists, PayloadsAllTheThings).

Репозиторий задуман как **компактная база для фаззинга инъекций**: немного, но по делу, без превращения в ещё одну огромную кучу payload’ов.

