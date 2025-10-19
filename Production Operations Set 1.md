# ⚙️ Java Production Support & Troubleshooting Guide

> 📘 A handy guide for Java support engineers — covering service downtime, dumps, memory leaks, SSL issues, job failures & more.

---

## 1️⃣ 🚨 What do you do when a service is down? (Steps to troubleshoot)

### 🧩 Steps:

* ✅ **Verify the Problem:** Check CPU or memory usage.
* 📜 **Check Logs:** Look for

  ```
  DB connection failed, ERROR, Exception, Connection refused, Timeout, OutOfMemoryError...
  ```
* 🌐 **Ping the URL** to confirm connectivity.
* 🔁 **Rollback or Restart:**

  * Rollback if a recent deployment caused the issue.
  * Restart the server after **downtime approval** as per ITIL workflow.
* 🧾 **Prepare RCA** → *What happened + Why it happened + How it was fixed + Preventive action.*

**Example RCA:**

```
Root Cause: Database connection pool exhausted due to unclosed ResultSet.
Fix: Increased max pool size from 10 → 20 and fixed code leak.
Prevention: Added connection leak detection.
```

---

## 2️⃣ 🧵 What is Thread Dump / Heap Dump and when do you take it?

### 🧵 Thread Dump:

* A **snapshot of all active threads** running in a JVM at a specific moment.
* Helps identify:

  * What each thread is doing
  * Which threads are blocked or stuck
  * Deadlocks

**Command:**

```bash
jstack -l <PID> > thread.dump.out
```

**Example Output:**

```
"Thread-21" #45 prio=5 WAITING on java.lang.Object@6b2f
   at java.lang.Object.wait(Native Method)
   at com.example.PaymentService.process(PaymentService.java:88)
```

🧠 Shows `Thread-21` is waiting on a lock — possible deadlock or stuck thread.
📈 **Take Thread Dump when CPU is high.**

---

### 🧠 Heap Dump:

* A **snapshot of JVM memory (heap area)** at a given time.
* Helps find:

  * Memory leaks
  * Which objects consume memory
  * How much memory each class occupies

**Command:**

```bash
jmap -dump:format=b,file=heap.hprof <PID>
```

📈 **Take Heap Dump when memory is high or `OutOfMemoryError` in logs.**

---

## 3️⃣ 💥 Common OutOfMemoryError causes and how to fix them

### 🔹 A. Java Heap Space

**Symptom:**

```
java.lang.OutOfMemoryError: Java heap space
```

**Cause:** Too many objects in memory, unreleased references, connections not closed, cache not cleared.
**Fix:**

* Close unused collections
* Clear cache
* Increase heap (temporary fix)

---

### 🔹 B. Unable to Create New Native Thread

**Symptom:**

```
java.lang.OutOfMemoryError: unable to create new native thread
```

**Cause:** Too many threads created.
**Fix:** Reduce thread pool size.

---

## 4️⃣ 🔐 How to handle certificate expiry (Keystore / Truststore)

* 🔎 Check via browser or logs. When expired, SSL/TLS fails with:

  ```
  javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path validation failed
  ```

### 🧾 Steps:

1. Generate a CSR and send to client or CA.
2. Request a new certificate (`.crt`, `.cer`, `.pfx`).
3. Update keystore/truststore.
4. 🔄 Restart the server.

---

## 5️⃣ 🔁 Steps for restarting services in production (Safe Restart)

* 🛑 Stop custom services
* 🧱 Stop JBoss
* 💾 Take backup of critical services
* 🚀 Start JBoss and wait till builds are deployed
* ▶️ Start custom services
* 🧪 Perform basic sanity test

---

## 6️⃣ 🕵️‍♂️ How to debug NullPointerException or Timeout issues

### 🔹 NullPointerException

**Step 1:** Check Stack Trace

```
java.lang.NullPointerException
  at com.example.StudentService.getStudent(StudentService.java:45)
```

**Step 2:** Identify Null Variable

```java
Student student = studentRepo.findById(id);
System.out.println(student.getName()); // <-- NPE if student == null
```

**Step 3:** Add Logging

```java
if(student == null){
    log.error("Student object is null for id: {}", id);
}
```

**Step 4:** Apply Fix

```java
Optional<Student> student = studentRepo.findById(id);
String name = student.map(Student::getName).orElse("Unknown");
```

---

### 🔹 Timeout Issue

* **When:** DB Timeout (query too long), Thread Pool Timeout (task waiting too long).
* **Check:** Avoid unnecessary `SELECT *`.
* **Fix:** Optimize queries, increase timeout or thread pool size.

---

## 7️⃣ 🐢 How to analyze slow queries in logs

* 🔍 Identify slow queries (`query_time > 1000` in Splunk/logs).
* ⚡ Check if indexes are used.
* 🚫 Avoid `SELECT *`.
* ✅ Use `LIMIT` and `OFFSET`.

---

## 8️⃣ 🔥 What will you do if CPU utilization is 90%?

* 🧵 Take thread dump
* 🔒 Check for deadlocks / blocked DB calls
* 🔄 Restart the server
* ⚙️ Optimize DB queries, external calls, thread pools, async logic

---

## 9️⃣ 🧠 How to debug memory leak

* 📈 Happens when app holds unused object references, unclosed DB connections, or cache never cleared.

**Example:**

```java
List<String> list = new ArrayList<>();
while(true){
    list.add("leak");
}
```

---

## 🔟 ⏰ A scheduled job is not running — what steps you’ll take?

* ⏱ Check expected run time and last successful run
* 🔍 Verify:

  * Job triggered but failed
  * Job never triggered
  * Job stuck in “running” state
* 🧩 In Spring Boot, ensure `@EnableScheduling` is present
* ⚠️ Maybe service restarted during job’s window
* 🔄 Restart or manually trigger job
* 🧾 Document RCA, configure alerting, store job status (success/failure) in DB.

---

## 1️⃣1️⃣ 🔒 How do you find which thread is blocked?

A **blocked thread** waits for a lock held by another thread.

**Step 1:** Take thread dump.
**Step 2:** Search `"BLOCKED"` threads.

**Example:**

```
"Thread-15" #24 prio=5 BLOCKED
   at com.practiceSpring.service.StudentService.updateStudent(StudentService.java:45)
   - waiting to lock <0x00000006d>
   - locked <0x00000007a>
```

🧠 Thread-15 is waiting for a lock held by another thread.

Find the owner:

```
"Thread-20" #26 prio=5 RUNNABLE
   - locked <0x00000006d>
```

✅ Thread-15 waits for Thread-20.
If Thread-20 is stuck, that’s your bottleneck.
👉 Use timeouts or sleep logic.

---

## 🧾 Summary Table

| #  | Issue              | Diagnostic Tool | Fix Strategy                    |
| -- | ------------------ | --------------- | ------------------------------- |
| 1  | Service Down       | Logs, Ping, RCA | Restart / Rollback              |
| 2  | Thread / Heap Dump | jstack / jmap   | Analyze CPU or Memory           |
| 3  | OutOfMemoryError   | Heap Dump       | Fix leaks / Increase heap       |
| 4  | SSL Expiry         | Browser / CA    | Renew certs / Restart           |
| 5  | Restart Steps      | Manual          | Stop → Backup → Start → Sanity  |
| 6  | NPE / Timeout      | Logs, Debug     | Null checks / Optimize          |
| 7  | Slow Query         | Splunk / Logs   | Index, Optimize, Limit          |
| 8  | High CPU           | Thread Dump     | Optimize code / DB calls        |
| 9  | Memory Leak        | Heap Dump       | Close connections / Clear cache |
| 10 | Job Not Running    | Scheduler Logs  | Enable / Alert / Retry          |
| 11 | Blocked Thread     | Thread Dump     | Identify lock owner             |


