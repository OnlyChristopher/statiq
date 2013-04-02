statiq
======

A node.js static website generator

### Install:

    npm install -g statiq

### Basic usage:

Make template, source and distribution folders:

    $ mkdir tpl
    $ mkdir src
    $ mkdir dst

Then a statiq.json file:

    $ nano statiq.json
    
    {
        "defaultTemplate": "index.html",
        "templatesRoot": "tpl/",
        "inputRoot": "src/",
        "outputRoot": "dst/"
    }

Build a directory structure in src folder, with Markdown-formatted pages, like

    src/index.md
    src/about.md
    src/docs/index.md

Sample index.md:

    Welcome!
    =======
    
    This is a *test page*.

Then make an index.html file containing a mustache template in your tpl folder:

    <!doctype html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>Test Page</title>
    </head>
    <body>
        <div id="main">
            {{{document}}}
        </div>
    </body>
    </html>

The `document` variable contains the parsed document.
Finally, run:

    $ statiq

And you'll get all the parsed pages in your distribution folder:

    dst/index.html
    dst/about.html
    dst/docs/index.html

### Local variables

You can set local variables in each source file, by placing a json object in its first line:

    {
      "title": "Index page"
    };
    Welcome!
    ========
    ...


Notice the required semicolon at the end of the json object.
Then your template could look like:

    <!doctype html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>{{title}}</title>
    </head>
    <body>
    ...

### Sections

Consider a multicolumn layout like this:

    <!doctype html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>{{title}}</title>
    </head>
    <body>
        <div id="main">
            <div class="left-column">
                {{{left}}}
            </div>
            <div class="right-column">
                {{{right}}}
            </div>
        </div>
        <div id="footer">Copyright &copy; {{year}}</div>
    </body>
    </html>

Instead of using the `document` variable (which contains all the .md file), you can declare sections like this:

    {
      "title": "Multi column",
      "year": 2013
    };
    
    <<left
    Welcome!
    ========
    
    Lorem ipsum dolor sit amet blah blah.
    left;
    
    <<right
    ### Useful links
    [Google](http://www.google.com/)
    [Wikipedia](http://www.wikipedia.org/)
    right;

This is a [Heredoc](http://en.wikipedia.org/wiki/Here_document)-ish declaration.
Sections start in a newline with `<<` followed by the section name, and another newline.
Then, you close the section with a newline followed by the section name a semicolon, and a newline again.
Section names are be case-sensitive alphanumeric strings.

### Global variables

Use the "globals" attribute of the statiq.json object:

    {
      ...
      "globals": {
        "sitename": "My awesome website",
        "...": "..."
      }
    }

Those variables will be available across the site:

    <!doctype html>
    <head>
       <title>{{title}} - {{sitename}}</title>
    </head>
    <body>
    ...


#### That's all

First time doing stuff like this, don't hate me :)
