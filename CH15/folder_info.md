# **Python IMAP Folder Information Viewer (`folder_info.py`) - Explained with Exam Questions**
---

This script connects to an **IMAP email server** using Python's `IMAPClient` library. It allows the user to **list detailed information** about a specified **mailbox folder**, such as the **number of messages**, **unread count**, and **recent messages**.

---

---

## **Imports and Setup:**
```python
import getpass, sys
from imapclient import IMAPClient
```

### **Explanation:**
- **`getpass`**: Used for **secure password input** without displaying characters.  
- **`sys`**: Manages **command-line arguments** and **error exits**.  
- **`IMAPClient`**: Simplifies working with **IMAP servers**.  

---

### ✅ **Potential Questions:**
1. **What is the purpose of `IMAPClient` in this script?**  
   - To simplify working with **IMAP servers** for **email management**.  

2. **Why use `getpass.getpass()` instead of `input()`?  
   - To ensure the **password** is not displayed during entry.  

---

---

## **Main Function: Handling Command-Line Arguments and Connecting to IMAP Server**
```python
def main():
    if len(sys.argv) != 4:
        print('usage: %s hostname username foldername' % sys.argv[0])
        sys.exit(2)

    hostname, username, foldername = sys.argv[1:]
    c = IMAPClient(hostname, ssl=True)
    try:
        c.login(username, getpass.getpass())
    except c.Error as e:
        print('Could not log in:', e)
    else:
        select_dict = c.select_folder(foldername, readonly=True)
        for k, v in sorted(select_dict.items()):
            print('%s: %r' % (k, v))
    finally:
        c.logout()
```

---

### **Explanation:**
### **1. Command-Line Argument Handling:**
```python
if len(sys.argv) != 4:
    print('usage: %s hostname username foldername' % sys.argv[0])
    sys.exit(2)
```
- The script expects **3 arguments**:  
   - **Hostname**: IMAP server address.  
   - **Username**: The email account's username.  
   - **Foldername**: The name of the mailbox folder to inspect.  
- If the arguments are missing, it **exits with error code `2`**.

---

### **2. Connecting to the IMAP Server (SSL):**
```python
c = IMAPClient(hostname, ssl=True)
```
- **`IMAPClient`** establishes a **secure connection** using **SSL encryption** (port `993`).  

---

### **3. User Authentication:**
```python
c.login(username, getpass.getpass())
```
- **Prompts** for a **password** and logs into the server using the provided credentials.  

---

### **4. Error Handling for Failed Logins:**
```python
except c.Error as e:
    print('Could not log in:', e)
```
- If **authentication fails**, an error message is printed.  

---

### ✅ **Potential Questions:**
1. **Why use `ssl=True` in `IMAPClient`?**  
   - To ensure a **secure encrypted** connection with the IMAP server.  

2. **What happens if the login fails?**  
   - The script prints `"Could not log in:"` and exits.  

---

---

## **Selecting and Displaying Folder Information:**
```python
select_dict = c.select_folder(foldername, readonly=True)
for k, v in sorted(select_dict.items()):
    print('%s: %r' % (k, v))
```

---

### **Explanation:**
### **1. Selecting the Folder:**
```python
c.select_folder(foldername, readonly=True)
```
- **Opens** the specified folder in **read-only** mode to avoid accidental changes.  

---

### **2. Fetching Folder Details:**
```python
select_dict = c.select_folder(foldername, readonly=True)
```
- **`select_folder()`** returns a **dictionary** containing:  
   - **EXISTS**: Number of messages in the folder.  
   - **RECENT**: Count of recently added messages.  
   - **UIDNEXT**: Next available unique identifier.  
   - **UNSEEN**: Count of unread messages.  

---

### **3. Displaying Folder Information:**
```python
for k, v in sorted(select_dict.items()):
    print('%s: %r' % (k, v))
```
- Loops through the **folder information** and prints the **key-value pairs**.  

---

### ✅ **Potential Questions:**
1. **What does `c.select_folder()` return?**  
   - A **dictionary** containing folder statistics such as:  
     - **EXISTS** (total messages).  
     - **UNSEEN** (unread messages).  
     - **RECENT** (recent messages).  

2. **Why use `readonly=True` when selecting a folder?**  
   - To avoid **modifying** the message states (like marking them as read).  

---

---

## **Closing the Connection:**
```python
finally:
    c.logout()
```

---

### **Explanation:**
- **Closing the IMAP Connection:**  
   ```python
   c.logout()
   ```
   - **Terminates** the IMAP session properly.  

- **Why use `finally`?**  
   - The **`finally`** block ensures the connection is closed, even if an **error** occurs.  

---

### ✅ **Potential Questions:**
1. **Why is it important to call `c.logout()`?**  
   - To properly **close** the IMAP session and free server resources.  

2. **What happens if you don't call `logout()`?**  
   - The connection may remain **open**, leading to **resource leaks**.  

---

---

## **Main Execution Block:**
```python
if __name__ == '__main__':
    main()
```

---

### **Explanation:**
- **Python Standard Entry Point:**  
   - Ensures the script runs **only when executed directly**, not when imported.  

---

### ✅ **Potential Questions:**
1. **Why use `if __name__ == "__main__":`?**  
   - To prevent the script from running when **imported** into another module.  

---

---

# ✅ **Summary of Key Concepts:**
| **Concept**                    | **Explanation**                                   |
|--------------------------------|--------------------------------------------------|
| **IMAP Protocol**               | Used for **retrieving and managing emails**.     |
| **IMAPClient Library**          | Simplifies working with **IMAP servers**.        |
| **Secure Connection (SSL)**     | `ssl=True` enables **encrypted** communication.  |
| **Folder Information Retrieval**| `select_folder()` fetches **folder stats**.      |
| **User Authentication**         | `login()` authenticates using **username** and **password**.|

---

---

# ✅ **Exam-Style Questions:**

### **True/False Questions:**
1. **True or False:** The script uses `IMAPClient` for **secure IMAP connections**.  
   - **True**  

2. **True or False:** The script modifies the messages after selecting the folder.  
   - **False** (The folder is opened in **read-only mode**).  

---

### **Multiple Choice Questions (MCQ):**
1. **What does `c.select_folder(foldername, readonly=True)` do?**  
   - A) Deletes messages in the folder.  
   - B) Opens the folder for **viewing only**. ✅  
   - C) Marks all messages as read.  

2. **What key in `select_folder()` results shows the number of unread messages?**  
   - A) `RECENT`  
   - B) `UNSEEN` ✅  
   - C) `EXISTS`  

---

### **Short Answer Questions:**
1. **What is the purpose of `c.select_folder()`?**  
   - It opens a specified folder and **retrieves message statistics** such as message count and unread messages.  

2. **How can you modify the script to display only the **unread messages count**?  
   ```python
   select_dict = c.select_folder(foldername, readonly=True)
   print('Unread Messages:', select_dict['UNSEEN'])
   ```  

---
