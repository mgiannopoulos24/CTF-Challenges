<div style="display: flex; justify-content: center; align-items: center; width: 100%;">

![img](https://i.imgur.com/Xw2DVAT.png)

</div>

<div style="display: flex; justify-content: center; align-items: center; width: 100%;">

  <!-- Column 1: Image -->
  <div style="width: 50%; text-align: center;">
    <img src='https://i.imgur.com/heKOInX.png' style='zoom: 80%;' />
  </div>

  <!-- Column 2: Title and Details -->
  <div style="width: 50%; text-align: center;">
    <h1 style="font-size: 2.5em;">brevi moduli</h1>
    <p>22<sup>nd</sup> April 2025</p>
    <p>Prepared By: <a href="https://app.hackthebox.com/users/2024290">deathwish24</a></p>
    <p>Challenge Author(s): <a href="https://app.hackthebox.com/users/37925">r4sti</a></p>
    Difficulty: <p style="color: green; font-weight: bold;">Very Easy</p>
  </div>

</div>
<br><br><br><br><br><br>

# Synopsis

- The challenge provides an RSA public key and asks for its prime factors in several rounds. The user must factorize the modulus for each round and submit the correct primes to obtain the flag.

## Description

- On a cold Halloween night, five adventurers gathered at the entrance of an ancient crypt. The Cryptkeeper appeared from the shadows, his voice a chilling whisper: "Five locks guard the treasure inside. Crack them, and the crypt is yours." One by one, they unlocked the crypt's secrets, but as the final door creaked open, the Cryptkeeper's eerie laughter filled the air. "Beware, for not all who enter leave unchanged."

## Skills Required

- Python
- SageMath (for integer factorization)
- Understanding of RSA cryptography
- Familiarity with pwntools for remote connections
- Basic regex and PEM parsing

## Skills Learned

- How to extract and parse PEM-encoded RSA keys
- How to use SageMath for integer factorization
- Automating CTF crypto challenges with Python and pwntools

# Enumeration

## Analyzing the source code

- The provided file is `server.py`.
- It simulates the challenge server, generating RSA public keys with small prime factors.
- The script uses `pycryptodome` for RSA key generation and PEM encoding.
- It expects the user to input the two prime factors of the modulus.
- The script checks if the provided inputs are prime and if their product equals the modulus.
- The script uses `pwntools` for remote interaction, `pycryptodome` for RSA key parsing, and `sage.all.factor` for factorization.

The basic workflow of the script is as follows:

1.  For each round, generate two 110-bit prime numbers (`pumpkin1` and `pumpkin2`).
2.  Calculate the modulus `n = pumpkin1 * pumpkin2`.
3.  Construct an RSA public key from `n` and `e = 65537`, then export it in PEM format.
4.  Print the PEM-encoded public key to the console.
5.  Prompt the user to enter the two prime factors.
6.  Verify that the entered values are prime and that their product equals `n`.
7.  Repeat steps 1-6 for a set number of rounds.
8.  After all rounds, print the flag.

A little summary of all the interesting things we have found out so far:

1. The modulus provided in each round is weak enough to be factored quickly.
2. The server expects the two prime factors in a specific order.
3. The challenge is fully automatable with the right libraries.

# Solution 

## Finding the vulnerability

The vulnerability is that the RSA modulus provided by the server is too small or otherwise weak, making it trivial to factor using modern tools like SageMath. This defeats the security of RSA, as knowing the prime factors allows you to break the encryption.

## Exploitation

### Connecting to the server

Here is the code used to connect to the server and automate the solution:

```python
def solve_challenge(address, port, rounds=5):
    """
    Connects to the challenge server, extracts the modulus, factorizes it,
    and sends the prime factors back to the server.
    """
    print(f"Connecting to {address}:{port}...")
    conn = remote(address, port)

    for i in range(rounds):
        print(conn.recvuntil(b'Round').decode())
        print(f"**** Round {i + 1}/{rounds} ****")

        # Receive and parse the RSA public key
        conn.recvuntil(b"?\n")
        pem_key = conn.recvuntil(b"enter your first pumpkin = ").decode().strip()
        n = extract_modulus(pem_key)
        print(f"Extracted modulus: {n}")

        # Factorize the modulus using SageMath
        p, q = factorize(n)
        print(f"Found primes: p = {p}, q = {q}")

        # Send the first prime factor when prompted for "enter your first pumpkin = "
        conn.sendline(str(p).encode())

        # Receive the prompt for the second pumpkin
        conn.recvuntil(b"enter your second pumpkin = ")

        # Send the second prime factor when prompted for "enter your second pumpkin = "
        conn.sendline(str(q).encode())

        print("Primes submitted. Moving to next round...\n")

    # Receive and print the flag
    flag = conn.recvall().decode()
    print("Flag:", flag)
    conn.close()

# Change this
if __name__ == "__main__":
    solve_challenge("83.136.255.197", 34818) 
```

Let us consider our attack scenario.

1. Receive the PEM-encoded public key from the server.
2. Extract the modulus using regex and base64 decoding.
3. Factorize the modulus using SageMath's `factor` function.
4. Send the two prime factors as required by the server.
5. Repeat for all rounds to get the flag.

### Getting the flag

A final summary of all that was said above:

1. The server sends a weak RSA modulus in each round.
2. We extract and factorize the modulus, then send the primes.
3. After all rounds, the server sends the flag.
