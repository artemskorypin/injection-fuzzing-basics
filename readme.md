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
- минимум дубликатов и мусора;
- словари удобно использовать в **ZAP**, Burp и других фаззерах.

При разроботке данных словарей использовались следующие источники:

- [SecLists](https://github.com/danielmiessler/SecLists/tree/master)
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master)
- [fuzzdb](https://github.com/fuzzdb-project/fuzzdb/tree/master)
- заводские словари ZAP’а

---

## Структура

### `SQLi/`

- `SQL/` — SQL-инъекции по СУБД (MySQL, MSSQL, Oracle, PostgreSQL) + общие словари:
  - базовые / blind / WAF-bypass payload’ы,
  - `sqli_general`, `sqli_passive`, `sqli_active`,
  - вспомогательные словари: распознавание СУБД, Unicode/whitespace, polyglot,
  - `paths_unix` / `paths_win` — полезные file-path’ы для чтения через SQLi.
- `noSQL/` — MongoDB-инъекции:
  - `$where`, операторы (`$ne`, `$in`, `$regex`),
  - auth bypass (JSON/urlencoded),
  - regex для длины/данных,
  - JS-пейлоады для DOM/JS-контекста.

### `XSS/`

- `Polyglot/` — полиглотные XSS-пейлоады (малый и большой набор).
- `XSS_DOM/` — DOM-based XSS (JS/JSON контекст: `XSS-JSB`, `XSS-JSN`).
- `XSS_special/` — большие/агрессивные списки (fuzzing, no-parens, payloadbox, Jhaddix).
- `XSS_stored-reflected/` — контекстные словари:
  - `HTML_ATTR`, `CSS_STYLE`, `XSS-URI`, `XSS-XML`, `XSS_Markdown`.

### `Command_Injection/`

- Unix: базовый `id`/`uname`/`cat`, расширенный набор, no-space обходы, OOB (DNS/HTTP).
- Windows: базовые `dir` и обходы через `%ENV:~start,len%`.
- `shell-delimiters` — разделители/операторы для разрыва команд.

### `Code_Injection/`

- `Code_Injection_JSON_basic.txt` — JSON-инъекции / proto-pollution / выход в JS-код.
- `Server_code_execution.txt` — безопасные “test RCE” (print/console.log/echo 7*7) для разных языков.
- `SSTI/` — полиглотный SSTI и детект движков (Jinja/Twig/Freemarker/ERB/Razor и др.).

### `Deserialization/`

- PHP: простые `serialize()`-структуры для детекта `unserialize()`.
- Java: Base64-похожие Java-serialized “магические” значения (`rO0AB...`).
- .NET: ViewState-паттерны (`/wEPDw...`) для обнаружения десериализации в ASP.NET.

### `Header_injection/`

- `Host--Origin--Forwarded-инъекции/`:
  - внутренние / внешние / продвинутые `Host`,
  - `Origin` для CORS / доверия к Origin.
- `HTTP Response Splitting -- CRLF Injection/`:
  - добавление заголовков через CRLF,
  - сплит и инъекция тела ответа.

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
  - а затем идти к оригинальным большим коллекциям с тысачами запросов (SecLists, PayloadsAllTheThings, fuzzdb, словари ZAP).

Репозиторий задуман как **чистая база для фаззинга инъекций**: немного, но по делу, без превращения в ещё одну огромную свалку payload’ов.

