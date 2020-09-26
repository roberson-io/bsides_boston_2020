# Maelstrom

This is a crypto problem that provides a `decrypt.py` file. If you try running it, it prints part of the flag and hangs.

There is a function `x` that is just checking whether or not a value is prime:

```python
def x(num):
    if num > 1:
        for i in range(2, num):
            if (num % i) == 0:
                return False
                break
        return True
    else:
        return False
```

You can write a better prime checker yourself, but I just used the `isprime` function in `sympy` and kept track of primes that have already been checked:

```bash
python3 -m venv venv
. venv/bin/activate
pip install sympy
```

```python
from sympy.ntheory import isprime

primes = set()

def x(num):
    if num in primes:
        return True
    else:
        n_prime = isprime(num)
        if n_prime:
            primes.add(num)
        return n_prime
```

I included my edited `decrypt.py` file.

Flag: `flag{more_primes_more_good}`