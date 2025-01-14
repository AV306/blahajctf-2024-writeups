# SSTI golfing 

A code golfing challenge for SSTI! [Instance](http://golf.c1.blahaj.sg/) (down)

## Restrictions

The server filters out a lot of good stuff, like `__builtins__` and `lipsum` (used by HackMD).
There’s also a 65 character limit on input, which rules out most payloads.

## Bypassing

Someone managed to drop `__builtins__` into an entry in Flask’s config global object (which I happened to see before FS reset it), but my solution is a bit less complicated :P

[niebardzo's SSTI payloads article](https://niebardzo.github.io/2020-11-23-exploiting-jinja-ssti/) was an interesting read, but ultimately unneeded.

Listing the available classes is easy — `{{dict.mro()[1].__subclasses__()}}` (34 char) does the trick.

Of interest (to me) was `subprocess.Popen` (at index 351), which would let us spawn a subprocess like `cat`. Unfortunately, a direct `Popen` payload is too long :(

So, what we can do is drop `subprocess.Popen` into a random field of the Flask global config object with something like `{{config.update(a=dict.mro()[1].__subclasses__()[351])}}`, and just `{{config.a('cat *',shell=1,stdout=-1).communicate()}}` to profit!

([Intended solution](https://rwandi-ctf.github.io/BlahajCTF2024/SSTI-Golf/) actually involves breakng the payload into pieces, so it's not *really* golfing)

<br>

Fun fact: you can add files to the server. Thankfully, the flag is write-protected.

![image](https://github.com/user-attachments/assets/6c52c8a8-54f7-460a-8c00-b1c918dd72e3)


