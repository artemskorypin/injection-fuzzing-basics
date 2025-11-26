# Инъекции заголовков / Host / Origin / CRLF

Этот раздел покрывает:

- инъекции в заголовки **Host / Origin / X-Forwarded-\***;
- атаки через **HTTP Response Splitting / CRLF Injection**.

Как и в остальном репозитории:

> **одна строка = один payload**, минимум дубликатов, фокус на детекте и базовой разведке.  
> За более продвинутыми техниками (request smuggling, сложный cache poisoning и т.п.) лучше обращаться к PayloadsAllTheThings.

---

## Host / Origin / Forwarded-инъекции

Папка: `Host--Origin--Forwarded-инъекции/`

### `header_host_internal.txt`

Значения для заголовка `Host` / `X-Forwarded-Host` / `X-Forwarded-For` и т.п. в стиле **“мы как будто изнутри”**:

- `localhost`, `127.0.0.1`, `::1`;
- приватные подсети (`10.x.x.x`, `192.168.x.x`, `172.16.x.x`);
- адреса метадаты облака (`169.254.169.254`, `metadata.google.internal`).

Используется для проверки:

- SSRF на внутренние сервисы;
- доверия к “локальным” хостам в логике приложения/прокси.

---

### `header_host_external.txt`

Значения для **Host header poisoning** и open redirect’ов:

- простые злонамеренные домены (`evil.example`, `attacker.example.com`);
- поддомены/формы вида `victim.example.com.evil.com`, `victim.example.com@attacker.example.com`;
- варианты с путями, портами, `https://...` и `//host`.

Цель — проверить, использует ли приложение/прокси `Host` для:

- генерации ссылок (парольный ресет, активации и т.п.);
- логики авторизации / multi-tenant / маршрутизации.

---

### `header_host_advanced.txt`

Более “хитрые” значения `Host`:

- комбинированные домены, которые могут по-разному парситься;
- варианты, полезные для кеш-поизонинга и странной обработки URL.

Используется, когда:

- базовые внутренние/внешние значения уже протестированы,
- нужно пофаззить поведение кэшей, балансировщиков и хитрых URL-обработчиков.

---

### `header_origin_cors.txt`

Значения для заголовка `Origin` (и частично `Referer`) в CORS/Origin-контексте:

- внешние Origin’ы (`https://evil.example`, `https://attacker.example.com`);
- `null` и `chrome-extension://...`;
- локальные Origin’ы (`http://localhost`, `http://127.0.0.1`, `http://intranet.local`).

Используется для проверки:

- CORS-мисконфигов (слишком доверчивый `Access-Control-Allow-Origin`);
- логики, завязанной на Origin/Referer.

---

## HTTP Response Splitting / CRLF Injection

Папка: `HTTP Response Splitting -- CRLF Injection/`

### `header_crlf_basic.txt`

Payload’ы, которые пытаются **добавить свои заголовки** через `CRLF`:

- `\r\n` / `%0d%0a` + `X-Injected-Header`, `Set-Cookie`, `Location`, `Refresh`;
- простые варианты log poisoning (вставка “лишних” строк в логи).

Сценарии использования:

- инъекция в любые заголовки/поля, которые могут попасть в HTTP-ответ;
- проверка, можно ли “разорвать” строку заголовка и начать новую.

---

### `header_crlf_body.txt`

Payload’ы, которые пытаются **обрубить заголовки и начать тело ответа**:

- добавление `\r\n\r\n` с вставкой простого текста (`INJECTED`, `INJECTED-BODY`);
- варианты с собственным `Content-Type` / `Content-Length` и HTML-телом.

Используется для проверки:

- HTTP Response Splitting;
- возможности подменить/подinject’ить часть тела ответа.

---

## Использование

- Значения из `header_host_*` и `header_origin_cors.txt` подставляются в:
  - `Host`, `Origin`, `Referer`,
  - `X-Forwarded-Host`, `X-Forwarded-For`, `X-Forwarded-Proto` и аналогичные заголовки.
- Значения из `header_crlf_*` подставляются в любые заголовки/поля, где возможен `CRLF` (в том числе в параметры, которые затем попадают в заголовок).

Файлы удобно использовать как payload-листы в ZAP/Burp/других фаззерах для систематической проверки поведения приложения и промежуточных прокси.

