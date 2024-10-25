# SQL Injection on Cobolt Challenge
## Challenge Analysis
1. **Code Review:**
- The PHP code constructs an SQL query that selects an ID from the prob_cobolt table based on the parameters id and pw.
- The code applies a regex filter on both id and pw inputs to prevent certain SQL injection techniques. Specifically, it disallows prob, _, ., and (), attempting to restrict access to other tables and SQL functions.
- The SQL query format used in the code is:
```sql
select id from prob_cobolt where id='{$_GET[id]}' and pw=md5('{$_GET[pw]}')
```
2. **Query to Bypass Authentication:**
- Our goal is to modify the query through SQL injection to bypass the password check and gain access as `admin`.
- The code does not filter out all SQL techniques, specifically, comments (`--`) and single quotes (`'`), allowing us to manipulate the query structure.
3. **Bypass Restrictions:**
- The input validation does not block the usage of `--` for comments, which we can use to bypass the password condition.
- Since the code accepts single quotes, we can exploit this by injecting a UNION-based payload to mimic a successful `admin` login.

## Solution Steps
1. **Formulate the Injection:**
- To gain `admin` access, we can inject into the `id` parameter and use a `UNION SELECT` statement to directly return the value `'admin'`.
- We can terminate the `id` parameter with `--` to ignore the `pw` condition entirely.
2. **Execution:**
- The payload for this injection will be:
```bash
https://los.rubiya.kr/chall/cobolt_b876ab5595253427d3bc34f1cd8f30db.php?id=' UNION SELECT 'admin' -- ' &pw=anything
```

- Here’s the breakdown:
    - `id=' UNION SELECT 'admin' -- `: This injects `' UNION SELECT 'admin'` into the query, making it return `'admin'` as the result.
    - `--`: This is a comment indicator in SQL, which ignores everything after it, bypassing the password condition entirely.
3. **Explanation of Injection:**
- The injected SQL query becomes:
```sql
select id from prob_cobolt where id='' UNION SELECT 'admin' -- ' and pw=md5('anything')
```
- This query is manipulated so that it returns `admin` as the `id`, effectively bypassing the need for a valid password.

4. **Expected Outcome:**
- By successfully injecting this payload, the application identifies us as `admin`, triggering the `solve("cobolt")` function and marking the challenge as solved.