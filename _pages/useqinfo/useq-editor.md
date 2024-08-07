---
permalink: /useqinfo/useq-editor/
title: "uSEQ Specs"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true
---

## URL

You can access the editor from

[https://useq.emutelabinstruments.co.uk/](https://useq.emutelabinstruments.co.uk/)

or you can use the embedded editors within this website.

## Connecting

Hit the ```Connect``` button in the corner, and you'll see a list of devices to connect to.  Names might vary across systems, it's possible the name will have 'rp2040' in it, and it's likely to be the name at the top of the list.


## Code

Code gets saved automatically in your browser's local storage as you type, and will be automatically restored when you reopen the web page from the same browser.  You can also manually load and save to your computer using the buttons.

## Running code

Place the cursor within a Modulisp statement, and hit ```ctrl-enter``` to run immeadiately or ```alt-enter``` to run at the start of the next bar.

## Getting help

Press alt-h for key commands.

## Structural Editing

We use [CodeMirror Clojure Mode](https://github.com/nextjournal/clojure-mode) structural editor.  The strucutral is aware of the syntax of the language being edited, so it will try and restrict you to only adding valid LISP statements by automatically managing the placement of brackets.  The 'slurping' and 'barfing' and snipping functions are particularly useful for managing lists. See the help window for hotkeys.

**Caveats**:  structural editing is a little different from standard editing, so it can be confusing at first.  For example the editor will not let you add or remove brackets that create unbalanced statements.  It's possible to expand brackets into the next statement or even to the end of the file.  If you're missing a bracket somewhere, use the bracket highlighting in combination with slurping and barfing fix the issue.  If everything gets in a mess, you can also highlight and delete any text.  We're working on a couple of enhancements to make this easier.


## Running the editor offline

It's possible to run the editor locally, with no need for internet access. 

1. Download or clone the git repo
2. You'll need to have node installed
3. To set up the packages, run ```npm install```
4. To run the editor: ```npm run dev```

## Disconnecting

Simply reload the webpage, and the connection will be dropped.

## Troubleshooting

### Connection problems

On linux or unix based operating systems, this page may have a solution:

[https://support.arduino.cc/hc/en-us/articles/360016495679-Fix-port-access-on-Linux](https://support.arduino.cc/hc/en-us/articles/360016495679-Fix-port-access-on-Linux)
