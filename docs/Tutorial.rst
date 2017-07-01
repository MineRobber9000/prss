Tutorial
========

Here is a simple example of how to take the latest 20 commits of a GitHub repository and makes an RSS feed.

First, we need access to both the requests library, the time module, and PRSS. Let's get that now: ::

    import requests,prss,time

Now that we have that, I would suggest getting the commit list. This example is for the "sonicretro/s2disasm" repository: ::

    n = requests.get('http://api.github.com/repos/sonicretro/s2disasm/commits')

Next, we can instantiate the PageRSS class: ::

    r = prss.PageRSS("Latest Commits to sonicretro/s2disasm","https://github.com/sonicretro/s2disasm","The latest commits from the Sonic Retro Sonic the Hedgehog 2 disassembly.")

Now we need to get the details on the latest commits, which is simple enough: ::

    lc = n.json()
    for c in lc:
        r.addItem(c['commit']['message'],c['html_url'],'"{}" by {} ({})'.format(c['commit']['message'],c['commit']['author']['name'],c['tree']['sha']))

And finally, we must put the RSS in a file (example is "~/public_html/srs2.xml"): ::

    import os
    with open(os.path.expanduser("~/public_html/srs2.xml"),"wb") as f:
        f.write(r.make())

The full code should look somewhat like: ::

    import requests,prss,time
    n = requests.get("https://api.github.com/repos/sonicretro/s2disasm/commits")
    r = prss.PageRSS("Latest Commits from sonicretro/s2disasm","http://github.com/sonicretro/s2disasm","The latest commits from the Sonic Retro Sonic the Hedgehog 2 disassembly.")
    lc = n.json()
    for c in lc:
        r.addItem(c['commit']['message'],c['html_url'],'"{}" by {} ({})'.format(c['commit']['message'],c['commit']['author']['name'],c['tree']['sha']))
    import os
    with open(os.path.expanduser("~/public_html/srs2.xml"),'wb') as f:
        f.write(r.make())
    n.quit()
