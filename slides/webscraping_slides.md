Webscraping in R
========================================================
author: William Marble
date: August 11, 2016

Introduction
========================================================


- Oftentimes there is valuable information online that is not nicely formatted for statistical analysis
- Some organizations make their data easily downloadable, but this isn't always the case
- Other times, the creators of the data weren't intending to create a database for researchers
- This tutorial will show you how wrangle semi-structured data from the internet into a spreadsheet-like format

Two political science examples
========================================================

Nielson and Simmons (2015) 
- Nielson and Simmons are interested in studying the effect of signing treaties on countries' standing in the international community
- There might be both tangible and intangible benefits 
- Measure intangible benefits using European Union press releases

Two political science examples
========================================================

Grimmer (2013)
- Grimmer is interested in how members of Congress communicate with constituents
- Focus on position taking or credit claiming? 
- Uses a large corpus of press releases from members of Congress

Two political science examples
========================================================

- The authors of the press releases didn't intend to create a database for researchers
- Nevertheless, in both cases they unintentionally created valuable data for social scientists
- By using ``webscraping'' tools, the researchers were able to give structure to the data that allowed them to generate new insights

Overiew of the rest of the tutorial
========================================================

Follow along by downloading notes and code:
http://stanford.edu/~wpmarble/webscraping_tutorial/webscraping_tutorial.pdf
http://stanford.edu/~wpmarble/webscraping_tutorial/code.R

1. Workflow for webscraping
2. Brief primer on HTML
3. Tools for webscraping
4. Simple example
5. In-depth example
6. Very Short Intro to APIs (time permitting)



When is webscraping useful? 
========================================================

- Data structured in a consistent manner across many web pages
- When there's enough data that it's too much to do by hand

When is webscraping not useful?
========================================================

- When you don't know how to navigate to the web pages you want 
- When the data aren't structured in a consistent manner across web pages
- When writing the code would take longer than doing it by hand (!!!)

========================================================
<h1 style="text-align: center;" markdown="1">Webscraping workflow</h1>


========================================================

1. Identify information on the internet that you want to use

2. Figure out how to automatically navigate to the web pages. Is there a consistent URL format? E.g., http://url.com/year/month/day.html

3. Figure out how to extract the information you want using features on the website (HTML markers and/or text markers)

4. Write a script to extract, format, and save the information you want using the flags you identified

5. Loop through all the websites from step 2, applying the script to each of them

6. Do some awesome analysis on your newly unlocked data!


========================================================

1. <font color="A9A9A9">Identify information on the internet that you want to use</font>

2. <font color="A9A9A9">Figure out how to automatically navigate to the web pages. Is there a consistent URL format? E.g., http://url.com/year/month/day.html</font>

3. Figure out how to extract the information you want using features on the website (HTML markers and/or text markers)

4. Write a script to extract, format, and save the information you want using the flags you identified

5. <font color="A9A9A9">Loop through all the websites from step 2, applying the script to each of them</font>

6. <font color="A9A9A9">Do some awesome analysis on your newly unlocked data!</font>



Primer on HTML
========================================================

HTML tells browsers how to display information. Unstanding the basics is important for webscraping. 

An example of what a website looks like under the hood:
http://stanford.edu/~wpmarble/webscraping_tutorial/html/silly_webpage.txt


Primer on HTML
========================================================

- Elements are surrounded by code that tells web browsers what they are -- &lt;element&gt;some text &lt;/element&gt;
- There is sometimes extra information, like "class" or "id" -- &lt;p class="someclass"&gt; paragraph &lt;/p&gt;
- Elements are nested
- In Chrome,  right click, then click "View Page Source"
- Use Chrome extension SelectorGadget to select elements you want


rvest, the webscraping package for R
========================================================

Read in a webpage using **read_html()**

```r
my_webpage = read_html("http://stanford.edu/~wpmarble/webscraping_tutorial/html/silly_webpage.html")
my_webpage
```

```
{xml_document}
<html>
[1] <head>\n    <title>This is the title of the webpage</title>\n  </head>
[2] <body>\n    <h1>This is a heading</h1>  \n    <p class="notThisOne"> ...
```

rvest, the webscraping package for R
========================================================
Select "nodes" using a CSS selector (the thing inside the brackets) using **html_nodes()**

```r
all_paragraphs = html_nodes(my_webpage, "p")
all_paragraphs
```

```
{xml_nodeset (3)}
[1] <p class="notThisOne">This is a paragraph</p>
[2] <p class="thisOne">This is another paragraph with a different class! ...
[3] <p class="divGraf"> \n        This is a paragraph inside a division, ...
```

rvest, the webscraping package for R
========================================================

Extract elements of the nodes using **html_text()**, **html_attrs()**, **html_name()**, etc.

```r
p_text = html_text(all_paragraphs)
p_text
```

```
[1] "This is a paragraph"                                                                     
[2] "This is another paragraph with a different class!"                                       
[3] " \n        This is a paragraph inside a division, along with a \n        a link.\n      "
```

Much more capability, but these are the main commands you need. Plenty of examples to come.

Regular expressions
========================================================

- Often you'll see a pattern in text that you can use to pull out what you want
- Regular expressions (or regex) is a language that allows you to precisely specify these patterns
- Full treatment beyond the scope of this presentation, but a few R commands

Regular expressions
========================================================

**grep(pattern, string)** takes a string vector and returns a vector of the indices of the string that match the pattern

```r
mystring = c("this is", "a string", "vector", "this")
grep("this", mystring)
```

```
[1] 1 4
```

Regular expressions
========================================================

**grepl(pattern, string)** takes a string vector as an input and returns a logical vector that says whether each element of the string matches the pattern

```r
mystring = c("this is", "a string", "vector", "this")
grepl("this", mystring)
```

```
[1]  TRUE FALSE FALSE  TRUE
```

Regular expressions
========================================================

**gsub(pattern, replacement, string)** finds all the instances of pattern in string and replaces it with replacement


```r
mystring = c("this is", "a string", "vector", "this")
gsub(pattern="is", replacement="WTF", mystring)
```

```
[1] "thWTF WTF" "a string"  "vector"    "thWTF"    
```

Examples
========================================================

Notes:
http://stanford.edu/~wpmarble/webscraping_tutorial/webscraping_tutorial.pdf

Code:
http://stanford.edu/~wpmarble/webscraping_tutorial/code.R



========================================================

<h1> Questions/comments: wpmarble@stanford.edu!</h1>
