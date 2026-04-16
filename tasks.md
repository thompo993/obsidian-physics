## High Priority
```tasks
group by function { \
    let allTasks = query.allTasks.filter(t => !t.isDone); \
    let groupUrgencyMap = new Map(); \
    let groupTaskCountMap = new Map(); \
    allTasks.forEach(t => { \
        let taskGroup = (t.file.filenameWithoutExtension || "") + (t.heading ? "#" + t.heading : ""); \
        groupUrgencyMap.set(taskGroup, (groupUrgencyMap.get(taskGroup) || 0) + (t.urgency || 0)); \
        groupTaskCountMap.set(taskGroup, (groupTaskCountMap.get(taskGroup) || 0) + 1); \
    }); \
    let normalize = (group) => groupUrgencyMap.get(group) / (groupTaskCountMap.get(group) || 1); \
    let sortedGroups = [...groupUrgencyMap.keys()].sort((a, b) => normalize(b) - normalize(a)); \
    let taskGroup = (task.file.filenameWithoutExtension || "") + (task.heading ? "#" + task.heading : ""); \
    let index = sortedGroups.indexOf(taskGroup); \
    return taskGroup ? '%%' + index + '%% [[' + taskGroup + ']] (Normalized Urgency: ' + normalize(taskGroup).toFixed(2) + ')' : ''; \
} 
```


````
```tasks
```
````