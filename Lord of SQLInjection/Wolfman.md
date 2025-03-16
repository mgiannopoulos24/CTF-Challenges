# SQL Injection on Wolfman Challenge

## Challenge Analysis

### Code Review:
- The PHP code creates a query to select an ID based on the `id` and `pw` parameters.
- It attempts to prevent SQL injection by filtering out certain characters in the `pw` parameter using regex.
- Disallowed characters include `prob`, `_`, `.`, and `()` to prevent attacks on other tables and databases.
- Additionally, the code blocks the use of whitespace in the pw parameter.
### Query to Bypass Authentication:
- The SQL query format in the code is:

```sql
select id from prob_wolfman where id='guest' and pw='{$_GET[pw]}'
```
- Our goal is to inject SQL into the `pw` parameter to make the condition always true and gain access as the `admin` user.
### Bypass Restrictions:
- The code blocks the use of whitespace, but we can use URL encoding or other techniques to bypass this restriction.
- Since single quotes (`'`) are not restricted, we can use them to manipulate the SQL query.

## Solution Steps
### Formulate the Injection:
- To bypass the login check, we need to manipulate the `pw` parameter to make the SQL condition always true.
- We can inject a condition like `'or'1'='1` into the pw parameter. This will make the query return true regardless of the actual password.
### Execution:
- The injection payload in the URL will be:
```console
https://los.rubiya.kr/chall/wolfman_4fdc56b75971e41981e3d1e2fbe9b7f7.php?pw=%27or%271%27=%271
```
- Here, `%27` is the URL-encoded representation of a single quote (`'`), and `%3D` is the URL-encoded representation of the equals sign (`=`).

### Expected Outcome:
- If successful, the application will log us in as the `admin` user, triggering the `solve("wolfman")` function to mark the challenge as solved.

## Conclusion

By injecting the payload `'or'1'='1` into the `pw` parameter, we were able to bypass the login check and gain access as the `admin` user. This classic SQL injection technique exploits the fact that the application does not properly sanitize user input, allowing us to manipulate the SQL query to our advantage.

### Final URL:
```console
https://los.rubiya.kr/chall/wolfman_4fdc56b75971e41981e3d1e2fbe9b7f7.php?pw=%27or%271%27=%271
```