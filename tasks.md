## High Priority
```tasks
group by function { \
    let cacheKey = 'filename-heading-normalised-urgency'; \
    let getTaskGroup = (t) => (t.file.filenameWithoutExtension || "") + (t.heading ? "#" + t.heading : ""); \
    if (!query.searchCache[cacheKey]) { \
        let allTasks = query.allTasks.filter(t => !t.isDone); \
        let groupUrgencyMap = new Map(); \
        let groupTaskCountMap = new Map(); \
        allTasks.forEach(t => { \
            let taskGroup = getTaskGroup(t); \
            groupUrgencyMap.set(taskGroup, (groupUrgencyMap.get(taskGroup) || 0) + (t.urgency || 0)); \
            groupTaskCountMap.set(taskGroup, (groupTaskCountMap.get(taskGroup) || 0) + 1); \
        }); \
        let normalize = (group) => groupUrgencyMap.get(group) / (groupTaskCountMap.get(group) || 1); \
        let sortedGroups = [...groupUrgencyMap.keys()].sort((a, b) => normalize(b) - normalize(a)); \
        query.searchCache[cacheKey] = { sortedGroups, normalize }; \
    } \
    let { sortedGroups, normalize } = query.searchCache[cacheKey]; \
    let taskGroup = getTaskGroup(task); \
    let index = sortedGroups.indexOf(taskGroup); \
    return taskGroup ? '%%' + index + '%% [[' + taskGroup + ']] (Normalized Urgency: ' + normalize(taskGroup).toFixed(2) + ')' : ''; \
}
```

## All Tasks
```tasks
```
