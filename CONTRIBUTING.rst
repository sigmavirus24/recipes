If you want to make a contribution to these docs, fork the project on any of 
the three places it is available (GitHub, BitBucket or Gitorious).

- Please ensure that what you are contributing is now already protected under 
  copyright by your employer and that you have full legal ability to 
  contribute the code.

- Please provide an explanation of situations where the code is useful and tag 
  your post with relevant words (to make it easily searchable).

- All source code longer than 25 lines (unless it is incomplete) should be in 
  a separate file and should be included in the text using the ``..  
  literalinclude::`` Sphinx directive. The file should reside in the 
  ``source/`` directory. For example:

  ::

        # file.py
        import requests

        # do magic with requests
        r = requests.get('http://httpbin.org/redirect/5',
                         allow_redirects=False)
        # etc


  ::

        Here is my awesome ``file.py`` which does ...

        .. literalinclude:: source/file.py
            :language: python

        etc.

- All complete code files should pass flake8_.

- All links should have the URL at the bottom of the document to make reading 
  the plain reStructuredText easier on future editors. Think of links as 
  footnotes.

- Add yourself to ``AUTHORS.rst`` if you're contributing your own recipe.

- Add yourself to ``EDITORS.rst`` if you're contributing edits to other 
  people's recipes.

- The recipes are not strictly restricted to python, but should be related to 
  python.


.. links
.. _flake8: http://pypi.python.org/pypi/flake8
