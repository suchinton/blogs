# GSoC '23 Blogs @ TLF-AGL

This repo hold the static content to be hosted on my [Blog Site](https://suchinton.github.io/).

### To add new posts we use the command given below

```
hugo new content/articles/firstpost.md
```

### To convert asset links into raw links

```
replace "github.com" with "raw.githubusercontent.com" 
replace "/blob/" with "/"
```

### To generate public static content for blog site [Git Sub-dir]()

```
hugo -t digitalgarden 
```

### To run site locally 

```
hugo server
```