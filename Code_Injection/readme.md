# Code / Template Injection

Этот раздел — про инъекции, которые приводят к выполнению кода на стороне сервера:

- прямой **server-side code execution**,
- **SSTI** (Server-Side Template Injection).

Во всём репозитории сохраняется общий принцип:

> **одна строка = один payload**, минимум дубликатов, фокус на детекте и базовой разведке.  
> За более сложными, “боевыми” примерами можно идти в разделы *Code Injection* и *Server Side Template Injection* репозитория PayloadsAllTheThings.

---

## Файлы

### `Server_code_execution.txt`

Мини-словарь для проверки **выполнения кода** в разных языках:

- Python, Ruby, PHP, Java, C#, JavaScript и др.;
- выражения вроде `print(7*7)`, `console.log(7*7);`, `<?php echo 7*7; ?>`.

Payload’ы намеренно “безобидные” (арифметика / вывод строк), но позволяют увидеть:

- исполняется ли вообще код из входных данных,
- и в каком языке/движке это происходит.

---

## Папка `SSTI/`

Словари для **инъекций в шаблонизаторах** (Server-Side Template Injection).

- `SSTI_polyglot.txt` — общий полиглот для детекта SSTI, когда неизвестен движок и контекст.
- `SSTI_engines_detection.txt` — небольшой набор выражений (`{{7*7}}`, `${3*3}`, `<%= 7*7 %>` и т.п.) для фингерпринта шаблонизатора (Jinja2, Twig, Freemarker, ERB, Razor и др.).
- `readme.md` — отдельное описание SSTI-части.

---

## Дальнейшее углубление

Этот раздел даёт **минимальный набор для детекта и первичной разведки**.  
Для:

- чтения файлов,
- обхода sandbox,
- построения надёжных RCE-цепочек

см. соответствующие разделы (*Code Injection*, *Server Side Template Injection*, *Insecure Deserialization* и т.п.) в репозитории PayloadsAllTheThings.

