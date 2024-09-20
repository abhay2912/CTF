# PyJail Challenge - "Filtered" Write-Up

## Challenge Description

In this challenge, we are tasked with reading the content of a file called `flag.txt` using a Python script with some constraints. The challenge is designed to simulate a restricted environment ("Python Jail") where we need to bypass input restrictions.

- **File to read**: `flag.txt`
- **Input Constraints**: 
  - Input length cannot exceed 14 characters (`M = 14`).
  - Input must consist of ASCII characters (`ord(c) < 128`).
  - Restricted terms: `exec`, `eval`, `breakpoint`, `help`, `license`, `exit`, `quit`.

### Provided Code (`main.py`)

```python
M = 14  # no malicious code could ever be executed since this limit is so low, right?
def f(code):
    assert len(code) <= M
    print(len(code))
    assert all(ord(c) < 128 for c in code)
    assert all(q not in code for q in ["exec", "eval", "breakpoint", "help", "license", "exit", "quit"])
    exec(code, globals())
f(input("> "))
```

## Objective
Our goal is to execute a command that reads the contents of `flag.txt`, while adhering to the constraints mentioned above.

## Approach
### Understanding the Restrictions
1. **Length Restriction**: The length of the input should not exceed 14 characters. This is defined by `M = 14`.
2. **ASCII Check**: Only characters with an ASCII value less than 128 are allowed.
3. **Blocked Keywords**: The code blocks specific terms like `exec`, `eval`, `breakpoint`, etc., which are generally used to execute commands or inspect code.

The challenge here is to find a way to execute code without using the blocked terms while respecting the input length constraint.

## Solution
### Step 1: Bypassing the Input Length Restriction
Initially, the length of the input is restricted to 14 characters, which makes it hard to execute complex commands. However, the function `f(code)` can be called recursively to bypass this length restriction. By resetting the `M` variable to a higher value, we can input a longer payload.

**Payload 1:**
```python
a=input;f(a())
```

### Step 2: Setting a Larger Input Limit
To increase the allowed input length, we can redefine M inside the second call, setting it to a higher value (e.g., 50) so that we can input a longer payload next.

**Payload 1:**
```python
M=50;f(a())
```
### Step 3: Reading the Flag
With the input length restriction lifted, we can now input a payload that reads and prints the contents of flag.txt. The open() function allows

**Payload 3:*
```python
print(open("flag.txt").read())
```

### Full Solution Walkthrough
1. **First Payload**: Redefine `a` as the `input()` function and recursively call `f()` to allow further inputs.
2. **Second Payload**: Increase the allowed input length by redefining `M` to 50.
3. **Third Payload**: Read and print the content of `flag.txt`.

## Final Exploit Code
```python
a=input;f(a())
M=50;f(a())
print(open("flag.txt").read())
```
This series of inputs effectively bypasses the restrictions and prints the flag.

## Notes
- The use of recursive function calls was key in overcoming the input restrictions.
- The challenge shows how security mechanisms can be bypassed if not thoroughly enforced.

## Conclusion
In this challenge, we bypassed the input length restriction by recursively calling the `f()` function and resetting the limit. By breaking the problem into smaller steps and understanding how Python handles variable assignments and input recursion, we managed to read the contents of `flag.txt` without using any of the blocked terms.

This challenge emphasizes the importance of sandboxing and restricting input in environments that allow code execution, as even small loopholes can lead to significant bypasses.

