# SQL Injection on Gremlin Challenge

## Challenge Analysis
1. **Code Review:**
- The PHP code creates a query to select an ID based on parameters `id` and `pw`.
- It attempts to prevent certain types of SQL injection by filtering out certain characters in both `id` and `pw` using regex.
- Disallowed characters include `prob`, `_`, `.`, and `()` to prevent attacks on other tables and databases.
2. **Query to Bypass Authentication:**
- The SQL query format in the code is: `select id from prob_gremlin where id='{$_GET[id]}' and pw='{$_GET[pw]}'`
- Our goal is to inject SQL into the `id` or `pw` parameter to make the condition always true and gain access.
3. **Bypass Restrictions:**
- Although the code blocks certain symbols, classic SQL techniques like comments (`#` or `--`) aren’t restricted.
- Since there’s no restriction on single quotes (`'`), we can inject SQL statements to manipulate the query.
## Solution Steps
1. **Formulate the Injection:**
- To bypass, let’s manipulate the `id` field by making the condition always true.
- We can inject a condition that makes the SQL statement true regardless of the password by setting the `pw` parameter to `password' OR '1'='1`.
2. **Execution:**
- The injection payload in the URL will be:
```bash
https://los.rubiya.kr/chall/gremlin_280c5552de8b681110e9287421b834fd.php?id=admin&pw=password' OR '1'='1
```
- This payload exploits the `pw` condition by ending the password with a closing single quote and appending `OR '1'='1`, making the condition always true.
3. **Explanation of Injection:**
- The injected SQL becomes:
```sql
select id from prob_gremlin where id='admin' and pw='password' OR '1'='1'
```
- Since `'1'='1'` is always true, the query returns results if the `id` exists, bypassing the need to match a correct password.
4. **Expected Outcome:**
- If successful, the application logs us in as `admin`, triggering the `solve("gremlin")` function to mark the challenge as solved.