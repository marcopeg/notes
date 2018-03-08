VI Editor
==========================

[VI Commands](http://osr600doc.sco.com/en/FD_create/vi_summary.html)

    // open existing file or start create new one
    vi file-name

    // activate insert mode
    i

    // leave insert mode (go back to the command mode)
    Esc

    // save and close the file
    ZZ

    // go end of file
    G

    // go benginning of file
    gg
    
    // go end of line
    alt + €
    
    // go beginning of line
    shift + alt + ^

### Move the cursor by search for a path
    
To search the document for an occourrence and move the cursor there `switch to command mode` and type `/search-path` then enter. The cursor will move to the first occourrence.

### Move lines

in command mode you can type `22,23m 33` which means:

> move from line 22 to line 23 just after line 33.

### Permanent Settings

to rememeber this setting create a `~/.exrc` file and put the `set number` line in it. 
this file is basically the settings file for vi.

### Tab size

	:set tabstop=4

### Line numbers

    :set number        activate line numbers
    :set nonumber      deactivate line numbers


