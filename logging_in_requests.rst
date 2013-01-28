How to Add Logging to a File When Using Requests
================================================

:author: Ian Cordasco
:tags: requests, logging, 1.x

In the latest versions of [Requests](https://github.com/kennethreitz/requests) 
the ability to configure sessions was removed and with it the ability to set a 
``verbose`` option with a file-like object. Recently, a user stepped into 
``#python-requests`` on Freenode and asked how to restore the behavior from 
before the refactor.

If one goes back in time
(``git checkout 9dce7861134c60596b91dfa8eda2ff66f5c5d5e5``), you will see 
(using ``grep``) that the call took place in ``models.py`` and simply 
formatted a string using ``datetime.now().isformat()``, the request method and 
the URL. We no longer have that available to us, so the next best thing is to 
use ``urllib3``'s built in logging. If you wanted, you could make it log to 
stderr by doing

::

    import requests
    requests.packages.urllib3.add_stderr_logger()

But then you'll only have the logged output printed to your terminal and that 
is not what the user wanted. Another way of doing this is to use the logging 
module yourself

::

    import requests
    import logging

    logger = logging.getLogger('requests.packages.urllib3')
    fh = logging.FileHandler()
    # ...
    logger.addHandler(fh, level=logging.DEBUG)

Why does this work?
-------------------

``urllib3`` uses it's namespace to register its own logger with the logging 
module (roughly speaking). In this instance, ``urllib3`` isn't a standalone 
package but part of ``requests``, so its proper namespace is 
``requests.packages.urllib3``. Using that information, you can get the 
``logging.Logger`` instance and configure it to your needs and desires. Note, 
however, that this will give you more detail than the old ``verbose`` 
configuration option did. As such, choose your level as you see fit.
