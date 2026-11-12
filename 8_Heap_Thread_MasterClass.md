# Part 1: Thread Dump -- 5 real production examples

Thread Dump mainly tab lete hain jab application hang, slow ya
unresponsive ho.

## 1. Deadlock

Application hang

**Log mein:**

``` text
ERROR: Thread deadlock detected
Found one Java-level deadlock:
```

**Thread Dump mein:**

``` text
Thread-1: BLOCKED
Waiting to lock Object-2
Holding Object-1

Thread-2: BLOCKED
Waiting to lock Object-1
Holding Object-2
```

**Problem:** Dono threads ek doosre ke lock ka wait kar rahe hain.

**Resolution:** - Code mein lock ordering correct karna. - Nested locks
avoid karna. - Permanent code fix deploy karna. - Urgent recovery mein
approved restart.

## 2. Thread Pool Exhaustion

Requests getting stuck

**Log mein:**

``` text
WARN: Task rejected from ThreadPoolExecutor
java.util.concurrent.RejectedExecutionException
```

**Thread Dump mein:**

``` text
"http-worker-1" WAITING
"http-worker-2" WAITING
"http-worker-3" WAITING

at java.util.concurrent.FutureTask.get()
```

**Problem:** Available worker threads busy hain aur naye requests
process nahi ho paa rahe.

**Resolution:** - Thread pool status check karo. - Identify karo kaunse
tasks threads ko occupy kar rahe hain. - Blocking operations aur
downstream delays investigate karo. - Pool size tuning sirf analysis ke
baad.

## 3. Database Connection Pool Exhaustion

Database requests failing

**Log mein:**

``` text
IJ000453: Unable to get managed connection
IJ000655: No managed connections available
```

**Thread Dump mein:**

``` text
at org.jboss.jca.core.connectionmanager
.pool.AbstractPool.getConnection()
```

**Problem:** Application ko DB connection nahi mil raha.

**Common reason:** Connection close nahi kiya gaya, ya slow SQL ki wajah
se connections occupied hain.

**Resolution:** - Active/available DB connections check karo. -
Connection leaks investigate karo. - Slow SQL aur long-running
transactions check karo. - Code mein proper connection closing ensure
karo.

## 4. High CPU Utilization

Application running slowly

**Log mein:**

``` text
WARN: High CPU utilization detected
```

**Thread Dump mein:**

``` text
"worker-thread-5" RUNNABLE

at com.cims.service.MessageProcessor.process()
at com.cims.service.MessageProcessor.retry()
```

**Problem:** Thread continuously processing ya retry loop mein ho sakta
hai.

**Resolution:** - top -H -p PID se high CPU thread identify karo. -
Thread ID ko hexadecimal mein convert karke dump mein nid match karo. -
Repeated stack traces compare karo. - Infinite loop ya excessive retry
ho to code fix, retry limit aur backoff implement karo.

## 5. External API Hanging

API response delayed

**Log mein:**

``` text
ERROR: Read timed out
java.net.SocketTimeoutException: Read timed out
```

**Thread Dump mein:**

``` text
"worker-thread-8" RUNNABLE

at java.net.SocketInputStream.socketRead0()
at java.net.SocketInputStream.read()
```

**Problem:** Application external API/server se response ka wait kar
rahi hai.

**Resolution:** - External API health check karo. - Network connectivity
verify karo. - API response time check karo. - Proper connection/read
timeout implement karo. - Uncontrolled retries avoid karo.

------------------------------------------------------------------------

# Part 2: Heap Dump -- 5 real production examples

Heap Dump mainly memory consumption, OutOfMemoryError aur memory leaks
investigate karne ke liye hota hai.

## 1. Database Connection Not Closed

Resource leak

**Log mein:**

``` text
WARN: IJ000453: Unable to get managed connection
ERROR: Connection pool exhausted
```

**Heap Dump mein:**

``` text
Class Name                 Objects

ConnectionImpl             500
StatementImpl              800
ResultSetImpl              700
```

**Problem:** Connections, statements ya related resources unnecessarily
retained ho sakte hain.

**Resolution:** - Connection leaks investigate karo. -
try-with-resources implement karo. - Statements aur ResultSets properly
close karo. - Connection pool monitor karo.

**Note:** Sirf connection close na hone ka proof Heap Dump se nahi
milta; pool metrics aur leak detection bhi check karte hain.

## 2. Static List Continuously Growing

Memory leak

**Log mein:**

``` text
java.lang.OutOfMemoryError: Java heap space
```

**Heap Dump mein:**

``` text
Class Name          Objects

User                500000
String              800000
ArrayList           1
```

**Problem:** Application ki static List mein continuously User objects
add ho rahe hain aur remove nahi ho rahe.

**Resolution:** - Unnecessary objects remove karo. - Collection size
limit karo. - Object references release karo.

## 3. Cache Expiry Missing

**Log mein:**

``` text
WARN: High heap memory utilization
ERROR: GC overhead limit exceeded
```

**Heap Dump mein:**

``` text
Class Name          Objects

HashMap             1
CacheEntry          500000
String              900000
```

**Problem:** Cache mein old data continuously store ho raha hai. Expiry
configured nahi hai.

**Resolution:** - Cache TTL configure karo. - Maximum cache size set
karo. - Eviction mechanism implement karo.

## 4. ExecutorService Not Shutdown

**Log mein:**

``` text
WARN: Application shutdown timed out
ERROR: Failed to terminate executor threads
```

**Heap Dump mein:**

``` text
Class Name              Objects

Thread                  200
ThreadPoolExecutor      10
Runnable                500
```

**Problem:** ExecutorService properly shutdown nahi hua aur
tasks/threads unnecessarily alive reh sakte hain.

**Resolution:** - shutdown() implement karo. - awaitTermination() use
karo. - Zarurat par shutdownNow() use karo. - Pending tasks aur thread
lifecycle check karo.

**Note:** Is issue ko confirm karne ke liye Thread Dump bhi important
hai.

## 5. Large Files Loaded Into Memory

OutOfMemoryError

**Log mein:**

``` text
java.lang.OutOfMemoryError: Java heap space

at java.util.Arrays.copyOf()
at java.io.ByteArrayOutputStream.toByteArray()
```

**Heap Dump mein:**

``` text
Class Name          Objects       Memory

byte[]              50000         900 MB
String              80000          50 MB
ArrayList            5000          20 MB
```

**Problem:** Application large files ko completely memory mein load kar
rahi hai.

**Resolution:** - Streaming approach use karo. - File chunks mein
process karo. - File size validation implement karo. - Concurrent file
processing limit karo.

------------------------------------------------------------------------

# Interview ke liye quick revision

## Thread Dump -- Problem \| Kya check karna hai?

  Thread Dump -- Problem   Kya check karna hai?
  ------------------------ ---------------------------------
  Deadlock                 BLOCKED threads, lock ownership
  Thread pool exhaustion   Worker threads, WAITING/BLOCKED
  DB pool exhaustion       Connection acquisition wait
  High CPU                 RUNNABLE thread, CPU usage
  External API hang        Socket read, timeout

## Heap Dump -- Problem \| Kya check karna hai?

  Heap Dump -- Problem   Kya check karna hai?
  ---------------------- ------------------------------------------
  Connection leak        Connection objects and pool metrics
  Static List            Excessive objects retained by collection
  Cache leak             HashMap/cache entries
  Executor leak          Thread and task references
  Large files            Huge byte arrays

## 3 commands jo interview mein yaad rakhne hain

**Thread Dump**

``` bash
jstack -l PID > threaddump.txt
```

**Heap Dump**

``` bash
jcmd PID GC.heap_dump /tmp/heapdump.hprof
```

**Java Process**

``` bash
jps -lv
```
