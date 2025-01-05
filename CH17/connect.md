
This script demonstrates how to establish a **basic FTP connection** using Python's `ftplib`. It connects to a public FTP server, prints a welcome message, displays the current working directory, and then closes the connection.

---

---

## **Imports and Setup:**
```python
from ftplib import FTP
```

### **Explanation:**
- **`ftplib`**: The standard Python library for handling **FTP (File Transfer Protocol)** connections and commands.

---

### ✅ **Potential Questions:**
1. **What is the purpose of the `ftplib` module?**  
   - The `ftplib` module provides support for **FTP operations** such as connecting to servers, transferring files, and managing directories.

2. **What type of connections can `ftplib` handle?**  
   - Standard **FTP connections** only (for secure FTP, `paramiko` or `ftplib.FTP_TLS` is required).

---

---

## **Main Function: Establishing Connection**
```python
def main():
    ftp = FTP('ftp.ibiblio.org')
    print("Welcome:", ftp.getwelcome())
```

### **Explanation:**
1. **Create an FTP Connection:**
   ```python
   ftp = FTP('ftp.ibiblio.org')
   ```
   - Connects to the FTP server at **`ftp.ibiblio.org`**.

2. **Get the Welcome Message:**
   ```python
   print("Welcome:", ftp.getwelcome())
   ```
   - **`ftp.getwelcome()`**: Retrieves the **welcome message** from the FTP server upon connection.

---

### ✅ **Potential Questions:**
1. **What does the `ftp.getwelcome()` method do?**  
   - It returns the **welcome message** sent by the server upon connection.  

2. **Why would you use the `getwelcome()` method?**  
   - To verify a successful connection and obtain server information.

---

---

## **Logging in to the FTP Server:**
```python
    ftp.login()
```

### **Explanation:**
- **Anonymous FTP Login:**
   ```python
   ftp.login()
   ```
   - **No credentials provided**: Logs in using **anonymous FTP**.  
   - On public servers, anonymous login allows access without a username or password.  

---

### ✅ **Potential Questions:**
1. **What happens if no arguments are passed to `ftp.login()`?**  
   - It defaults to **anonymous login**.

2. **How would you log in with a username and password instead?**  
   ```python
   ftp.login(user="username", passwd="password")
   ```

3. **What is the difference between anonymous FTP and credential-based FTP?**  
   - **Anonymous FTP** allows public access without authentication.  
   - **Credential-based FTP** requires a **username** and **password** for access.

---

---

## **Displaying the Current Working Directory:**
```python
    print("Current working directory:", ftp.pwd())
```

### **Explanation:**
- **Print Working Directory (PWD):**
   ```python
   ftp.pwd()
   ```
   - **`ftp.pwd()`** retrieves the **current working directory** on the remote server.

---

### ✅ **Potential Questions:**
1. **What does `ftp.pwd()` do?**  
   - It displays the **current working directory** on the remote server.  

2. **What is the FTP command equivalent to `pwd()`?**  
   - The **FTP command** equivalent is `PWD`.  

---

---

## **Closing the Connection:**
```python
    ftp.quit()
```

### **Explanation:**
- **Gracefully Close the Connection:**
   ```python
   ftp.quit()
   ```
   - Sends the `QUIT` command to the server and **terminates the FTP session**.

---

### ✅ **Potential Questions:**
1. **What is the difference between `ftp.quit()` and `ftp.close()`?**  
   - **`ftp.quit()`** sends the `QUIT` command and closes the connection gracefully.  
   - **`ftp.close()`** simply terminates the socket without notifying the server.  

2. **Why is it important to close the FTP connection?**  
   - To prevent leaving **hanging sessions** on the server and free up resources.

---

---

## **Main Execution Block:**
```python
if __name__ == '__main__':
    main()
```

### **Explanation:**
- **Standard Python Entry Point:**
   - Ensures the `main()` function executes **only when the script is run directly**, not when imported as a module.

---

### ✅ **Potential Questions:**
1. **What is the purpose of the `if __name__ == '__main__':` block?**  
   - It ensures the script runs only when executed directly, not when imported.  

2. **What would happen if this block were removed?**  
   - The script would still run, but it would execute automatically when imported as a module.

---

---

# ✅ **Summary of Key Concepts:**
| **Concept**                     | **Explanation**                                  |
|---------------------------------|--------------------------------------------------|
| **FTP Protocol**                | A standard protocol for transferring files over a network. |
| **Anonymous Login**             | Allows users to log in without a password on public servers. |
| **FTP Commands Used**           | `PWD` (Print Working Directory), `QUIT` (Close Connection). |
| **Connection Handling**         | `ftp.login()`, `ftp.pwd()`, `ftp.quit()`          |
| **Standard FTP Methods**        | Basic FTP operations without encryption.         |
| **Difference Between `quit()` and `close()`** | `quit()` formally terminates the session, `close()` just ends the socket. |

---

---

# ✅ **Exam-Style Questions:**

### **True/False Questions:**
1. **True or False:** The script uses the `ftplib` module to create an FTP connection.  
   - **True**  

2. **True or False:** The script uses secure FTP (SFTP) for the connection.  
   - **False** (It uses standard FTP).

---

### **Multiple Choice Questions (MCQ):**
1. **What does the `ftp.getwelcome()` method do?**  
   - A) Uploads a file to the server.  
   - B) Prints the current working directory.  
   - C) Retrieves the server's welcome message.  
   - **Answer:** C) Retrieves the server's welcome message.  

2. **What is the purpose of the `ftp.pwd()` method?**  
   - A) Prints the working directory on the local machine.  
   - B) Prints the working directory on the remote server.  
   - C) Changes the working directory on the server.  
   - **Answer:** B) Prints the working directory on the remote server.  

---

### **Short Answer Questions:**
1. **How can you modify the script to use a username and password instead of anonymous login?**  
   ```python
   ftp.login(user="username", passwd="password")
   ```  

2. **Why should you use `ftp.quit()` instead of `ftp.close()`?**  
   - `ftp.quit()` properly closes the FTP session by sending the `QUIT` command to the server, whereas `ftp.close()` just closes the socket connection without server notification.

---

---

