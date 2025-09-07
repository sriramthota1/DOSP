# Project 1: Sums of Consecutive Squares

## � Overview
This project solves the problem of finding sequences of consecutive squares that sum to a perfect square.  
The program is implemented in **[Gleam](https://gleam.run/)** using the **Actor Model**, ensuring parallel computation across multiple CPU cores.

Given two inputs:
- **N** → The maximum starting number for sequences.
- **k** → The length of the sequence.

The program outputs all valid starting numbers `a` such that:

\[
a^2 + (a+1)^2 + ... + (a+k-1)^2 = m^2
\]

for some integer \(m\).

---

##  Running the Program

### Build & Run
```bash
gleam deps download
gleam build
gleam run -- <N> <k>
````

### Example 1

```bash
gleam run -- 3 2
```

**Output:**

```
3
```

Explanation: $3^2 + 4^2 = 5^2$.

---

### Example 2

```bash
gleam run -- 40 24
```

**Output:**

```
1
9
20
25
```

Explanation:

* $1^2 + \dots + 24^2 = 70^2$
* $9^2 + \dots + 32^2 = 106^2$
* $20^2 + \dots + 43^2 = 158^2$
* $25^2 + \dots + 48^2 = 182^2$

---

## ⚙️ Work Unit Size

* **Chosen Work Unit Size:** **1000 subproblems per worker**.
* **Reasoning:**

  * Smaller units (100–500) caused frequent actor communication overhead.
  * Larger units (>5000) left some workers idle while others processed big chunks.
  * **1000** gave the best balance between load distribution and communication cost, maximizing CPU utilization on my machine.

---

## Performance Results

### Run: `gleam run -- 1000000 4`

* **Real Time:** *\[fill in your measured value]*
* **CPU Time:** *\[fill in your measured value]*
* **CPU Time / Real Time Ratio:** *\[fill in your measured ratio]*

-> A ratio significantly greater than **1** means effective multi-core parallelism.
-> A ratio close to **1** means poor parallelism.

> Please note: these values depend on the machine and number of cores available.

---

##  Largest Problem Solved

The largest instance successfully solved was:

```bash
gleam run -- [your_max_N] [your_max_k]
```

* Example: `gleam run -- 10000000 10` (replace with your actual maximum run).
* This demonstrates scalability of the actor-based approach.
* The limit depends on available memory, CPU, and runtime constraints.

---

##  Implementation Notes

- Implemented **exclusively** with Gleam **actors**.  
- **Boss actor** distributes ranges of starting indices.  
- **Worker actors** compute sums and check for perfect squares.  
- Results collected by the boss and printed in ascending order.  

##  Mathematical Optimization

Inline sum-of-squares formula: $S(n)=\frac{n(n+1)(2n+1)}{6}$.

Block form (recommended for readability):

$$
S(n)=\frac{n(n+1)(2n+1)}{6}
$$

For $k$ consecutive squares starting at $a$:

$$
\sum_{i=0}^{k-1} (a+i)^2 \;=\; S(a+k-1) - S(a-1)
$$

Expanded to an $O(1)$ computation (no inner loop):

$$
k\,a^2 \;+\; a\,k(k-1) \;+\; \frac{k(k-1)(2k-1)}{6}
$$

### Perfect Square Check
1. Compute the sum $T$ using the formula above.  
2. Let $r=\lfloor\sqrt{T}\rfloor$.  
3. It’s a hit iff $r^2=T$.

This reduces the computation from **O(k)** per candidate to **O(1)**, which is critical for large inputs.



## 📂 Project Structure

```
/src
   main.gleam        # entry point (parses args, starts boss)
/src/boss.gleam      # boss actor (distributes work)
/src/worker.gleam    # worker actor (computes sums)
/src/messages.gleam  # actor message types
/README.md           # this file
/gleam.toml          # dependencies
```

---

##  Bonus (Optional)

* Extend to **remote actors** to run across multiple machines.
* Enables solving very large instances such as:

```bash
gleam run -- 100000000 20
```

* Record a short **video demo** explaining your setup and speedup.

---

##  Author

* Your Name
* Distributed Operating Systems Principles — Fall 2025

```

```
