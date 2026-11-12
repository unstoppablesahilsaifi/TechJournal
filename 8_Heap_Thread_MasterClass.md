Part 1: Thread Dump – 5 real production examples
Thread Dump mainly tab lete hain jab application hang, slow ya unresponsive ho.

1. Deadlock
Application hang
Log mein:
ERROR: Thread deadlock detected
Found one Java-level deadlock:


Thread Dump mein:
Thread-1: BLOCKED
Waiting to lock Object-2
Holding Object-1

Thread-2: BLOCKED
Waiting to lock Object-1
Holding Object-2


Problem: Dono threads ek doosre ke lock ka wait kar rahe hain.

Resolution:
- Code mein lock ordering correct karna.
- Nested locks avoid karna.
- Permanent code fix deploy karna.
- Urgent recovery mein approved restart.
2. Thread Pool Exhaustion
Requests getting stuck
Log mein:
WARN: Task rejected from ThreadPoolExecutor
java.util.concurrent.RejectedExecutionException


Thread Dump mein:
"http-worker-1" WAITING
"http-worker-2" WAITING
"http-worker-3" WAITING

at java.util.concurrent.FutureTask.get()


Problem: Available worker threads busy hain aur naye requests process nahi ho paa rahe.

Resolution:
- Thread pool status check karo.
- Identify karo kaunse tasks threads ko occupy kar rahe hain.
- Blocking operations aur downstream delays investigate karo.
- Pool size tuning sirf analysis ke baad.
3. Database Connection Pool Exhaustion
Database requests failing
Log mein:
IJ000453: Unable to get managed connection
IJ000655: No managed connections available


Thread Dump mein:
at org.jboss.jca.core.connectionmanager
.pool.AbstractPool.getConnection()


Problem: Application ko DB connection nahi mil raha.

Common reason: Connection close nahi kiya gaya, ya slow SQL ki wajah se connections occupied hain.

Resolution:
- Active/available DB connections check karo.
- Connection leaks investigate karo.
- Slow SQL aur long-running transactions check karo.
- Code mein proper connection closing ensure karo.
4. High CPU Utilization
Application running slowly
Log mein:
WARN: High CPU utilization detected


Thread Dump mein:
"worker-thread-5" RUNNABLE

at com.cims.service.MessageProcessor.process()
at com.cims.service.MessageProcessor.retry()


Problem: Thread continuously processing ya retry loop mein ho sakta hai.

Resolution:
- top -H -p PID se high CPU thread identify karo.
- Thread ID ko hexadecimal mein convert karke dump mein nid match karo.
- Repeated stack traces compare karo.
- Infinite loop ya excessive retry ho to code fix, retry limit aur backoff implement karo.
5. External API Hanging
API response delayed
Log mein:
ERROR: Read timed out
java.net.SocketTimeoutException: Read timed out


Thread Dump mein:
"worker-thread-8" RUNNABLE

at java.net.SocketInputStream.socketRead0()
at java.net.SocketInputStream.read()


Problem: Application external API/server se response ka wait kar rahi hai.

Resolution:
- External API health check karo.
- Network connectivity verify karo.
- API response time check karo.
- Proper connection/read timeout implement karo.
- Uncontrolled retries avoid karo.
