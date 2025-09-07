
# Project 1: Sums of Consecutive Squares

## 📖 Overview
This project solves the problem of finding sequences of consecutive squares that sum to a perfect square.  
The program is implemented in **[Gleam](https://gleam.run/)** using the **Actor Model**, ensuring parallel computation across multiple CPU cores.

Given two inputs:
- **N** → The maximum starting number for sequences.
- **k** → The length of the sequence.

The program outputs all valid starting numbers `a` such that:

\[
a^2 + (a+1)^2 + \dots + (a+k-1)^2 = m^2
\]

for some integer \(m\).

---

## 🚀 Running the Program

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

* **Definition:** Number of sub-problems (starting indices) assigned to a worker per request.
* After testing different values, the best performance (on my machine) was obtained with **1000 subproblems per worker**.
* Tradeoff:

  * Smaller units → more communication overhead.
  * Larger units → risk of load imbalance.
* **1000** balanced parallelism and efficiency for my environment.

> **Note:** Your best value may vary by CPU core count and scheduler; try 200–5000 and compare.

---

## 📊 Performance Results

### Run: `gleam run -- 1000000 4`

* **Real Time:** *\[insert measured value]*
* **CPU Time / Real Time Ratio:** *\[insert measured ratio]*

**How I measured:**

* Linux/macOS:

  ```bash
  /usr/bin/time -p gleam run -- 1000000 4
  ```
* Windows (PowerShell):

  ```powershell
  Measure-Command { gleam run -- 1000000 4 }
  ```

**Interpretation:**

* A ratio significantly **> 1** indicates effective parallelism (multiple cores busy).
* A ratio close to **1** indicates low parallelism.

---

## 💪 Largest Problem Solved

```bash
gleam run -- [your_max_N] [your_max_k]
```

Replace with your actual tested values.
This demonstrates scalability of the actor-based design.

---

## 🛠 Implementation Notes

* Implemented **exclusively** with Gleam **actors**.
* **Boss actor** distributes subproblems (ranges of start values).
* **Worker actors** compute sums and check for perfect squares.
* Results are sent back to the boss and printed in ascending order.
* Sieve/Math helpers used to:

  * Compute range-sum-of-squares quickly using formulae.
  * Check perfect squares using integer sqrt.

---

## 🧪 Correctness Hints

* The sum of squares from 1 to $n$ is $S(n) = \frac{n(n+1)(2n+1)}{6}$.
* The sum of $k$ consecutive squares starting at $a$ is $S(a+k-1) - S(a-1)$.
* Check whether the result is a perfect square by integer square root and squaring back.

---

## 📂 Project Structure

```
/src/main.gleam        # entry point (parses args, starts boss)
/src/boss.gleam      # boss actor (work distribution, result aggregation)
/src/worker.gleam    # worker actor (computes range checks)
/src/messages.gleam  # actor message types
/README.md           # this file
/gleam.toml          # dependencies & project metadata
```

---

## 👤 Authors

* Nabeel Ahmed Mohammed - 33179038
* Sriram Thota - 
* Distributed Operating Systems Principles — Fall 2025

```
```
