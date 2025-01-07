# **Python SMTP Email Sender with EHLO Support (`ehlo.py`) - Explained with Exam Questions**
---

This script demonstrates how to send a **plain text email** using Python's `smtplib` while checking the **SMTP server's capabilities** using the **EHLO** and **HELO** commands. It also verifies message size limits before sending the email.

---

---

## **Imports and Setup:**
```python
import smtplib, socket, sys
```

### **Explanation:**
- **`smtplib`**: Provides functions for sending emails using the **SMTP protocol**.  
- **`socket`**: Handles low-level **network errors**.  
- **`sys`**: Used for **command-line arguments** and handling errors gracefully.  

---

### ✅ **Potential Questions:**
1. **What is the purpose of `smtplib` in this script?**  
   - It is used to **send emails** via an **SMTP server**.  

2. **Why import `socket` in the script?**  
   - To catch and handle **network-related errors** like connection issues.  

---

---

## **Email Message Template:**
```python
message_template = """To: {}
From: {}
Subject: Test Message from simple.py

Hello,

This is a test message sent to you from the ehlo.py program
in Foundations of Python Network Programming.
"""
```

### **Explanation:**
- A **basic email template** is defined with placeholders `{}` for:  
   - `To` (Recipient Address)  
   - `From` (Sender Address)  

- The placeholders are filled using the **`format()`** method later in the script.

---

### ✅ **Potential Questions:**
1. **Why use a message template in this script?**  
   - To simplify the process of creating the **email body**.  

2. **How does the script fill the placeholders in the template?**  
   - By using the **`format()`** method.  

---

---

## **Main Function: Argument Handling and Sending the Email**
```python
def main():
    if len(sys.argv) < 4:
        name = sys.argv[0]
        print("usage: {} server fromaddr toaddr [toaddr...]".format(name))
        sys.exit(2)
```

### **Explanation:**
1. **Argument Parsing:**
   ```python
   if len(sys.argv) < 4:
   ```
   - The script expects **at least 3 arguments**:  
     - `server`: The SMTP server.  
     - `fromaddr`: Sender's email address.  
     - `toaddrs`: One or more recipient addresses.  

2. **Error Handling for Missing Arguments:**
   ```python
   sys.exit(2)
   ```
   - If the arguments are missing, the script exits with **error code 2**.  

---

### ✅ **Potential Questions:**
1. **Why check the length of `sys.argv`?**  
   - To ensure all **required arguments** are provided.  

2. **What does `sys.exit(2)` indicate?**  
   - It exits the script with an **error code** indicating incorrect usage.  

---

---

## **Preparing the Message and Sending It:**
```python
    server, fromaddr, toaddrs = sys.argv[1], sys.argv[2], sys.argv[3:]
    message = message_template.format(', '.join(toaddrs), fromaddr)
```

### **Explanation:**
1. **Extracting Arguments:**
   ```python
   server, fromaddr, toaddrs = sys.argv[1], sys.argv[2], sys.argv[3:]
   ```
   - The server, sender, and recipients are extracted from the **command-line arguments**.  

2. **Formatting the Message:**
   ```python
   message = message_template.format(', '.join(toaddrs), fromaddr)
   ```
   - The message is generated using the **template** with the provided arguments.  

---

### ✅ **Potential Questions:**
1. **Why use `sys.argv[3:]`?**  
   - To support **multiple recipients**.  

2. **What does `format()` do in the message construction?**  
   - It replaces the placeholders `{}` with the provided values.  

---

---

## **Connecting to the SMTP Server and Sending the Message:**
```python
    try:
        connection = smtplib.SMTP(server)
        report_on_message_size(connection, fromaddr, toaddrs, message)
    except (socket.gaierror, socket.error, socket.herror,
            smtplib.SMTPException) as e:
        print("Your message may not have been sent!")
        print(e)
        sys.exit(1)
    else:
        s = '' if len(toaddrs) == 1 else 's'
        print("Message sent to {} recipient{}".format(len(toaddrs), s))
        connection.quit()
```

---

### **Explanation:**
1. **Connecting to the SMTP Server:**
   ```python
   connection = smtplib.SMTP(server)
   ```
   - Establishes a connection to the provided **SMTP server**.  

2. **Calling the EHLO Check Function:**
   ```python
   report_on_message_size(connection, fromaddr, toaddrs, message)
   ```
   - Calls the **`report_on_message_size()`** function to check **server capabilities** and message size.  

3. **Handling Connection Errors:**
   ```python
   except (socket.gaierror, socket.error, socket.herror, smtplib.SMTPException) as e:
   ```
   - Catches **network and SMTP errors** and exits if any issue occurs.  

---

### ✅ **Potential Questions:**
1. **What does `smtplib.SMTP()` do?**  
   - It establishes a connection to the **SMTP server**.  

2. **Why call the `report_on_message_size()` function?**  
   - To verify if the **SMTP server** can accept the message based on its size.  

---

---

## **Function: Checking EHLO and Message Size**
```python
def report_on_message_size(connection, fromaddr, toaddrs, message):
    code = connection.ehlo()[0]
    uses_esmtp = (200 <= code <= 299)
    if not uses_esmtp:
        code = connection.helo()[0]
        if not (200 <= code <= 299):
            print("Remote server refused HELO; code:", code)
            sys.exit(1)

    if uses_esmtp and connection.has_extn('size'):
        print("Maximum message size is", connection.esmtp_features['size'])
        if len(message) > int(connection.esmtp_features['size']):
            print("Message too large; aborting.")
            sys.exit(1)

    connection.sendmail(fromaddr, toaddrs, message)
```

---

### **Explanation:**
1. **Sending the EHLO Command:**
   ```python
   code = connection.ehlo()[0]
   uses_esmtp = (200 <= code <= 299)
   ```
   - Sends the **EHLO (Extended Hello)** command to the server.  
   - EHLO is required for **ESMTP**, which supports **enhanced features** like message size limits.  

2. **Fallback to HELO (for Legacy Servers):**
   ```python
   if not uses_esmtp:
       code = connection.helo()[0]
   ```
   - If **EHLO** fails, it attempts the **older HELO** command instead.  

3. **Checking Message Size:**
   ```python
   if uses_esmtp and connection.has_extn('size'):
       if len(message) > int(connection.esmtp_features['size']):
           print("Message too large; aborting.")
           sys.exit(1)
   ```
   - If the server supports **ESMTP size extensions**, the script checks if the message exceeds the server's limit.  

---

### ✅ **Potential Questions:**
1. **What is the difference between `EHLO` and `HELO` in SMTP?**  
   - **EHLO** is used in **ESMTP** and supports advanced features like message size limits.  
   - **HELO** is a simpler, older command for **basic SMTP servers**.  

2. **What does `connection.has_extn('size')` check?**  
   - Whether the server supports the **message size limit extension**.  

---

---

# ✅ **Summary of Key Concepts:**
| **Concept**                  | **Explanation**                                     |
|------------------------------|-----------------------------------------------------|
| **SMTP Protocol**            | Used for sending emails over a network.            |
| **EHLO and HELO**            | `EHLO` is for extended SMTP, while `HELO` is basic. |
| **smtplib Module**            | Used for **sending emails** in Python.              |
| **Error Handling**           | Catches **network and SMTP errors** with `try...except`.|
| **Message Size Handling**    | Checks if the server can handle the message size.   |

---

---

# ✅ **Exam-Style Questions:**

### **True/False Questions:**
1. **True or False:** `EHLO` is always required when sending an email.  
   - **False** (Legacy servers may only support `HELO`).  

2. **True or False:** The script checks the server’s message size limit.  
   - **True**  

---

