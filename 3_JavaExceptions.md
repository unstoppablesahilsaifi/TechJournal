
# 🚨 Java Exceptions – Troubleshooting Guide

A quick-reference handbook for **understanding & fixing common Java exceptions** in production or during interviews.  
💡 *Meaning stays exactly the same, just simpler for faster recall.*

---

## 1️⃣ NullPointerException
📌 **Meaning:** Trying to use something that is `null` (doesn’t exist).  
🔍 **Check:**
- Was the object created before use?  
- Did the DB/API return `null`?  
- In `obj.getX().getY()`, is something in between `null`?  

---

## 2️⃣ ArrayIndexOutOfBoundsException
📌 **Meaning:** Accessing an array/list position that doesn’t exist.  
🔍 **Check:**
- Is index bigger than size?  
- Is the list empty?  
- Loop condition wrong?  

---

## 3️⃣ ClassNotFoundException
📌 **Meaning:** Java can’t find the class at runtime.  
🔍 **Check:**
- Missing JAR in classpath?  
- Package/class name correct?  

---

## 4️⃣ NoClassDefFoundError
📌 **Meaning:** Class existed at compile time but not at runtime.  
🔍 **Check:**
- Was the JAR skipped in deployment?  
- Is classpath same in all environments?  

---

## 5️⃣ IllegalArgumentException
📌 **Meaning:** Method got a bad/wrong value.  
🔍 **Check:**
- Value `null` or invalid?  
- Number outside allowed range?  

---

## 6️⃣ NumberFormatException
📌 **Meaning:** Trying to turn non-number into a number.  
🔍 **Check:**
- String is `"abc"` or empty?  
- Wrong characters from data source?  

---

## 7️⃣ SQLException
📌 **Meaning:** Database-related problem.  
🔍 **Check:**
- SQL query correct?  
- DB connection OK?  
- Constraint/index issue?  

---

## 8️⃣ FileNotFoundException
📌 **Meaning:** File location doesn’t exist.  
🔍 **Check:**
- Path correct?  
- File deployed?  
- Read permissions OK?  

---

## 9️⃣ IOException
📌 **Meaning:** Problem in file/network/disk read/write.  
🔍 **Check:**
- Disk full?  
- Network down?  
- Permissions correct?  

---

## 🔟 SocketTimeoutException
📌 **Meaning:** Network request took too long.  
🔍 **Check:**
- Server slow?  
- Timeout too short?  

---

## 1️⃣1️⃣ ConcurrentModificationException
📌 **Meaning:** Changing a collection while looping through it.  
🔍 **Check:**
- Using `for-each` and removing items?  
- Another thread changing the same collection?  

---

## 1️⃣2️⃣ StackOverflowError
📌 **Meaning:** Method kept calling itself endlessly (recursion loop).  
🔍 **Check:**
- Missing base case in recursion?  
- Two methods calling each other endlessly?  

---

## 1️⃣3️⃣ OutOfMemoryError
📌 **Meaning:** Java ran out of memory.  
🔍 **Check:**
- Memory leak (objects not released)?  
- Loaded huge data into memory?  
- Heap size too small?  

---

## 1️⃣4️⃣ IllegalStateException
📌 **Meaning:** Method called at the wrong time.  
🔍 **Check:**
- Called `iterator.next()` without `hasNext()`?  
- Using stream after closing it?  

---

## 1️⃣5️⃣ InvocationTargetException
📌 **Meaning:** Reflection method threw an exception.  
🔍 **Check:**
- Look at `.getCause()` for real problem.  
- Check actual method’s code.  

---

### 📚 Tip for Production Support
When checking logs:
- Always **search for root cause in stack trace** (look for the *first* exception in logs).
- For chained exceptions, **focus on the original error** — others may just be side effects.

---

