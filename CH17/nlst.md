
This script connects to an **FTP server**, navigates to a specified directory, and lists the contents using **Python's `ftplib` module**. It demonstrates **anonymous FTP login**, **directory navigation**, and **simple directory listing**.

---

---

## **Imports and Setup:**
```python
from ftplib import FTP
```

### **Explanation:**
- **`ftplib`**: Standard Python library for interacting with **FTP servers** (File Transfer Protocol).

---

### ✅ **Potential Questions:**
1. **What is the `ftplib` module used for?**  
   - It allows interaction with **FTP servers** for tasks like file transfer and directory listing.

2. **What protocol does `ftplib` use?**  
   - It uses the **FTP (File Transfer Protocol)** for unencrypted file transfers.

---

---

## **Main Function: Establishing the FTP Connection**
```python
def main():
    ftp = FTP('ftp.ibiblio.org')
    ftp.login()
```

### **Explanation:**
1. **Connecting to the Server:**
   ```python
   ftp = FTP('ftp.ibiblio.org')
   ```
   - Creates an **FTP connection** to the server at `ftp.ibiblio.org`.

2. **Anonymous Login:**
   ```python
   ftp.login()
   ```
   - Logs in as an **anonymous user** (no username or password required).

---

### ✅ **Potential Questions:**
1. **What does `ftp.login()` do when called without arguments?**  
   - It logs in as an **anonymous user**.  

2. **How would you modify the script to log in with credentials?**  
   ```python
   ftp.login(user="username", passwd="password")
   ```  

---

---

## **Changing the Directory:**
```python
    ftp.cwd('/pub/academic/astronomy/')
```

### **Explanation:**
- **Change Working Directory (CWD):**
   ```python
   ftp.cwd('/pub/academic/astronomy/')
   ```
   - **`cwd()`** changes the **remote working directory** on the FTP server.

---

### ✅ **Potential Questions:**
1. **What does the `ftp.cwd()` command do?**  
   - It changes the **remote working directory** on the server.  

2. **How can you verify the current working directory?**  
   ```python
   print("Current Directory:", ftp.pwd())
   ```

---

---

## **Retrieving the Directory Listing:**
```python
    entries = ftp.nlst()
    ftp.quit()
```

### **Explanation:**
1. **Retrieve Directory Contents:**
   ```python
   entries = ftp.nlst()
   ```
   - **`nlst()`** returns a **simple list** of filenames in the current directory.  

2. **Quit the FTP Connection:**
   ```python
   ftp.quit()
   ```
   - **`quit()`** properly closes the FTP connection by sending the `QUIT` command.

---

### ✅ **Potential Questions:**
1. **What is the difference between `ftp.nlst()` and `ftp.dir()`?**  
   - **`ftp.nlst()`** provides a **simple list of filenames**.  
   - **`ftp.dir()`** provides a **detailed listing** including file permissions and file sizes.  

2. **Why is it important to call `ftp.quit()`?**  
   - To properly terminate the session and free server resources.

---

---

## **Printing the Directory Entries:**
```python
    print(len(entries), "entries:")
    for entry in sorted(entries):
        print(entry)
```

### **Explanation:**
1. **Count and Print Entries:**
   ```python
   print(len(entries), "entries:")
   ```
   - Prints the **total number of files and directories** retrieved.

2. **Sorting and Displaying Entries:**
   ```python
   for entry in sorted(entries):
       print(entry)
   ```
   - **`sorted()`** is used to list the filenames in **alphabetical order** for better readability.

---

### ✅ **Potential Questions:**
1. **Why use `sorted()` when printing the entries?**  
   - To display the filenames in **alphabetical order**.  

2. **How would you modify the script to list the filenames without sorting?**  
   ```python
   for entry in entries:
       print(entry)
   ```

---

---

## **Main Execution Block:**
```python
if __name__ == '__main__':
    main()
```

### **Explanation:**
- **Python Standard Practice:**  
   - This ensures the `main()` function runs **only when executed directly**, not when imported as a module.

---

### ✅ **Potential Questions:**
1. **What is the purpose of `if __name__ == '__main__':`?**  
   - To ensure the script runs only when executed directly.  

2. **Why is it important in reusable scripts?**  
   - It prevents the script's code from running when imported as a module into other programs.

---

---

# ✅ **Summary of Key Concepts:**
| **Concept**                  | **Explanation**                                     |
|------------------------------|-----------------------------------------------------|
| **FTP Protocol**             | Used for transferring files over a network.         |
| **Anonymous Login**          | Allows public access without a password.            |
| **Directory Listing Methods**| `ftp.nlst()` for filenames only, `ftp.dir()` for detailed listing. |
| **Connection Management**    | `ftp.login()`, `ftp.cwd()`, `ftp.quit()`             |
| **Sorting Results**          | `sorted()` for alphabetical order of filenames.      |

---

---

# ✅ **Exam-Style Questions:**

### **True/False Questions:**
1. **True or False:** The script uses `ftp.dir()` for listing directory contents.  
   - **False** (It uses `ftp.nlst()`).  

2. **True or False:** The `ftp.nlst()` method provides a list of filenames without metadata.  
   - **True**  

---

### **Multiple Choice Questions (MCQ):**
1. **What does the `ftp.nlst()` method return?**  
   - A) A list of filenames only.  
   - B) A detailed list including file permissions and sizes.  
   - C) The size of each file.  
   - **Answer:** A) A list of filenames only.  

2. **What does `ftp.cwd()` do?**  
   - A) Changes the working directory.  
   - B) Uploads a file.  
   - C) Prints the working directory.  
   - **Answer:** A) Changes the working directory.  

---

### **Short Answer Questions:**
1. **How can you modify the script to list filenames without sorting them?**  
   ```python
   for entry in entries:
       print(entry)
   ```  

2. **What is the difference between `ftp.dir()` and `ftp.nlst()`?**  
   - `ftp.nlst()` provides a **simple list of filenames**, while `ftp.dir()` provides a **detailed listing** including permissions, file size, and owner.

---

