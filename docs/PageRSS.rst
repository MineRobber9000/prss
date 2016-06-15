PageRSS
=======

``PageRSS(title,link,description,pubDate)``  

The main class of the module.

* ``title``: The title of your page.
* ``link``: The link to your page on the internet.
* ``description``: The description of your page.
* ``pubDate``: The reults of ``time.localtime()``.

``PageRSS.addItem(title,link,description)``

Add an item to the RSS feed.

* ``title``: The title of the item.
* ``link``: The location of the item.
* ``description``: A description of the item.

``PageRSS.make()``

Generates the RSS feed and returns the contents of the RSS file.
