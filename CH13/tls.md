# **Python SMTP Email Sender (`simple.py`) - Explained with Exam Questions**
---

This script demonstrates how to send a **basic email** using Python's `smtplib`. It sends an email with a **plain-text body** and basic headers via an **SMTP server**.

---

---

## **Imports and Setup:**
```python
import sys, smtplib
```

### **Explanation:**
- **`sys`**: Used for handling **command-line arguments** and exiting with error codes.  
- **`smtplib`**: Python's **Simple Mail Transfer Protocol (SMTP)** library for sending emails.  

---

### ✅ **Potential Questions:**
1. **What does the `smtplib` module do in Python?**  
   - It provides functions to send emails using the **SMTP protocol**.  

2. **Why use `sys` in this script?**  
   - To handle **command-line arguments** and **exit control**.  

---

---

## **Email Message Template:**
```python
message_template = """To: {}
From: {}
Subject: Test Message from simple.py

Hello,

This is a test message sent to you from the simple.py program
in Foundations of Python Network Programming.
"""
```

### **Explanation:**
- **Multiline String:**  
   - A **template** for the email body is stored using triple quotes (`"""`).  

- **Template Fields:**  
   - `{}` placeholders are used for dynamic **`To:`**, **`From:`**, and the **email body**.

---

### ✅ **Potential Questions:**
1. **Why use a message template in this script?**  
   - To simplify the **formatting** and **reusability** of the email body.  

2. **How does Python insert values into the placeholders (`{}`)?**  
   - Using the **`format()`** method for string interpolation.  

---

---

## **Main Function: Argument Parsing and Email Construction**
```python
def main():
    if len(sys.argv) < 4:
        name = sys.argv[0]
        print("usage: {} server fromaddr toaddr [toaddr...]".format(name))
        sys.exit(2)
```

### **Explanation:**
1. **Argument Handling:**
   ```python
   if len(sys.argv) < 4:
       print("usage: {} server fromaddr toaddr [toaddr...]".format(name))
       sys.exit(2)
   ```
   - The script requires **at least 3 arguments**:  
     - `server`: The SMTP server address.  
     - `fromaddr`: The sender's email address.  
     - `toaddr`: One or more recipient addresses.  

2. **Exiting on Error:**
   ```python
   sys.exit(2)
   ```
   - If the arguments are insufficient, the script prints a **usage message** and exits with **error code 2**.  

---

### ✅ **Potential Questions:**
1. **Why check the length of `sys.argv`?**  
   - To ensure the **correct number of arguments** are provided.  

2. **What does `sys.exit(2)` do?**  
   - It terminates the script with an **error code** indicating improper usage.  

---

---

## **Extracting Arguments and Formatting the Message:**
```python
    server, fromaddr, toaddrs = sys.argv[1], sys.argv[2], sys.argv[3:]
    message = message_template.format(', '.join(toaddrs), fromaddr)
```

### **Explanation:**
1. **Extracting Arguments:**
   ```python
   server, fromaddr, toaddrs = sys.argv[1], sys.argv[2], sys.argv[3:]
   ```
   - **`server`**: SMTP server address.  
   - **`fromaddr`**: Sender's email address.  
   - **`toaddrs`**: List of recipient addresses.  

2. **Formatting the Message:**
   ```python
   message = message_template.format(', '.join(toaddrs), fromaddr)
   ```
   - The email message is generated using the **template** and the provided arguments.  

---

### ✅ **Potential Questions:**
1. **Why use `sys.argv[3:]`?**  
   - To allow multiple **recipients** to be specified after the sender's address.  

2. **What does the `format()` method do here?**  
   - It replaces the placeholders `{}` with the provided values.  

---

---

## **Sending the Email Using `smtplib`:**
```python
    connection = smtplib.SMTP(server)
    connection.sendmail(fromaddr, toaddrs, message)
    connection.quit()
```

### **Explanation:**
1. **Connecting to the SMTP Server:**
   ```python
   connection = smtplib.SMTP(server)
   ```
   - Connects to the specified **SMTP server**.  

2. **Sending the Email:**
   ```python
   connection.sendmail(fromaddr, toaddrs, message)
   ```
   - **`sendmail()`** sends the email using the provided **sender**, **recipients**, and **message body**.  

3. **Closing the Connection:**
   ```python
   connection.quit()
   ```
   - **`quit()`** terminates the SMTP session properly.  

---

### ✅ **Potential Questions:**
1. **What does `smtplib.SMTP(server)` do?**  
   - It establishes a connection to the specified **SMTP server**.  

2. **What would happen if you forget to call `connection.quit()`?**  
   - The connection would remain **open**, potentially causing **resource leaks**.  

3. **Why is `sendmail()` used instead of `send_message()`?**  
   - `sendmail()` is simpler for **plain text messages** but lacks **MIME support** for complex emails.  

---

---

## **Confirmation Message:**
```python
    s = '' if len(toaddrs) == 1 else 's'
    print("Message sent to {} recipient{}".format(len(toaddrs), s))
```

### **Explanation:**
- **Confirmation Message:**  
   ```python
   s = '' if len(toaddrs) == 1 else 's'
   print("Message sent to {} recipient{}".format(len(toaddrs), s))
   ```
   - Displays a message confirming the number of recipients.  
   - Adjusts **pluralization** for proper grammar.  

---

### ✅ **Potential Questions:**
1. **Why check for multiple recipients using the conditional operator?**  
   - To ensure the message uses the correct grammar (singular vs. plural).  

---

---

## **Main Execution Block:**
```python
if __name__ == '__main__':
    main()
```

### **Explanation:**
- **Python Standard Entry Point:**  
   - Ensures the script runs **only when executed directly**, not when imported.  

---

### ✅ **Potential Questions:**
1. **Why use `if __name__ == "__main__":` in Python scripts?**  
   - To prevent the script from running when **imported** as a module.  

---

---

# ✅ **Summary of Key Concepts:**
| **Concept**                    | **Explanation**                                   |
|--------------------------------|--------------------------------------------------|
| **SMTP Protocol**              | Used for sending emails over a network.          |
| **smtplib**                    | Python's built-in library for sending emails.    |
| **Plain Text Email**           | The script sends a basic **plain-text** message. |
| **Command-Line Arguments**     | Used for passing **server**, **sender**, and **recipients**. |
| **Error Handling**             | The script checks for **incorrect usage** and exits. |

---

---

# ✅ **Exam-Style Questions:**

### **True/False Questions:**
1. **True or False:** The script uses `smtplib` to send emails.  
   - **True**  

2. **True or False:** The script supports HTML emails.  
   - **False** (It only supports **plain text**).  

---

### **Multiple Choice Questions (MCQ):**
1. **What does the `smtplib.SMTP(server)` method do?**  
   - A) Receives emails from a server.  
   - B) Sends an email directly to recipients.  
   - C) Establishes a connection to an SMTP server.  
   - **Answer:** C) Establishes a connection to an SMTP server.  

2. **Why use `sys.argv[3:]` in this script?**  
   - A) To collect the list of recipients.  
   - B) To fetch the SMTP server address.  
   - C) To store the message body.  
   - **Answer:** A) To collect the list of recipients.  

---

### **Short Answer Questions:**
1. **How can you modify the script to send a message with multiple recipients?**  
   - List additional recipients after the sender address in `sys.argv`.  

2. **What happens if you omit the call to `connection.quit()`?**  
   - The SMTP session remains **open**, potentially causing **resource leaks**.  

---

