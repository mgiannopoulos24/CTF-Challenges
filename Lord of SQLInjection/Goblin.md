# SQL Injection on Goblin Challenge
## Challenge Analysis
### Code Review
1. **PHP Code Analysis:**
- The provided PHP code connects to a database and attempts to authenticate based on an `id` and a parameter `no`.
- The code includes checks to prevent certain SQL injection characters, specifically:
    It blocks keywords like `prob`, `_`, `.`, and `()` by regex matching in the `no` parameter.
    It also prohibits quotes (`'`, `"`, and `) to prevent classic injection techniques.
2. **SQL Query Structure:**
- The SQL query generated is:
```sql
select id from prob_goblin where id='guest' and no={$_GET[no]}
```
- The goal here is to bypass the filters and inject SQL into the `no` parameter to make the query true for specific conditions, aiming to eventually retrieve an `id` of `admin`.
3. **Objective:**
- Our goal is to inject SQL into the `no` parameter to manipulate the condition. Specifically, we aim to retrieve an `id` matching `admin` to solve the challenge by bypassing the restrictions in place.
### Bypass Strategy
1. **ASCII-Based Injection:**
- The filters block various characters but allow logical operators (`||`) and numeric operations.
- By using `ASCII(SUBSTRING(id,1,1))`, we can check if the first character of `id` matches a specific ASCII code. For example, `97` corresponds to the lowercase letter `'a'`, which helps us test whether the `id` starts with `'a'`.
2. **Formulating the Injection Payload:**
- We can craft a payload that will bypass the restrictions and perform a blind SQL injection to evaluate character-by-character:
```sql
0||ASCII(SUBSTRING(id,1,1))=97
```
- This payload checks if the ASCII value of the first character of `id` is `97`, effectively selecting for an `id` that starts with `'a'`.

## Solution Steps
1. **Formulate the URL Injection:**
- To test if the first character of `id` is `'a'`, the URL injection becomes:
```bash
https://los.rubiya.kr/chall/goblin_e5afb87a6716708e3af46a849517afdc.php?no=0||ASCII(SUBSTRING(id,1,1))=97
```
- This URL bypasses the usual numeric restriction by leveraging the `||` operator to evaluate a true condition if the first character of `id` is `'a'`.
2. **Injection Mechanism Explanation:**
- This injects `0||ASCII(SUBSTRING(id,1,1))=97`, making the `no` condition evaluate to `true` when `id` starts with `'a'`.
- This SQL injection technique allows us to test each character of the `id` sequentially using ASCII values until we confirm a match with `admin`.
3. **Expected Outcome:**
- When the `id` resolves to `'admin'`, the code calls `solve("goblin")`, which clears the challenge.