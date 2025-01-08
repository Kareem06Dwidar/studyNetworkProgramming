# **Complete Code Explanation (WSGI Server Using Raw WSGI Specification)**

```python
#!/usr/bin/env python3
# Foundations of Python Network Programming, Third Edition
# https://github.com/brandon-rhodes/fopnp/blob/m/py3/chapter10/timeapp_raw.py
# A simple HTTP service built directly against the low-level WSGI spec.

import time

def app(environ, start_response):
    host = environ.get('HTTP_HOST', '127.0.0.1')
    path = environ.get('PATH_INFO', '/')
    if ':' in host:
        host, port = host.split(':', 1)
    if '?' in path:
        path, query = path.split('?', 1)
    headers = [('Content-Type', 'text/plain; charset=utf-8')]
    if environ['REQUEST_METHOD'] != 'GET':
        start_response('501 Not Implemented', headers)
        yield b'501 Not Implemented'
    elif host != '127.0.0.1' or path != '/':
        start_response('404 Not Found', headers)
        yield b'404 Not Found'
    else:
        start_response('200 OK', headers)
        yield time.ctime().encode('ascii')
```

---

## **What is WSGI?**
- **WSGI (Web Server Gateway Interface)** is a Python standard for web servers and web applications to communicate.
- It defines how a **web server** forwards HTTP requests to a **Python application** and how the application returns the response.

---

## **Key Concepts Before Breaking Down the Code**
- **WSGI Components:**
   - **`environ`**: A dictionary containing the incoming request data.
   - **`start_response`**: A callback function to send the HTTP status and headers back to the client.
   - **`yield`**: Used to send the response body back in chunks (generators).
- **Purpose of This Code:** 
   - A simple HTTP server that:
     - Only responds to **GET** requests.
     - Only works for `localhost` (`127.0.0.1`).
     - Returns the **current date and time** if conditions are met.

---

# **Step-by-Step Code Breakdown**

---

## **1. Shebang and Comments**
```python
#!/usr/bin/env python3
# A simple HTTP service built directly against the low-level WSGI spec.
```
- **Shebang Line (`#!/usr/bin/env python3`)**:
   - Specifies the Python interpreter for running the script (`python3`).
- **Comments**: Provide information about the script's source and purpose.

---

## **2. Importing Required Modules**
```python
import time
```
- **`time` module**: Provides the `time.ctime()` function, which returns the current time as a readable string.

---

## **3. Defining the WSGI Application**
```python
def app(environ, start_response):
```
- **`def app()`**: The **WSGI application function**.
- **`environ`**: Contains the incoming HTTP request data.
- **`start_response`**: A callback function to send the **status code** and **headers** back to the client.

---

## **4. Retrieving Host and Path Information**
```python
host = environ.get('HTTP_HOST', '127.0.0.1')
path = environ.get('PATH_INFO', '/')
```
- **`environ.get()`**: Fetches request data from the `environ` dictionary.
- **`HTTP_HOST`**: Represents the requested hostname.
- **`PATH_INFO`**: Represents the requested URL path (default is `/`).

---

## **5. Splitting Host and Port**
```python
if ':' in host:
    host, port = host.split(':', 1)
```
- **`if ':' in host`**:
   - Splits the **host** and **port** if a port number is included in the host.
   - Example: `127.0.0.1:8000` splits into `host = 127.0.0.1` and `port = 8000`.

---

## **6. Handling URL Query Parameters**
```python
if '?' in path:
    path, query = path.split('?', 1)
```
- **`if '?' in path:`**:
   - Splits the URL path from the **query string** if it exists.
   - Example: `/index.html?user=test` → `path = /index.html`, `query = user=test`.

---

## **7. Setting the Response Headers**
```python
headers = [('Content-Type', 'text/plain; charset=utf-8')]
```
- **Headers**:
   - Specifies the response as **plain text** (`text/plain`) with **UTF-8 encoding**.

---

## **8. Handling Unsupported HTTP Methods**
```python
if environ['REQUEST_METHOD'] != 'GET':
    start_response('501 Not Implemented', headers)
    yield b'501 Not Implemented'
```
- **`if environ['REQUEST_METHOD'] != 'GET'`:**
   - If the request method **is not `GET`**, the server returns:
     - **501 Not Implemented**
     - Body: **"501 Not Implemented"**
     - **Yield Statement**: Sends a byte response (`b'501...'`).

---

## **9. Handling Incorrect Host and Path**
```python
elif host != '127.0.0.1' or path != '/':
    start_response('404 Not Found', headers)
    yield b'404 Not Found'
```
- **Condition**:
   - If the **host** is not `localhost` (`127.0.0.1`) or the **path** is not `/`.
- **Returns**:
   - **404 Not Found** error.

---

## **10. Sending the Current Time (Success Case)**
```python
else:
    start_response('200 OK', headers)
    yield time.ctime().encode('ascii')
```
- **If all conditions are satisfied**:
   - Responds with **200 OK**.
   - Sends the **current date and time** as a response body using `time.ctime()`.

---

# **How to Run This Server?**
1. **Run the server using Python:**
   ```bash
   python3 timeapp_raw.py
   ```
2. **Open a web browser:**
   - Go to `http://127.0.0.1:8000`.
3. **Expected Output:**
   - Current time displayed on the browser.
4. **Testing Invalid Cases:**
   - `http://127.0.0.1:8000/invalid` → Shows **404 Not Found**.
   - Sending a `POST` request → Shows **501 Not Implemented**.

---

# **Key Networking Concepts Related to the Code:**
### ✅ **1. What is WSGI?**
- **WSGI (Web Server Gateway Interface)** is a standard for communication between **Python web servers** and **web applications**.

### ✅ **2. HTTP Request Methods:**
- **GET**: Fetches data (used in this code).
- **POST**: Sends data to the server.
- **PUT/DELETE**: Modify or delete data.

### ✅ **3. HTTP Status Codes Explained:**
- **200 OK**: Request succeeded.
- **404 Not Found**: Resource not available.
- **501 Not Implemented**: Server doesn't support the method used.

---

# **Potential Exam Questions and Answers**

### 📌 **1. What is the Role of `environ` in WSGI?**
**Answer:** 
- `environ` is a dictionary containing **all request data**, including headers, method, path, and query string.

---

### 📌 **2. Why Does the Server Return `501 Not Implemented`?**
**Answer:**
- It returns **501** when the request method is not **GET** since the server is not configured to handle other methods.

---

### 📌 **3. What is the Purpose of `start_response()`?**
**Answer:**
- **`start_response()`** initiates the response by sending the **status code** and **headers** back to the client.

---

### 📌 **4. How Does the Server Handle Query Strings?**
**Answer:**
- The code checks for a `?` in the path and separates the **path** from the **query string** using `.split()`.

---

### 📌 **5. How Does the Server Send Data Back to the Client?**
**Answer:**
- The server sends data using the `yield` statement with **byte-encoded** data (`b'...').

---

# ✅ **Summary of Key Takeaways:**
- **WSGI** defines how servers communicate with Python apps.
- The code handles **GET** requests only.
- **WebOb** simplifies handling HTTP requests and responses.
- **Key Status Codes Used:** `200`, `404`, `501`.
- **Key Concepts:** WSGI, request parsing, status codes, and headers.

---

