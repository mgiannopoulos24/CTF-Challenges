![img](https://i.imgur.com/Xw2DVAT.png)

<img src='https://i.imgur.com/heKOInX.png' style='zoom: 80%;' align=left /> <font size='10'>SugarFreeCandies</font>

22<sup>nd</sup> April 2025

Prepared By: [deathwish24](https://app.hackthebox.com/users/2024290)

Challenge Author(s): [0x50r4](https://app.hackthebox.com/users/1308105)

Difficulty: <font color='green'>Very Easy</font>

<br><br>

# Synopsis

- The challenge involves reverse-engineering a Python script that splits the flag into three parts, performs calculations on them, and outputs the results. The goal is to recover the original flag given the output values.

## Description

- The provided Python script takes the flag, divides it into three chunks (`cnd1`, `cnd2`, `cnd3`), and computes four values (`v1`, `v2`, `v3`, `v4`) based on these chunks. The computed values are given in `output.txt`. We need to solve the system of equations to retrieve the original flag.

## Skills Required

- Python
- Cryptography
- Algebra (solving systems of equations)
- Using `sympy` library

## Skills Learned

- Solving a system of non-linear equations using `sympy`.
- Converting between long integers and bytes.
- Reverse-engineering cryptographic calculations.

# Enumeration

## Analyzing the source code

- We are provided with a Python script and an `output.txt` file. The script calculates `v1`, `v2`, `v3`, and `v4` based on `cnd1`, `cnd2`, and `cnd3`, which are derived from the flag. The `output.txt` file contains the values of `v1`, `v2`, `v3`, and `v4`.

If we look at the provided files, we can see that our goal is:

- Recover the original flag by reverse-engineering the calculations performed in the Python script.

The basic workflow of the challenge is as follows:

1.  The flag is split into three parts: `cnd1`, `cnd2`, and `cnd3`.
2.  Four values `v1`, `v2`, `v3`, and `v4` are calculated based on these parts.
3.  The values `v1`, `v2`, `v3`, and `v4` are provided in `output.txt`.
4.  We need to solve the system of equations to find `cnd1`, `cnd2`, and `cnd3` and reconstruct the flag.

A little summary of all the interesting things we have found out so far:

1.  The flag is split into three parts.
2.  The relationship between `cnd1`, `cnd2`, `cnd3` and `v1`, `v2`, `v3`, `v4` is defined by a system of equations.
3.  We can use `sympy` to solve this system of equations.

# Solution

## Finding the vulnerability

The "vulnerability" here is the mathematical relationship that allows us to reverse the process. The equations:

- $v1 = cnd1^3 + cnd3^2 + cnd2$
- $v2 = cnd2^3 + cnd1^2 + cnd3$
- $v3 = cnd3^3 + cnd2^2 + cnd1$
- $v4 = cnd1 + cnd2 + cnd3$

form a system of non-linear equations. Given `v1`, `v2`, `v3`, and `v4`, we can solve for `cnd1`, `cnd2`, and `cnd3` using algebraic methods (in this case, `sympy`).

## Exploitation

### Connecting to the server

This is a local challenge, so no connection is required.

### Getting the flag

A final summary of all that was said above:

1.  We have the values `v1`, `v2`, `v3`, and `v4` from `output.txt`.
2.  We have the equations relating `v1`, `v2`, `v3`, `v4` to `cnd1`, `cnd2`, and `cnd3`.
3.  We use `sympy` to solve the system of equations for `cnd1`, `cnd2`, and `cnd3`.
4.  We convert `cnd1`, `cnd2`, and `cnd3` back to bytes and concatenate them to get the flag.

This recap can be represented by code using `pwn()` function:

```python
def pwn():
    from sympy import symbols, solve
    from Crypto.Util.number import long_to_bytes

    # Given values from output.txt
    v1 = 4196604293528562019178729176959696479940189487937638820300425092623669070870963842968690664766177268414970591786532318240478088400508536
    v2 = 11553755018372917030893247277947844502733193007054515695939193023629350385471097895533448484666684220755712537476486600303519342608532236
    v3 = 14943875659428467087081841480998474044007665197104764079769879270204055794811591927815227928936527971132575961879124968229204795457570030
    v4 = 6336816260107995932250378492551290960420748628

    # Define symbols
    cnd1, cnd2, cnd3 = symbols('cnd1 cnd2 cnd3')

    # Solve the system of equations
    solutions = solve([
        cnd1**3 + cnd3**2 + cnd2 - v1,
        cnd2**3 + cnd1**2 + cnd3 - v2,
        cnd3**3 + cnd2**2 + cnd1 - v3,
        cnd1 + cnd2 + cnd3 - v4
    ], (cnd1, cnd2, cnd3))

    # Extract the solution (there may be multiple solutions, we choose one)
    cnd1_val, cnd2_val, cnd3_val = solutions[0]

    # Convert long integers back to bytes
    flag_part1 = long_to_bytes(cnd1_val)
    flag_part2 = long_to_bytes(cnd2_val)
    flag_part3 = long_to_bytes(cnd3_val)

    # Reconstruct the flag
    flag = flag_part1 + flag_part2 + flag_part3
    print(flag.decode())

if __name__ == "__main__":
    pwn()
```