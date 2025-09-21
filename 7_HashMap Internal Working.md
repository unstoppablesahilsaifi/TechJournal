
# 🔥 HashMap Internal Working


## 1️⃣ Basic structure

- **HashMap key-value pairs store karta hai.**
- Internal structure: **Array + LinkedList + Balanced Tree (Red-Black Tree)**.

Matlab:

- Pehle ek **array of buckets** hota hai (default size = 16).
- Har bucket ek **LinkedList** ya **Tree** ke head ko point karta hai.
- Bucket decide hota hai **hash(key)** se.

---

## 2️⃣ Jab hum `put(key, value)` karte hain

Example:

```java
map.put("Sahil", 101);
````

### Steps:

1. **Hash calculation**

   * Pehle key ka `hashCode()` call hota hai.
   * Phir HashMap us hash ko aur optimize karta hai (XOR shift) taaki **collision kam ho**.

   Example:

   ```java
   int hash = key.hashCode() ^ (key.hashCode() >>> 16);
   ```

2. **Index calculation**

   * Index nikalta hai:

     ```java
     index = hash % capacity   // capacity = array size (default 16)
     ```

   * Matlab key `"Sahil"` bucket number maan le 5 pe jaayegi.

3. **Check bucket**

   * Agar bucket empty hai → direct **Node store** kar do. ✅

4. **Collision case** (same bucket me already element hai)

   * Compare existing node ke **hash** aur **equals()** se:

     * Agar same key hai → value **update** kar do.
     * Agar different key hai → LinkedList me node append kar do.

5. **Treeify (Java 8 improvement)**

   * Agar ek bucket me LinkedList ki length **> 8** ho jaati hai aur array size ≥ 64 hai →
     LinkedList ko **Red-Black Tree** me convert kar do for faster search (O(log n) instead of O(n)).

---

## 3️⃣ Jab hum `get(key)` karte hain

Example:

```java
map.get("Sahil");
```

### Steps:

1. **Hash + Index calculation** (same as put).
2. Bucket pe jao → check nodes.
3. Agar ek hi node hai → return value.
4. Agar multiple nodes hain (collision case):

   * Compare `hash` aur `equals()` se key find karo.
   * Agar Tree hai → O(log n) search.
   * Agar LinkedList hai → O(n) traversal.

---

## 4️⃣ Resizing (Rehashing)

* Default capacity = 16, Load Factor = 0.75.
* Jab size > (capacity \* loadFactor) ho jaata hai → resizing hota hai.

👉 Example:

* Capacity = 16, Load factor = 0.75 → threshold = 12.
* 13th entry dalte hi array size **double** ho jaata hai (16 → 32).
* Aur saare nodes ko **rehash** karke nayi array me redistribute karna padta hai.

---

## 5️⃣ Complexity

* **Best case (no collision, good hash):** O(1) for put/get.
* **Worst case (poor hash, sab ek hi bucket me):** O(n).
* **Java 8 ke baad:** worst case improve → O(log n) (Tree).

---

# 🔹 Interview Story Mode Answer

*HashMap internally ek array of buckets maintain karta hai. Har bucket ek LinkedList ya Red-Black Tree ko point karta hai. Jab hum put karte hain, key ka hashCode calculate hota hai, usse bucket index decide hota hai. Agar bucket empty hai to node store hoti hai, agar collision hai to equals() se check karke ya to value update hoti hai ya list/tree me insert hoti hai. Java 8 ke baad agar ek bucket me nodes > 8 ho jaaye to wo list tree me convert ho jaati hai for O(log n) lookup. Get operation bhi same tarike se hash se bucket find karta hai aur value return karta hai. Aur jab size load factor (default 0.75) cross kar jaata hai, to HashMap apne array ka size double karke rehash karta hai. Best case O(1), worst case O(log n) after Java 8."*

---

# 🔥 More Simple way to understand:

**"HashMap = Array + LinkedList + Tree, with hashing for index, collision handling by chaining/tree, resizing by doubling, and O(1) average time."**


---

# 🔹 Easy Dry Run: HashMap put & get

Maan le:

```java
HashMap<String, Integer> map = new HashMap<>();
map.put("A", 1);
map.put("B", 2);
map.put("Aa", 3);
map.put("BB", 4);
```

---

## Step 1: `map.put("A", 1)`

1. `"A".hashCode()` nikalta hai → maan le `65`.
2. Array size = 16 (default).
   Index = `65 % 16 = 1`.
3. Bucket\[1] empty hai → `(A,1)` store kar do.

```
[0] -> null
[1] -> (A=1)
[2] -> null
...
```

---

## Step 2: `map.put("B", 2)`

1. `"B".hashCode()` = 66.
2. Index = `66 % 16 = 2`.
3. Bucket\[2] empty → `(B,2)` store ho gaya.

```
[1] -> (A=1)
[2] -> (B=2)
```

---

## Step 3: `map.put("Aa", 3)`

1. `"Aa".hashCode()` → maan le 2112.
2. Index = `2112 % 16 = 0`.
3. Bucket\[0] empty → `(Aa,3)` store ho gaya.

```
[0] -> (Aa=3)
[1] -> (A=1)
[2] -> (B=2)
```

---

## Step 4: `map.put("BB", 4)`

⚠️ Yeh interesting hai! `"Aa"` aur `"BB"` ka hashCode **same hota hai = 2112** (Java ke famous interview trick).

1. `"BB".hashCode()` = 2112.
2. Index = `2112 % 16 = 0`.
3. Bucket\[0] already occupied by `(Aa,3)` → **collision**.

   * Compare keys: `"Aa".equals("BB")` → false.
   * Isliye `(BB,4)` linked list me add ho gaya.

```
[0] -> (Aa=3) -> (BB=4)
[1] -> (A=1)
[2] -> (B=2)
```

---

## Step 5: `map.get("BB")`

1. `"BB".hashCode() = 2112`.
2. Index = `2112 % 16 = 0`.
3. Bucket\[0] traverse karo:

   * First node = `"Aa"` → not equal.
   * Second node = `"BB"` → match → value `4` return. ✅

---

# 🔹 Key Takeaways (Easy Version)

1. **HashCode** → Index decide karta hai.
2. **Collision** hone par → LinkedList ban jaata hai.
3. Java 8 se agar LinkedList length > 8 → Treeify ho jaata hai (fast search ke liye).
4. Load Factor 0.75 cross hone par → capacity double aur rehash hota hai.

---

# 🔥 Super Short Interview Line

*"HashMap internally ek array of buckets hai. Key ka hashCode index decide karta hai. Collision hone par list ya tree banta hai. Load factor cross hone par resize hota hai. Average O(1) hai, worst case O(log n) after Java 8."*



---

