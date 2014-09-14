Mac Terminal
==========================

## Tabs Management

You can open a new tab with `cmd + t` then you can switch between tabs with `Shift + Cmd + arrows` and you close a tab by `cmd + t`.



## ShortCuts

    # move the cursor at the beginning of the line
    Ctrl + a
    
    # move the cursor at the end of the line
    Ctrl + e
    
    # clear all following characters from cursor position
    Ctrl + k
    
    # move cursor right (like arrow right)
    Ctrl + f
    
    # move cursor left (like arrow left)
    Ctrl + b
    
    # clear the screen
    Ctrl + l
    Cmd + k
    
    # redo the very last command
    !!
    
    
## VI Editor

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
    
[VI Commands](http://osr600doc.sco.com/en/FD_create/vi_summary.html)    

    
### Move the cursor by search for a path
    
To search the document for an occourrence and move the cursor there `switch to command mode` and type `/search-path` then enter. The cursor will move to the first occourrence.





## File Permissions

    # change recursively to all files and folders
    chmod -R 777 /var/www


[http://djlab.com/2009/06/cpanel-suphp-chmod-all-files-644-and-all-folders-755/]() list some commands to change 755 to folders and 644 to files

    # add executable permission to a file
    chmod +x ./file-name



## Compress

    # create zip archive without external folder structure
    zip -r archive.zip ./folder-to-zip
    
## Alias

    // ~/.bash_profile (add to the file)
    alias mou='/Applications/Mou.app/Contents/MacOS/Mou'

makes the command `mou` general available and linking to the real executable.