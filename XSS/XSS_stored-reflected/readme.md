1) HTML_ATTR.txt
Все payload’ы для:

HTML-body / tag-breaking (<script>...</script>, </title>...)

HTML-атрибутов и евентов (onload=, onerror=, src=javascript:..., <img ...>, <iframe ...>, <embed ...> и т.п.)

«polyglot/evasion» варианты, если они не CSS-специфичные

После чистки тут останутся и базовые, и энкодированные HTML/атрибутные варианты.

2) CSS_STYLE.txt
Только то, что целится в CSS-контекст:

style= атрибуты с expression(...), -moz-binding, url(javascript:...)

<style>...</style> / @import / css rules с JS-URI, если они именно в CSS

Важное правило: если строка валидна как HTML-атрибут, а CSS там «случайно» — лучше в HTML_ATTR.
Если строка валидна только в CSS — в CSS_STYLE.

3) XSS-URI.txt
Payload’ы для URL/protocol sinks:

javascript: / data: / vbscript: в href/src/action/location

engine/protocol-handler экзотика (firefoxurl:, res://, navigatorurl: и т.п.)

Это отдельный контекст, правильно что отдельно.

4) XSS-XML.txt
Только XML/XHTML/MSXML/data-island-форматы:

<![CDATA[ ... ]]>

<xml id=... datasrc=... datafld=...> и подобные

Редкий, но шумный контекст — держать отдельно очень полезно.
