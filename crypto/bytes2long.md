# bytes2long

Just insert:

```python
from Crypto.Util.number import bytes_to_long;
N = 0;
for line in chatgpt_strings:
    N += int.from_bytes( line, 'little' )

x = N;
```

into the challenge file and profit.
