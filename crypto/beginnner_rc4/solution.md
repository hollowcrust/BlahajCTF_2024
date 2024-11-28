# Beginner RC4

```Python3
def RC4_KeySchedule(key, S):
    j = 0
    for i in range(len(key)):
        j = (j + S[i] + key[i]) % 16
        S[i], S[j] = S[j], S[i]
    return S

def encrypt(message, key):
    state = RC4_KeySchedule(key, list(range(16))) # This was taken from RC4, surely its secure!
    output = []
    for i in range(len(message)):
        m = message[i]
        s = state[i % len(state)]
        output.append(m ^ s)
        if i % len(state) == 0:
            state == RC4_KeySchedule(state, state) # this must be secure, right? You can't figure next xor stream data without knowing the state!
    return output

from os import urandom

key = urandom(16)           
flag = b"The flag is blahaj{?????????????????????????????????????}"
ct = encrypt(flag, key)
print(ct)
"""
[83, 103, 109, 44, 99, 106, 98, 96, 32, 104, 122, 43, 96, 97, 107, 102, 101, 105, 124, 110, 56, 106, 127, 80, 99, 56, 102, 53, 94, 121, 57, 119, 119, 82, 127, 125, 62, 113, 102, 88, 58, 105, 51, 118, 93, 117, 52, 95, 121, 48, 109, 54, 113, 105, 115, 35, 116]
"""
```

We are given an RC4 algorithm snippet as above. The algorithm works by XOR-ing each byte of the message with a byte of the key, and the key changes every 16 bytes using `RC4_KeyScheduler()`. We don't know the original key, but if we can get one specific key state, we can derive the key states for the whole process.<br/><br/>
Notice that besides the flag header `blahaj{`, the text `The flag is ` is also encrypted and hence there are 19 characters/bytes known to us. The `encrypt()` function changes the state of the key when the index interator `i` is a multiple of 16, which means the same key is used for characters from index 1 to 16 (0-based). We can XOR the bytes in flag with the numbers in `ct` from index 1 to 16 (the number received at index 16 needs to be move to the front) to get the key state. We can now run the whole RC4 process on `ct` to get the flag.<br/>

```Python3
def RC4_KeySchedule(key, S):
    j = 0
    for i in range(len(key)):
        j = (j + S[i] + key[i]) % 16
        S[i], S[j] = S[j], S[i]
    return S

def decrypt(message, state):
    output = []
    for i in range(1, len(message)):
        m = message[i]
        s = state[i % len(state)]
        output.append(m ^ s)
        if i % len(state) == 0:
            state = RC4_KeySchedule(state, state) 
    return output

flag = b"The flag is blahaj{?????????????????????????????????????}"
ct = [83, 103, 109, 44, 99, 106, 98, 96, 32, 104, 122, 43, 96, 97, 107, 102, 101, 105, 124, 110, 56, 106, 127, 80, 99, 56, 102, 53, 94, 121, 57, 119, 119, 82, 127, 125, 62, 113, 102, 88, 58, 105, 51, 118, 93, 117, 52, 95, 121, 48, 109, 54, 113, 105, 115, 35, 116]

key = [0 for i in range(16)]
for i in range(1, 17):
  key[i % 16] = ct[i] ^ flag[i]

m = decrypt(ct, key)
for c in m:
  print(chr(c), end='')
```

## Flag
    ```
    blahaj{d0nt_m4k3_y0ur_st4te_4a5y_t0_r3c0ver!}  
    ```
