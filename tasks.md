## High Priority
```tasks
group by function { \
    let taskGroup = (task.file.filenameWithoutExtension || "") + (task.heading ? "#" + task.heading : ""); \
    let totalUrgency = query.allTasks.filter(t => \
        (t.file.filenameWithoutExtension || "") + (t.heading ? "#" + t.heading : "") === taskGroup && !t.isDone) \
        .reduce((sum, t) => sum + (t.urgency || 0), 0); \
    let prioGroups = query.allTasks.filter(t => \
        (t.file.filenameWithoutExtension || "") + (t.heading ? "#" + t.heading : "") === taskGroup && t.happens.moment && !t.done.moment) \
        .map(t => (t.file.filenameWithoutExtension || "") + (t.heading ? "#" + t.heading : "")); \
    let sortedGroups = [...new Set(query.allTasks.filter(t => !t.isDone) \
        .map(t => (t.file.filenameWithoutExtension || "") + (t.heading ? "#" + t.heading : "")))] \
        .map(group => ({ \
            group: group, \
            urgency: query.allTasks.filter(t => \
                (t.file.filenameWithoutExtension || "") + (t.heading ? "#" + t.heading : "") === group && !t.isDone) \
                .reduce((sum, t) => sum + (t.urgency || 0), 0) \
        })) \
        .sort((a, b) => b.urgency - a.urgency); \
    let index = sortedGroups.findIndex(g => g.group === taskGroup); \
    return taskGroup ? '%%' + index + '%% [[' + taskGroup + ']] (Total Urgency: ' + totalUrgency.toFixed(2) + ')' : ''; \
} 
```


