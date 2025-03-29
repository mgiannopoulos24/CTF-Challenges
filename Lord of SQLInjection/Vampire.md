# SQL Injection on Vampire Challenge
## Challenge Analysis
1. **Code Review:**
    - The PHP code creates a query to select an ID from the `prob_vampire` table.
    - There are multiple filtering mechanisms in place:
        - Single quotes are blocked using `preg_match('/\'/i', $_GET[id])`
        - Input is converted to lowercase with `strtolower($_GET[id])`
        - The string "admin" is removed from the input with `str_replace("admin","",$_GET[id])`
    - The goal is to make `$result['id'] == 'admin'` true to solve the challenge.
2. **Vulnerability Identification:**
    - The key vulnerability is in the string replacement function.
    - When the code removes "admin" from the input string, we can abuse this by nesting the target string.
    - This type of filter bypass allows us to create an input that, after processing, produces our desired payload.
3. **Attack Vector:**
    - The SQL query format is: `select id from prob_vampire where id='{$_GET[id]}'`
    - Since we can't use single quotes, we need to focus on bypassing the string replacement.

## Solution Steps
1. **Formulate the Bypass:**
    - We need an input string that, after "admin" is removed, still contains "admin".
    - By using nested occurrence of "admin", we can achieve this.
    - If we input "adadminmin", after the filter removes "admin", we'll be left with "admin".

2. **Execution:**
    - The bypass payload in the URL will be:
    ```console
    https://los.rubiya.kr/chall/vampire_e3f1ef853da067db37f342f3a1881156.php?id=adadminmin
    ```

3. **Explanation of Bypass:**
    - Original input: `adadminmin`
    - After lowercase conversion: `adadminmin` (no change since already lowercase)
    - After string replacement: admin (the middle "admin" is removed)
    - The resulting SQL query becomes:

    ```sql
    select id from prob_vampire where id='admin'
    ```

4. **Expected Outcome:**

The query will match the 'admin' record in the database.
This triggers the `solve("vampire")` function, completing the challenge.

## Key Concepts
- This challenge demonstrates a common security flaw in blacklist filtering.
- When implementing string replacement for security, multiple passes or recursive replacements may be necessary to prevent bypass techniques.
- This type of attack is sometimes referred to as a "filter bypass" or "filter evasion" technique.