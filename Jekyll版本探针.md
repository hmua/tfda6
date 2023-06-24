---
permalink: 探针
output: false

---
# Jekyll版本探针 测试
*根据[Liquid Filters](https://jekyllrb.com/docs/liquid/filters/)制作*
*根据[GitHub Pages Dependency versions](https://pages.github.com/versions/)，GitHub Pages默认使用Jekyll 3.9.3*

{%assign a='abc,de,fg,hijk,abc,de,fgh'|split:','%}
{{a|size}}7✓
{{a|join:','}}✓
{{a.size}}7✓
{{a.join:','}}×
{{a.first.size}}3✓

### Map
{{a|map:'upcase'}}

---
{{site.pages|map:'path'|join:'、'}}，预期*assets/css/style.scss,README.md*✓

### Where
{%assign pages=site.pages|where:'path',page.path%}
{{pages.size}}，预期*1*✓
{{pages.first.path}}，预期*{{page.path}}*✓

### Where Expression (≥3.2.0)
{{a|where_exp:'b','b=="abc"'}}，预期*abcabc*✓

### Binary operators in `where_exp` filter (≥4.0)
{{ a | where_exp:"b", "b=='de' or b=='fg'" }}

`gh-pages` classic：*build error!*；新版：*defgde*

### Detecting `nil` values with `where` filter (≥4.0)
{%assign a=site.pages|where:'path',nil%}
{{a.size}}

`gh-pages` classic：*（非0）*；新版：*0*

### Find (≥4.1.0)
{%assign a=site.pages|find:'path',page.path%}
>{{a.path}}

`gh-pages` classic：*无输出*，*`|find`部分忽略了*；
新版：*{{page.path}}*×

### 行内注释 (Liquid ≥5.4)
{{'{'}}% # ... %}
*新版也不支持，build error!*
