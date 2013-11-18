Bash Scripting
==============

[http://tldp.org/HOWTO]()

## Variables

    #!/bin/bash
    TEST="a string value"
    echo $TEST

- if you add spaces arount "=" for code styling then the script fail!
- if you avoid "$" in `echo` then `TEST` become a string input

To create a variable local to a function prepend with `local`:

    TEST="global value"
    function foo {
      local TEST="local value"
      echo $TEST
    }
    
    # test it out:
    echo $TEST
    foo
    echo $TEST



## Conditionals

### if

    if [ true != false ]; then
	    echo 'hello'
    fi
    
- spaces matters in the first line
- there are no strict controls like `===`

### if / else

    if [ true != false ]; then
	    echo 'hello'
	else
	    echo 'world'
    fi
    



## Cycles

### for

    for i in "a" "b" "c"; do
        echo item: $i
        echo ---
    done
    
`for` cycle through a given input.  
a list is everything space separated.
    
    # classic for
    for i in `seq 1 10`; do
	    echo $i
    done

### while

    i=0
    while [ $i -lt 10 ]; do
    	echo i = $i
    	let i=i+1
    done
    
!! pay attention on `let i=i+1`  
I found no ways to do it different!

### until



## Functions
    
    # declare
    function hello {
        echo Hello World!
    }
    
    # use it
    hello
    
### use arguments

    function say {
        echo $1
    }
    
    say Hello!
    #-> "Hello!"
    
    say Hello World!
    #-> "Hello"
    
above function `echo` only the first param!

`$#` number of params

`$@` params list
