pyserv: A simple bash function to use SimpleHTTPServer
======================================================

:Author: Ian Cordasco
:Origin: http://www.coglib.com/~icordasc/blog/2012/11/one-liners.html
:Tags: bash, python, SimpleHTTPServer, screen,

The Final Code
--------------

.. code-block:: bash

    pyserv() {
        if [[ -n $1 ]] ; then
            port="$1"
        else
            port="8000"
        fi

        old_dir="$(pwd)"

        if [[ -n $2 ]] ; then
            cd $2
        fi

        screen -dmS pyserv${port} python -m SimpleHTTPServer ${port}
        echo "http://localhost:${port}"
        echo "screen -r pyserv${port}"

        if ! [[ "$(pwd)" == $old_dir ]] ; then
            cd $old_dir
        fi
    }


The Evolution
-------------

I have started to use a lot of tools that generate HTML for me and place them 
in pre-determined directories (e.g., coverage.py, sphinx, pelican). I can 
always use Firefox to navigate to those directories 
(``file:///home/sigma/sandbox/project/_build/html/index.html``) but that is 
annoying without a doubt. Sure I could bookmark that, but what if I move the 
project directory or change some settings? Wouldn't it be nice to just type a 
regular URL into the location bar? One solution is to just use python's 
``SimpleHTTPServer`` module from the command-line like so

.. code-block:: bash

    $ cd _build/html
    $ python -m SimpleHTTPServer 8080
    ^C
    $ cd -

But that takes over the terminal and sending it to the background introduces 
ugly interlacing of output. So I then started putting it in a screen session

.. code-block:: bash

    $ cd _build/html
    $ screen -dmS pyserver python -m SimpleHTTPServer 8080
    $ cd -

Using ``-dm`` allows me to immediately place the screen session in the 
background instead of attaching it to the terminal. The ``-S`` option allows 
me to give it a name so I can do ``screen -r pyserver`` instead of ``screen 
-ls; screen -r pidnumber``. But constantly having to type out the screen 
command was beginning to annoy me so I wrote a function 

.. code-block:: bash

    pyserv() {
        if [[ -n $1 ]] ; then
            port="$1"
        else
            port="8000"
        fi

        screen -dmS pyserv${port} python -m SimpleHTTPServer ${port}
        echo "http://localhost:${port}"
        echo "screen -r pyserv${port}"
    }

This allowed me to not even need to set up a port by providing a default and 
echoed the url and screen command to the terminal for me. I could then just 
type

.. code-block:: bash

    $ cd _build/html ; pyserv ; cd -

But constantly having to type ``cd`` was beginning to annoy me as well and 
this resulted in the final iteration of this function.

.. code-block:: bash

    pyserv() {
        if [[ -n $1 ]] ; then
            port="$1"
        else
            port="8000"
        fi

        old_dir="$(pwd)"

        if [[ -n $2 ]] ; then
            cd $2
        fi

        screen -dmS pyserv${port} python -m SimpleHTTPServer ${port}
        echo "http://localhost:${port}"
        echo "screen -r pyserv${port}"

        if ! [[ "$(pwd)" == $old_dir ]] ; then
            cd $old_dir
        fi
    }

It has all the simplicity of the original function with more functionality:

- You can simply call ``pyserv`` in a directory and it will work

- It gives you the URL and screen alias.

- It will now change directories for you and serve the content from there, 
  while placing you back in your original directory

A couple simple invocations are

.. code-block:: bash

    $ pyserv 8080 _build/html
    $ pyserv 9090 htmlcov/

By placing this in your ``.bashrc``, ``.bash_profile`` or any other bash 
related file, (and then sourcing it ``. (filename)``) you can have immediate 
access to this functionality.
