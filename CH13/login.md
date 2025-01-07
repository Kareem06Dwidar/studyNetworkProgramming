# **Python SMTP Email Sender with Authentication (`login.py`) - Explained with Exam Questions**
---

This script demonstrates how to send an **authenticated email** using Python's `smtplib`. It prompts the user for **credentials**, sends a plain-text message, and handles basic error checking for SMTP communication.

---

---

## **Imports and Setup:**
```python
import sys, smtplib, socket
from getpass import getpass
```

### **Explanation:**
- **`sys`**: Manages **command-line arguments** and error handling.  
- **`smtplib`**: Provides SMTP support for sending emails.  
- **`socket`**: Handles low-level **network errors**.  
- **`getpass`**: Allows **secure password input** without echoing characters to the terminal.  

---

### ✅ **Potential Questions:**
1. **Why use `getpass` instead of `input()` for passwords?**  
   - To prevent the password from being displayed on the screen.  

2. **What is the purpose of `smtplib`?**  
   - It provides a way to send emails using the **SMTP protocol**.  

---

---

## **Email Message Template:**
```python
message_template = """To: {}
From: {}
Subject: Test Message from simple.py

Hello,

This is a test message sent to you from the login.py program
in Foundations of Python Network Programming.
"""
```

### **Explanation:**
- A **multiline string template** for the email body.  
- The `{}` placeholders are filled dynamically using `format()` later in the script.  

---

### ✅ **Potential Questions:**
1. **What is the purpose of the `message_template` in the script?**  
   - To provide a **reusable format** for the email message.  

2. **How are the placeholders `{}` filled in the message?**  
   - Using the `.format()` method in Python.  

---

---

## **Main Function: Parsing Arguments and Input Handling**
```python
def main():
    if len(sys.argv) < 4:
        name = sys.argv[0]
        print("Syntax: {} server fromaddr toaddr [toaddr...]".format(name))
        sys.exit(2)
```

### **Explanation:**
- **Argument Parsing:**
   - The script expects **3 arguments**:  
     - `server`: The SMTP server address.  
     - `fromaddr`: Sender's email address.  
     - `toaddrs`: One or more recipient email addresses.  

- **Error Handling for Missing Arguments:**  
   - If the arguments are missing, the script displays usage information and **exits with code 2**.  

---

### ✅ **Potential Questions:**
1. **Why check the length of `sys.argv`?**  
   - To ensure the user provides the necessary **server, sender, and recipient** arguments.  

2. **What does `sys.exit(2)` indicate?**  
   - It exits the script with an error code **2**, indicating **incorrect usage**.  

---

---

## **Prompting for Credentials and Preparing the Message:**
```python
    server, fromaddr, toaddrs = sys.argv[1], sys.argv[2], sys.argv[3:]
    message = message_template.format(', '.join(toaddrs), fromaddr)

    username = input("Enter username: ")
    password = getpass("Enter password: ")
```

### **Explanation:**
1. **Extracting Arguments:**
   ```python
   server, fromaddr, toaddrs = sys.argv[1], sys.argv[2], sys.argv[3:]
   ```
   - The server, sender, and recipients are extracted from the **command-line arguments**.  

2. **Message Preparation:**
   ```python
   message = message_template.format(', '.join(toaddrs), fromaddr)
   ```
   - The message is formatted using the **template** defined earlier.  

3. **User Credentials Input:**
   ```python
   username = input("Enter username: ")
   password = getpass("Enter password: ")
   ```
   - **`getpass()`** is used to securely prompt for a **password**.  

---

### ✅ **Potential Questions:**
1. **Why use `getpass()` for password entry?**  
   - To prevent the password from being displayed on the screen.  

2. **How does the script handle multiple recipients?**  
   - By using `sys.argv[3:]` and joining the recipients using `', '.join(toaddrs)`  

---

---

## **Connecting to the SMTP Server and Sending the Email:**
```python
    try:
        connection = smtplib.SMTP(server)
        try:
            connection.login(username, password)
        except smtplib.SMTPException as e:
            print("Authentication failed:", e)
            sys.exit(1)
        connection.sendmail(fromaddr, toaddrs, message)
```

### **Explanation:**
1. **Connecting to the SMTP Server:**
   ```python
   connection = smtplib.SMTP(server)
   ```
   - Connects to the specified **SMTP server**.  

2. **Authenticating with the Server:**
   ```python
   connection.login(username, password)
   ```
   - **`login()`** authenticates the user using the provided **credentials**.  

3. **Handling Authentication Errors:**
   ```python
   except smtplib.SMTPException as e:
       print("Authentication failed:", e)
       sys.exit(1)
   ```
   - If authentication fails, the script prints the error and **exits with code 1**.  

4. **Sending the Email:**
   ```python
   connection.sendmail(fromaddr, toaddrs, message)
   ```
   - **`sendmail()`** sends the message using the **provided sender, recipients, and body**.  

---

### ✅ **Potential Questions:**
1. **What does `smtplib.SMTP(server)` do?**  
   - It establishes a connection with the specified **SMTP server**.  

2. **What happens if the login fails?**  
   - The script catches the `SMTPException` and exits with an **error message**.  

3. **Why use `try...except` blocks here?**  
   - To **gracefully handle errors** like authentication failures or network issues.  

---

---

## **Error Handling and Closing the Connection:**
```python
    except (socket.gaierror, socket.error, socket.herror, smtplib.SMTPException) as e:
        print("Your message may not have been sent!")
        print(e)
        sys.exit(1)
    else:
        s = '' if len(toaddrs) == 1 else 's'
        print("Message sent to {} recipient{}".format(len(toaddrs), s))
        connection.quit()
```

### **Explanation:**
- **Network Error Handling:**
   ```python
   except (socket.gaierror, socket.error, socket.herror, smtplib.SMTPException) as e:
   ```
   - Catches various **network-related errors**, including:  
     - `socket.gaierror`: DNS lookup failure.  
     - `socket.error`: General connection issues.  
     - `smtplib.SMTPException`: General SMTP-related issues.  

- **Success Message:**
   ```python
   s = '' if len(toaddrs) == 1 else 's'
   print("Message sent to {} recipient{}".format(len(toaddrs), s))
   ```
   - Confirms successful sending with proper **pluralization**.  

- **Closing the SMTP Connection:**
   ```python
   connection.quit()
   ```
   - **Closes the SMTP session** using the `QUIT` command.  

---

### ✅ **Potential Questions:**
1. **Why is error handling important when sending emails?**  
   - To prevent the script from **crashing** and to **inform** the user of errors.  

2. **Why call `connection.quit()` after sending the email?**  
   - To properly **close the SMTP connection** and free resources.  

---

---

# ✅ **Summary of Key Concepts:**
| **Concept**                  | **Explanation**                                     |
|------------------------------|-----------------------------------------------------|
| **SMTP Protocol**            | Used for sending emails over a network.            |
| **smtplib**                  | Built-in Python module for **SMTP communication**. |
| **Authentication Handling**  | Credentials are securely handled using `getpass()`.|
| **Error Handling**           | The script handles network and **SMTP errors**.    |
| **Binary Output Handling**   | Not required in this case since the email is **plain text**.|

---

---

# ✅ **Exam-Style Questions:**

### **True/False Questions:**
1. **True or False:** The script supports sending **plain text emails only.**  
   - **True**  

2. **True or False:** The script uses `getpass()` for secure password entry.  
   - **True**  

---

### **Multiple Choice Questions (MCQ):**
1. **What does the `smtplib.SMTP()` method do?**  
   - A) Receives emails.  
   - B) Establishes an SMTP connection.  
   - **Answer:** B) Establishes an SMTP connection.  

