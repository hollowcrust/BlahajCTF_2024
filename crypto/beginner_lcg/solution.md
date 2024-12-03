# Beginner LCG

### chal.py
```Python3
from Crypto.Util.number import getPrime, bytes_to_long
from secrets import randbelow

FLAG = "blahaj{redacted}"
f = bytes_to_long(FLAG.encode())

def LCG(a, m, x, c):
    while True:
        x = (a * x + c) % m
        yield x

m = getPrime(128)
# you can have m
print(f"{m = }")
# but i'll derive all the other parameters from the secret flag!
a = f % m
x = (f >> 128) % m
c = (f >> 256) % m
rng = LCG(a, m, x, c)

outputs_left = 3

while True:
    option = input(f"""[1] See output ({str(outputs_left)} outputs left)
[2] Check flag
""")
    if option == "1":
        if outputs_left > 0:
            output = next(rng)
            print(output)
            outputs_left-=1
        else:
            print("You know too much!")
    elif option == "2":
        print("Prove you know the flag!")
        print("Whats a?")
        if str(a) != input():
            continue
        print("Whats c?")
        if str(c) != input():
            continue
        print("Ok, you must know the flag then... here it is:")
        print(FLAG)
        quit()
    else:
        quit()
```
As suggested from the title, the challenge involves the problem of <a href="https://en.wikipedia.org/wiki/Linear_congruential_generator">Linear Congruential Generator</a>. The LCG is used in the code to generate the next value of $x$ where $x_{i+1} = (ax_{i} + c) \mod m$. When connecting to the server, we are given the modulo $m$, and we need to find the values of $a$ and $c$ (not the whole flag or $x$). Since we are given 3 chances to get the values of $x$, we can just spend all 3 chance to obtain $x_1, x_2, x_3$. We can observe the following expressions:<br/>

- $x_1 \equiv (ax_0 + c) \pmod{m}$
- $x_2 \equiv (ax_1 + c) \pmod{m}$
- $x_3 \equiv (ax_2 + c) \pmod{m}$

We can eliminate $c$ from the system by considering $x_3 - x_2 \equiv a(x_2 - x_1) \pmod{m}$. From here we can obtain $a$ as $a \equiv (x_3 - x_2) * (x_2 - x_1)^{-1} \pmod {m}$. Then we can use the 2nd or 3rd equation to find $c$ by $c \equiv (x_2 - ax_1) \pmod{m}$.

## Flag
```
blahaj{thr33_eqns_thr33_var1ables_supER_cr4zy!}
```
