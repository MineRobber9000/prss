Tutorial
========

Here is a simple example of how to take the latest 20 posts of an NNTP newsgroup and makes them an RSS feed.

First, we need access to both NNTPLib, the time module, and PRSS. Let's get that now: ::

    import nntplib,prss,time

Now that we have that, I would suggest getting the connection. This example is for the "gmane.comp.python.committers" newsgroup: ::

    n = nntplib.NNTP('news.gmane.org')

Next, we can instantiate the PageRSS class: ::

    r = prss.PageRSS("Python Committers Email List Latest 20","http://blog.gmane.org/gmane.comp.python.committers","The latest 20 posts from the CPython committers email list.",time.localtime())

Now we need to get the details on the latest 20 posts, which we can get by first getting the details of the newsgroup: ::

    resp, count, first, last, name = n.group('gmane.comp.python.committers')

Then we must get the last 20 posts: ::

    resp, subs = n.xhdr('subject',(last-20)+"-"+last)
    for id, sub in subs:
        resp, body = n.body(id)
        r.addItem(sub,"http://permalink.gmane.org/gmane.comp.python.commiters/{!s}".format(id),body)

And finally, we must put the RSS in a file (example is "~/public_html/gcpc.xml"): ::

    import os
    with open(os.path.expanduser("~/public_html/gcpc.xml"),"wb") as f:
        f.write(r.make())

Now we can close the NNTP connection with ``n.auit()``.

The full code should look somewhat like: ::

    import nntplib,prss,time
    n = nntplib.NNTP("news.gmane.org")
    r = prss.PageRSS("Python Committers Email List Latest 20","http://blog.gmane.org/gmane.comp.python.committers","The latest 20 posts from the CPython committers email list.",time.localtime())
    resp, count, first, last, name = n.group("gmane.comp.python.committers")
    subs = n.xhdr("subject",(last-20)+"-"+last)
    for id, sub in subs:
        resp, body = n.body(id)
        r.addItem(sub,"http://permalink.gmane.org/gmane.comp.python.committers/{!s}".format(id),body)
    import os
    with open(os.path.expanduser("~/public_html/gcpc.xml"),'wb') as f:
        f.write(r.make())
    n.quit()
