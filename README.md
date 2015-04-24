# PIXLSUS
This is the repository for being able to write posts for https://pixls.us and to preview the results against the site style.

## How To
Articles are held in their own sub-folders.  For example, this particular repository has a single article located in the folder `Sample`.  Inside this folder is the markdown file for the article, `index.md`, as well as an optional stylesheet for the page `index.css`.  New articles should be created in a sub-folder in a similar fashion, and all assets should be self-contained in that folder.

1. Simply fork this repository to your own account.
2. Create your own subfolder for your article.
3. The main markdown file for an article is usually named `index.md` inside the sub-folder.  (See the `Sample` sub-folder in this project for an example).
4. Your article sub-folder should be self-contained with the `index.md` file, the optional `index.css` stylesheet, and any image assets needed.
5. Once you have at least a single commit to your forked repository, you can preview the results of your changes at the compiled version of these pages at: `http://YOUR-GITHUB-USERNAME.github.io/PIXLSUS/YOUR-SUBFOLDER-NAME`<br/>
For example, to view the main page here, you can go to: http://patdavid.github.io/PIXLSUS/<br/>
and to view the `Sample` post, simply go to: http://patdavid.github.io/PIXLSUS/Sample/

 > Before the preview pages will be built you must have at least one commit in your fork (creating your own sub-folder and index.md file should be enough)
 > You should give it a few minutes to build the first time (though subsequent builds should be faster).



To create a new directory directly in the web-interface, hit the "+" to create a new file in the repository, then simply use forward slashes in the filename field to automatically create a folder:
<div style="border: dashed 1px gray;"><img src="/styles/create-folder.gif" alt=""/></div>
3. The article markdown file is *normally* named `index.md`.
4. Remember to include front-matter in this file:
```
---
title: A Title
date: 2015-04-20T11:47:07-05:00
---
The rest of the actual file contents...
```
6. 
