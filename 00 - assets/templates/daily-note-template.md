<% tp.web.daily_quote() %>

# Check Tasks
[[Tasks]]

modification date: <% tp.file.last_modified_date("dddd Do MMMM YYYY HH:mm:ss") %>

















# Date Conversion
[[<% tp.date.now("YYYY-MM-DD", -1) %>]] <== [[<% tp.date.now("YYYY-MM-DD") %>]] (Today) ==> [[<% tp.date.now("YYYY-MM-DD", 1) %>]]
