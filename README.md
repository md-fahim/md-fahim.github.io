In this repo

- contents folder contains all the textual content for each page
- for each menu item/page there is a .md file and a html file
- header.txt in partial folder makes the link and handle the view
- jfif image format is used for faster image loading
- For creating a new *post*, need to update **post.json** in poster folder 
- While creating md file related to post use --- to get it into new page

If we want to create a new path/new menu item, we need to follow the steps

- Need to create abcd.html
- Also need a abcd.md for the text content
- Then in the partial folder, there is header.txt where need to include abcd.html (with luicid icons and others)


Major Changes

- Exclude about.html and all the contents in that file is shifted to index.html
