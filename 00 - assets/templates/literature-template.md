---
cssclass: literature-note
alias: [{% if shortTitle %}"{{shortTitle | safe}}"{% else %}"{{title | safe}}"{% endif %}]
---

> [!info]
> - **Cite Key:** [[@{{citekey}}]]
{%- for attachment in attachments | filterby("path", "endswith", ".pdf") %}
> - **Link:** [{{attachment.title}}](file://{{attachment.path | replace(" ", "%20")}})
{%- endfor -%}
{%- if abstractNote %}
> - **Abstract:** {{abstractNote}}
{%- endif -%}
{%- if bibliography %}
> - **Bibliography:** {{bibliography}}
{%- endif %}
{%- if hashTags %}
> - **Tags:** {{hashTags}}
{%- endif %}

# Tags:

# Article: