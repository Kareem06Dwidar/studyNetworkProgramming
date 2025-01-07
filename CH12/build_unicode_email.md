# **Python Unicode Email Builder (`build_unicode_email.py`) - Explained with Exam Questions**
---

This script demonstrates how to create a **properly formatted Unicode email** using Python's `email` module. It focuses on **MIME encoding**, **character sets**, and **proper formatting** of email headers and bodies.

---

---

## **Imports and Setup:**
```python
import email.message, email.policy, sys
```

### **Explanation:**
- **`email.message`**: Used to create an **email message object**.  
- **`email.policy`**: Provides policies for email formatting.  
- **`sys`**: Used to handle **standard output** for email generation.  

---

### ✅ **Potential Questions:**
1. **Why use `email.message.EmailMessage` instead of a regular string?**  
   - To ensure proper handling of **MIME** and **Unicode** data with standard headers.  

2. **What does `sys.stdout.buffer` do in this script?**  
   - It outputs the **binary representation** of the email message directly to the terminal.  

---

---

## **Defining the Email Content:**
```python
text = """\
Hwær cwom mearg? Hwær cwom mago?
Hwær cwom maþþumgyfa?
Hwær cwom symbla gesetu?
Hwær sindon seledreamas?"""
```

---

### **Explanation:**
- **Multiline String:**  
   - This variable stores a **multiline Unicode string** written in **Old English**.  

---

### ✅ **Potential Questions:**
1. **Why use triple quotes (`"""`) for the `text` variable?**  
   - To define a **multiline string** conveniently.  

2. **Does Python handle Unicode characters automatically in strings?**  
   - Yes, starting from Python **3.x**, strings are **Unicode by default**.  

---

---

## **Main Function: Creating and Formatting the Email**
```python
def main():
    message = email.message.EmailMessage(email.policy.SMTP)
    message['To'] = 'Böðvarr <recipient@example.com>'
    message['From'] = 'Eardstapa <sender@example.com>'
    message['Subject'] = 'Four lines from The Wanderer'
    message['Date'] = email.utils.formatdate(localtime=True)
```

---

### **Explanation:**
1. **Creating the Email Message Object:**
   ```python
   message = email.message.EmailMessage(email.policy.SMTP)
   ```
   - **`EmailMessage`** creates a new **MIME-compliant** email message.  
   - **`email.policy.SMTP`** ensures the message follows **SMTP standards**.  

2. **Setting Email Headers:**
   ```python
   message['To'] = 'Böðvarr <recipient@example.com>'
   message['From'] = 'Eardstapa <sender@example.com>'
   message['Subject'] = 'Four lines from The Wanderer'
   message['Date'] = email.utils.formatdate(localtime=True)
   ```
   - The headers set include:  
     - **To:** Recipient's address with a **Unicode name**.  
     - **From:** Sender's address with a **Unicode name**.  
     - **Subject:** Describes the content of the message.  
     - **Date:** Automatically generated using `email.utils.formatdate()`.

---

### ✅ **Potential Questions:**
1. **What does `email.policy.SMTP` do?**  
   - It ensures the email formatting adheres to the **SMTP standard**.  

2. **Why use `email.utils.formatdate()`?**  
   - To generate a properly formatted **date header** for the email.  

3. **What happens if you omit the `Subject` header?**  
   - The email will still be valid but will appear with a **blank subject line**.

---

---

## **Setting the Email Content (with Quoted-Printable Encoding):**
```python
    message.set_content(text, cte='quoted-printable')
```

---

### **Explanation:**
1. **Adding the Message Body:**
   ```python
   message.set_content(text, cte='quoted-printable')
   ```
   - **`set_content()`** sets the body of the email.  
   - **`cte='quoted-printable'`** specifies **Quoted-Printable Encoding**, which is ideal for **Unicode** text in emails.  

---

### ✅ **Potential Questions:**
1. **What is Quoted-Printable Encoding?**  
   - A **MIME encoding** that allows non-ASCII characters to be safely included in email messages.  

2. **Why use Quoted-Printable Encoding here?**  
   - To handle **Unicode characters** correctly in email clients.  

---

---

## **Outputting the Email as Bytes:**
```python
    sys.stdout.buffer.write(message.as_bytes())
```

---

### **Explanation:**
1. **Printing the Email to Standard Output:**
   ```python
   sys.stdout.buffer.write(message.as_bytes())
   ```
   - **`as_bytes()`** converts the email message to its **raw binary representation**.  
   - **Why?** Emails are transmitted as **bytes** over SMTP.  

---

### ✅ **Potential Questions:**
1. **Why use `sys.stdout.buffer` instead of `print()`?**  
   - To ensure the output is in **binary format**, as required for **SMTP transmission**.  

2. **What is the difference between `as_string()` and `as_bytes()`?**  
   - **`as_string()`** returns a **string representation** of the email.  
   - **`as_bytes()`** returns a **binary representation** suitable for transmission.  

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
1. **Why is the `if __name__ == "__main__":` block necessary?**  
   - It prevents the script from running automatically when imported as a module.  

---

---

# ✅ **Key Concepts Recap:**
| **Concept**                    | **Explanation**                                   |
|--------------------------------|--------------------------------------------------|
| **Unicode Handling in Emails** | Uses **Quoted-Printable Encoding** for Unicode characters. |
| **MIME Email Creation**        | `EmailMessage` class simplifies MIME message construction. |
| **Binary vs String Output**    | **`as_bytes()`** outputs a binary version of the email. |
| **SMTP Compliance**            | The `email.policy.SMTP` ensures standards for SMTP servers. |
| **Email Headers Management**   | Headers like **To, From, Date, Subject** are set manually. |

---

---

# ✅ **Exam-Style Questions:**

### **True/False Questions:**
1. **True or False:** The script uses `quoted-printable` encoding to handle Unicode characters.  
   - **True**  

2. **True or False:** The `email.message.EmailMessage` class automatically handles MIME encoding.  
   - **True**  

---

### **Multiple Choice Questions (MCQ):**
1. **What does the `set_content()` method do?**  
   - A) Sets the email headers.  
   - B) Adds the email body with specified content transfer encoding.  
   - C) Attaches a file.  
   - **Answer:** B) Adds the email body with specified content transfer encoding.  

2. **Why use `as_bytes()` instead of `as_string()` for email output?**  
   - A) To send the email as a text message.  
   - B) To ensure the email is **SMTP-compliant** with binary output.  
   - C) To allow easier debugging.  
   - **Answer:** B) To ensure the email is SMTP-compliant with binary output.  

---

### **Short Answer Questions:**
1. **What is the advantage of using `quoted-printable` encoding in emails?**  
   - It allows **Unicode characters** to be included safely in **7-bit encoded** email messages.  

2. **How can you modify the script to add a CC (Carbon Copy) header?**  
   ```python
   message['CC'] = 'cc@example.com'
   ```  

---

