---
tags:
  - note
created: 2026-09-08
topic: learning-obsidian
module:
loc:
  - home
---
# Links: 
[[example note here]]

# Notes:
## Explaining what you see above 
If you are confused as to what you see above, it is a **properties** table, which is in YAML syntax and will show in all of your obsidian notes. 

 It allows you to give your notes different properties, which will allow for easy searching later on. The **Links** section above is a place for linking any notes you make that you think may be relevant, and this is done by using "\[\[example note here\]\]".  I don't worry much about links, and find them a bit gimmicky, but they are easy enough to do so i use them anyway. 

The note taking approach we want should be simple and lazy, with having topic, and module, and location, i am confident we can single out any note, for example, a lecture on semiconductors, taken in Tyndall, on P-N junctions can be found, and if i need to specify more i will just add the property (for example i could add a `type` property to separate worksheets and past papers and lecture notes, for example. 

---
## Folder Structure
My folder structure is fairly simple. To be clear, most people do not even use folders, and only tags and links, i think this is arrogant, as it means you cannot navigate your files without obsidian, and others who do not have obsidian cannot navigate it making proper folders means that anyone can find something in your notes.


My basic file structure is as follows:
![[Pasted image 20260909002515.png]]

The "00" and "01" is just so i can choose the order the folders appear, i do the same for all of my notes, I preface them is YYMMDD so they by default are sorted by creation date, for example, this note is called `260908 - Guide To Physics for Obsidian`. 

### 00 - assets 
This folder has a few things in them, and basically contains all the things needed to make the vault work. 
### attachments 
Where I store all images, screenshots, diagrams, PowerPoints, Course Notes, pdfs etc. It just keeps them in all in one place and not messy. I used to bother with labelling things but dont anymore, as it wasnt useful. if its plot from a code or a document a do, but screenshots i just copy and paste. 

You should setup in the "Files and links" section in the settings to set you attachment folder to this folder, so everything automatically goes here. (Lazy and simple). 

### drawings 
A plugin that we need to use later, Excalidraw, requires a folder. It will let us take handwritten notes and diagrams. If i want to put a drawing in a typed document, it goes here, the "annotations" and "cropped" are just because Excalidraw does it like this by default, so i wanted it to match. If i do a full note handwritten style, i put in a different folder, usually the topic folder.

### misc 
A folder for notes that i don't know where they go, basically just keeps the rest of the folder structure uncluttered, i have things like definitions in there "paramagnetic" for example

### templates
This  folder is used to hold templates, which are a key part of our workflow, they allow us to auto create the initial setup of a note so we don't forget things like creation date, topics etc. I will explain this more later. examples of some templates i used are above.

## 01 - daily-notes
This is where all my daily notes go, daily notes is a core plugin (made by devs) and is where my note for each day goes, this can be random stuff, like a to-do list for the day, or something that doesn't quite deserve its own note.

At work i would use it for things like 

"speak to manager about thermal calibration procedure"

I have these made automatically, and will explain how to do this. 

## Other 
The rest of my folders are just for actual topics, like "year 3 stfc" was for my placement. in there i have things like "meetings" and "digitiser" order this like you normally would, but only add sub folders if you need them. over doing folders can just make navigation harder, and you wont even be using them much

## Tasks
Tasks is a single note, not a folder, but using a plugin it automatically contains all of the "to do" things i have, so i will show you how to set it up.
![[Pasted image 20260909004737.png]]


---
## Essential Plugins Setup
To setup the core aspects of the same style of vault i have, you will need to setup the following plugins and community plugins. The difference is that one is made by the dev team, and one by some random guys, so treat community plugins more carefully, i only use ones with many downloads, all the ones in the setup I recommend.

### Obsidian Git
The purpose of this is to allow for version control that is synced to the cloud and other devices, i tried OneDrive but it tended to make loads of copies for no reason which was annoying. The guide i used to setup Obsidian git is https://youtu.be/ImrLbomFYA0?si=58Hq2QDw_WkM0XRw. The only difference is that i made a **fine grained personal access token (PAT)** so that if it were ever leaked, only my vault is compromised, not my whole GitHub, I recommend you do the same :), 
#### Notes on Obsidian Git
- You cannot use two devices simultaneously 
- it does not auto sync on closing obsidian
- you have to have all of your vault in one "master folder", I just have all sub folders in this master folder, so it doesn't really change anything (folder structure will be explained later):


The first issue has no solution, use the paid obsidian sync service. The second i recommend going to Settings  -> Hotkeys, and setting `Git: Commit and Sync then close Obsidian` to something sensible, i chose `Ctrl + Alt + S` for "save and close". This way when you close obsidian, you can do it this way, so you always have your most updated version on GitHub. 

#### Settings I use for Obsidian Git
Auto commit-and-sync interval: 1
Auto commit-and-sync after stopping file edits: True
Auto pull: off 
Pull on start up: True

The rest are the default settings. 

### Daily Note
This is a core plugin i use with the following setups: 
![[Pasted image 20260909011104.png]]

### Templater
This is a superior version to the basic obsidian "templates" functionality.  It allows you to auto create notes instantly, instead of having to insert templates, it also has some really cool features like: 

```
<% tp.web.daily_quote() %>
<% tp.date.now("YYYY-MM-DD") %>
```

The only relevant setting for this plugin is the "template folder location". The other thing i do is change the hotkey for "create new note", which is `Ctrl + N` i changed it so that it is now "create new note from template" this way whenever i try and make a new note, it prompts me to select a template. 

I have quite a few handy templates that i can share, please let me know if you want them. One thing to note that to make a template for Excalidraw you have to press the three dots in the top right corner of a note and press "open as markdown" to view the properties table. 

### Tasks 
This lets you make tasks, and add them to various views, so you can filter by completion date etc. I haven't ever touched the settings, and simply have a hotkey that means when i do `Ctrl + alt + T` it creates a task, and then i fill this in. Once I've done this, it appears in my "tasks" note, which means i can make a task wherever and view them all in one place. My tasks note looks like this in raw code (I took it from a forum somewhere but cannot remember):

``````
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
        let sortedGroups = [...groupUrgencyMap.keys()].sort((a, b) => { \
            let na = normalize(a), nb = normalize(b); \
            if (isNaN(na) && isNaN(nb)) return 0; \
            if (isNaN(na)) return 1; \
            if (isNaN(nb)) return -1; \
            return nb - na; \
        }); \
        query.searchCache[cacheKey] = { sortedGroups, normalize }; \
    } \
    let { sortedGroups, normalize } = query.searchCache[cacheKey]; \
    let taskGroup = getTaskGroup(task); \
    let index = sortedGroups.indexOf(taskGroup); \
    return taskGroup ? '%%' + (isNaN(normalize(taskGroup)) ? Number.MAX_SAFE_INTEGER : index) + '%% [[' + taskGroup + ']] (Normalized Urgency: ' + (isNaN(normalize(taskGroup)) ? 'N/A' : normalize(taskGroup).toFixed(2)) + ')' : ''; \
}
```



## All Tasks
```tasks
```
``````
### Excalidraw
Just download Excalidraw, and watch a basic YouTube tutorial, this is the least used of my plugins, but i think ill end up using it much more when i return from university. 

## Non Essential community plugins 
### Latex Suite
Makes latex code easier and faster to write, if your good at typing its (apparently) as fast as writing

### Calendar
Nice to have a calendar to view all daily notes, but is not essential 

### Zotero 
I am including this because last time i stressed how good this was. I since realized that i should just use Zotero as a reference manager, it is an excellent tool, and i was needlessly complicating things. I do not recommend using Zotero if you have seen any of my previous guides. 


## Workflow 
### General Workflow 
My general workflow is i create a new note, this is done with `Ctrl + N` and I therefore have to use a premade template, which means that i don't forget to complete any of the properties, therefore my notes stay organized. I always title my notes with the date in YYMMDD format at the start. Then i move the note to a relevant folder

Then i take a note, and if required i can add to-dos, go and make a drawing and then embed it, make a random note of something if required. Then once i am done on my session, i commit and sync and close obsidian **every time**. 

### Assisting Workflow: Hotkeys
Here all all the hotkeys created by me that I  use. Also try and learn as many commands as possible, the most important two are (in my opinion):

- Add file property: `Ctrl + ;`
- Open Command Pallet: `Ctrl + P`

![[Pasted image 20260909011940.png]]