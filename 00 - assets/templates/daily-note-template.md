<% tp.web.daily_quote() %>

# Check Tasks
[[Tasks]]

```
modification date: <%+ tp.file.last_modified_date("dddd Do MMMM YYYY HH:mm:ss") %> <%* let yesterday = tp.date.now("YYYY-MM-DD", -1, tp.file.title, "YYYY-MM-DD") let today = tp.date.now("YYYY-MM-DD", 0, tp.file.title, "YYYY-MM-DD") let tomorrow = tp.date.now("YYYY-MM-DD", 1, tp.file.title, "YYYY-MM-DD") %> [[<% tp.date.yesterday("YYYY-MM-DD") %>]] <== <button class="date_button_today">Today</button> ==> [[<% tp.date.tomorrow("YYYY-MM-DD") %>]] ## Tasks ### Over Due ```tasks not done due before <% today %> ``` ### Today #### Due Today ```tasks not done due on <% today %> ```
```


# Notes:







# Tags
#daily-note
