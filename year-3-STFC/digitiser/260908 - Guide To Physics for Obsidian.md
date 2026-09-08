---
tags:
  - note
created: 2026-09-08
topic:
module:
loc:
---
# Links: 
[[example note here]]

# Notes:
## Explaining what you see above 
If you are confused as to what you see above, it is a **properties** table, which you will be able to easily set up. It allows you to give your notes different properties, which will allow for easy searching later on. The **Links** section above is a place for linking any notes you make that you think may be relevant, and this is done by using "\[\[example note here\]\]".  I don't worry much about links, and find them a bit gimmicky, but they are easy enough to do so i use them anyway. 

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

You should setup in the "Files and links" section in the settings to set you attachment folder to this folder, so everything automatically goes here. (Lazy and simple)

---
## Plugins Setup
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

### Excalidraw
