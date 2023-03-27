## String

```python
ord(str) -> int
chr(int) -> str
print(str, end="") # no newline
string.isprintable()
```

## Hex

```python
# convert int to hexstr
hex(int)  # with '0x' prefix
format(int, 'x').decode()

# convert hexstr to bytes
binascii.unhexlify(hexstring)
binascii.unhexlify(hexstring[2:]) # exclude '0x'
bytes.fromhex(hexstring)

# convert bytes to hex
binascii.hexlify(bytes) # hexstr in bytes
# binascii.hexlify(b'hell0') -> b'68656c6c30'


```

```python
def xor_a5(bytes):
    ret = binascii.hexlify(bytes)
    ret = int(ret, 16)
    ret ^= 0xa5a5a5a5
    return ret
```

## Math

```python
from Crypto.Util.number import inverse
d = inverse(e, phi) # modular inverse
m = pow(c, d, n) # c^d mod n

from Crypto.Util.number import long_to_bytes
s = str(long_to_bytes(m)) # convert int m to bytes then convert to string
```

```python
from gmpy2 import iroot
c = <bigint>
n = <bigint>
e = 3
m, is_true_root = iroot(i*n+c, e)
```

```python
bytes([a ^ b])
```

## Request

```python
cookie = {"auth": "r0oT"} # `cookies` is a dict
request.get(host, cookies=cookie)
```

## Itertools

```python
import itertools
for i in itertools.product(string.digits, repeat=6):
	key = ''.join(i)
# key = '000000', '000001', '000002'... '999999'


all_strs_combinations = list(itertools.combinations(given_strs, max_len))
# it returns like [] [1] [2] [3] [1, 2] [1, 3] [2, 3] [1, 2, 3]
```

# pwntools

```python
from pwn import *
conn = remote(host, port)
conn = process(file)

conn.recvuntil(string)
conn.send(string)
conn.sendline(string) # with a trailed \n
conn.recv() # recv return bytes so:
conn.recv.decode() # is more preferred

p32(int) -> string # convert int to 32bits payload string
p64(int) -> string # similar

```