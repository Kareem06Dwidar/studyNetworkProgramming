This script connects to an **FTP server** and **recursively lists all directories and files** from a specified starting directory using Python's `ftplib`. It demonstrates **recursion**, **error handling**, and **FTP directory navigation**.

---

---

## **Imports and Setup:**
```python
from ftplib import FTP, error_perm
```

### **Explanation:**
- **`FTP`**: Core class from the `ftplib` library for FTP connections and operations.  
- **`error_perm`**: An exception from `ftplib` raised when a **permission error** occurs (e.g., trying to enter a non-directory).  

---

### ✅ **Potential Questions:**
1. **What is the purpose of `error_perm` in the script?**  
   - It handles **permission errors** when trying to access non-directories or restricted directories.  

2. **What happens if `error_perm` is not caught?**  
   - The script would terminate with a traceback error when trying to enter restricted directories.  

---

---

## **Function: `walk_dir()` - Recursively Listing Directories**
```python
def walk_dir(ftp, dirpath):
    original_dir = ftp.pwd()
    try:
        ftp.cwd(dirpath)
    except error_perm:
        return  # ignore non-directories and ones we cannot enter
    print(dirpath)
    names = sorted(ftp.nlst())
    for name in names:
        walk_dir(ftp, dirpath + '/' + name)
    ftp.cwd(original_dir)  # return to cwd of our caller
```

---

### **Explanation:**
This function **recursively lists** all files and subdirectories on an FTP server.

---

### **Step-by-Step:**
1. **Store the Original Directory:**
   ```python
   original_dir = ftp.pwd()
   ```
   - **`ftp.pwd()`** stores the current working directory before changing it.

2. **Change Directory with Error Handling:**
   ```python
   try:
       ftp.cwd(dirpath)
   except error_perm:
       return  # ignore non-directories and ones we cannot enter
   ```
   - **`ftp.cwd()`** attempts to change to the specified directory.
   - If the directory is **not accessible** or **not a directory**, an **`error_perm`** exception is raised and caught.

3. **Print Directory Name:**
   ```python
   print(dirpath)
   ```
   - The current directory path is printed after successful access.

4. **List Files and Recursively Call:**
   ```python
   names = sorted(ftp.nlst())
   for name in names:
       walk_dir(ftp, dirpath + '/' + name)
   ```
   - **`ftp.nlst()`** lists the filenames and subdirectories in the current directory.
   - **Recursion:** The function calls itself on each item in the directory.

5. **Return to the Original Directory:**
   ```python
   ftp.cwd(original_dir)
   ```
   - Returns to the previous working directory to avoid losing track during recursion.

---

### ✅ **Potential Questions:**
1. **What does `ftp.cwd()` do in the script?**  
   - It **changes the working directory** on the remote server.  

2. **How does the script handle permission errors?**  
   - By using a `try...except` block and catching the `error_perm` exception.  

3. **Why is recursion used in this script?**  
   - Recursion allows the script to explore all **subdirectories** automatically.  

4. **What happens if a directory cannot be accessed?**  
   - The script **skips** the directory and continues with the next one.

---

---

## **Main Function: Establishing Connection and Initiating Recursive Listing**
```python
def main():
    ftp = FTP('ftp.kernel.org')
    ftp.login()
    walk_dir(ftp, '/pub/linux/kernel/Historic/old-versions')
    ftp.quit()
```

---

### **Explanation:**
1. **Connect to the FTP Server:**
   ```python
   ftp = FTP('ftp.kernel.org')
   ftp.login()
   ```
   - Connects to the **`ftp.kernel.org`** server and logs in as an **anonymous user**.

2. **Call the Recursive Function:**
   ```python
   walk_dir(ftp, '/pub/linux/kernel/Historic/old-versions')
   ```
   - Calls the `walk_dir()` function to start listing the contents of the specified directory.

3. **Closing the Connection:**
   ```python
   ftp.quit()
   ```
   - Closes the FTP connection properly using `ftp.quit()`.

---

### ✅ **Potential Questions:**
1. **What does `ftp.quit()` do?**  
   - It sends the `QUIT` command to the server and **closes the connection gracefully**.  

2. **Why use anonymous FTP here?**  
   - The server is public, and **anonymous access** is allowed for browsing files.

---

---

## **Main Execution Block:**
```python
if __name__ == '__main__':
    main()
```

### **Explanation:**
- **Python Standard Entry Point:**  
   - Ensures the script runs its logic only when executed directly and not imported.

---

### ✅ **Potential Questions:**
1. **Why use `if __name__ == "__main__":` in Python scripts?**  
   - To prevent the script from running automatically when imported as a module.  

---

---

# ✅ **Key Concepts Recap:**
| **Concept**                  | **Explanation**                                     |
|------------------------------|-----------------------------------------------------|
| **FTP Protocol**             | Used for transferring files and directories over a network. |
| **Recursive Function**       | The `walk_dir()` function uses **recursion** to explore subdirectories. |
| **Error Handling**           | The script uses `try...except` to catch **permission errors**. |
| **FTP Commands Used**        | `cwd()` (change directory), `nlst()` (list files), `quit()` (close connection). |
| **Anonymous Login**          | No credentials needed for public servers.           |

---

---

# ✅ **Exam-Style Questions:**

### **True/False Questions:**
1. **True or False:** The script uses recursion to explore all subdirectories.  
   - **True**  

2. **True or False:** If a permission error occurs, the script will terminate.  
   - **False** (It handles errors using `try...except`).  

---

### **Multiple Choice Questions (MCQ):**
1. **What does the `ftp.nlst()` method do?**  
   - A) Uploads a file.  
   - B) Returns a list of filenames in the current directory.  
   - C) Deletes a file.  
   - **Answer:** B) Returns a list of filenames in the current directory.  

2. **Why is recursion used in this script?**  
   - A) To create multiple FTP connections.  
   - B) To avoid permission errors.  
   - C) To explore all directories and subdirectories automatically.  
   - **Answer:** C) To explore all directories and subdirectories automatically.  

---

### **Short Answer Questions:**
1. **Explain how the script handles directories it cannot access.**  
   - The script uses a `try...except` block and **catches the `error_perm`** exception to skip non-accessible directories.  

2. **What does the `ftp.pwd()` method do?**  
   - It returns the **current working directory** on the FTP server.

3. **How can you modify the script to log in with a username and password?**  
   ```python
   ftp.login(user="username", passwd="password")
   ```

---

