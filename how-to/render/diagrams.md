# How to render mermaid diagrams in HTML exported files

  Before having Pandoc integrated into Abricotine, the way to get PDF was to export HTML ( Export as HTML menu item ) and print the result with a PDF printer.

  The following tutorial aims to show how to create a template that will replace all *mermaid* tagged blocks into actual diagrams.

  The sequence is this one :
1. Duplicate Github template as Diagram template 
2. Add some code to the new template
3. Export document as HTML
	
  The sequence may be described with *mermaid* like this :
  
``` mermaid
sequenceDiagram
   Github template ->>Diagram template:duplicate
   Diagram template->>Diagram template:Add some code
   Diagram template->>Abricotine:install
   Abricotine->>Browser:Export document as HTML	
```

## Duplicate Github template as Diagram template 

  First of all, open the directory containing the templates used when HTML export is wanted : **Edit -> Open Config Directory**.
  
  Step into the **template** directory and duplicate the **github** directory and rename the new one as **diagram**.
  
  Step into the **diagram** directory and edit the **template.json** file.
  
  Replace all the **Github** occurences whith **Diagram**.
  
  Now you have a diagram template ready to be modified.
  
## Add some code to the new template

  When Abricotine exports a document as HTML, it dumps all the text from the editor where the tag **$DOCUMENT_CONTENT** is found into the template. 
  
  In the Diagram template, the tag is enclosed by an **article** tag.
  
  The piece of code that will be added loops over the content of the **article** container and make the following changes :
  - for each block, change **pre**/**code** couple with a single **div** tag
  - replace class **lang-mermaid** with **mermaid**
  - Add mermaid standard code to init the rendering

  To do this, edit the **template.html** file within the **diagram** directory.
  
  ***After*** the **article** closing tag and ***before*** the **body** closing tag, add the following lines :
  
``` javascript
        <script src="https://unpkg.com/mermaid@8.0.0/dist/mermaid.min.js"></script>
        <script>
var article = document.getElementsByTagName('article')[0];

for(var i = article.children.length - 1; i >= 0 ; i--) {
    child = article.children[i]
    if( child.tagName.toUpperCase() == 'PRE'  && child.firstChild.className == 'lang-mermaid') {
        child.outerHTML = child.innerHTML.replace('<code','<div').replace('lang-mermaid','mermaid').replace('</code>','</div>')
    }
}

mermaid.initialize({theme: 'neutral'});
mermaid.init();
        </script>
 ```
  
  Save the file.
  
## Export document as HTML

  To see the new template in the possible choices when exporting the document as HTML, you have to reopen Abricotine.
  
  You can use this very document to test. Here is the way to go :
  
``` mermaid
graph LR
menu-file[File] -->|Go to| menu-export-html(Export as HTML)
menu-export-html --> diagram-template{Diagram template}
diagram-template -->|save as...| result_file[result.html]
result_file -->|Open into | MyBrowser
```
  

## Make it offline

  To make the call to mermaid using offling library can be done merely replacing  ***https://unpkg.com/mermaid@8.0.0/dist/mermaid.min.js*** with  ***$ASSETS_PATH/mermaid.js*** provided that **mermaid.js** has been placed into the **assets** directory of the **diagram** template.
