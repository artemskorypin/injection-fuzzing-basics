# XSS wordlists

Этот раздел содержит словари нагрузок для разных типов XSS:

- отражённый (reflected)
- сохранённый (stored)
- DOM-based
- полиглотный XSS
- контекстно-зависимые случаи (атрибуты, стили, URI, XML, Markdown)

Везде действует общий принцип:

> **одна строка = один payload**, минимум дубликатов, фокус на идеях, а не на тоннах вариаций.

---

## Структура

### `Polyglot/`
Полиглотные XSS-пейлоады, которые пытаются сработать сразу в нескольких контекстах (HTML, атрибуты, JS, URI и т.д.).

- `XSS-POLYGLOTS.txt` — компактный основной набор.
- `XSS-Polyglot-Ultimate-0xsobky.txt` — большой агрессивный список полиглотов.

---

### `XSS_DOM/`
Payload’ы для **DOM-based XSS** — когда ввод попадает в JS/JSON, а не напрямую в HTML.

- `XSS-JSB.txt` — JavaScript-код / выражения (`<script>`, обработчики и т.п.).
- `XSS-JSN.txt` — JSON/объектные контексты, где данные могут быть выполнены через `eval`/`Function` и т.п.

---

### `XSS_special/`
«Спецнаборы» для широкого или нестандартного фаззинга.

- `XSS-Fuzzing.txt` — общий большой набор для массового фаззинга.
- `XSS-NO-PARENS.txt` — XSS без круглых скобок (обход фильтров `()`).
- `XSS-payloadbox.txt` — крупная коллекция payloadbox.
- `XSS-With-Context-Jhaddix.txt` — контекстный список (по типам контекста: тег, атрибут, JS и т.п.).

---

### `XSS_stored-reflected/`
Контекстно-ориентированные словари для классического stored/reflected XSS.

- `HTML_ATTR.txt` — XSS в HTML-атрибутах.
- `CSS_STYLE.txt` — XSS в стилях (`style`, `<style>`).
- `XSS-URI.txt` — XSS в URI/URL (`href`, `src`, `javascript:`, `data:` и т.п.).
- `XSS-XML.txt` — XSS в XML/XHTML/SVG-контексте.
- `XSS_Markdown.txt` — XSS в Markdown (ссылки, опасные схемы).

---

## Использование

- Файлы можно подключать как payload-листы в ZAP/Burp/другие фаззеры.
- При известном контексте выбирай соответствующую папку/файл (атрибут, стиль, JS, JSON, Markdown).
- При неизвестном — начинай с:
  - `XSS-Fuzzing.txt` или полиглотов (`XSS-POLYGLOTS.txt`),
  - а потом уточняй контекстом из `XSS_stored-reflected` и `XSS_DOM`.

