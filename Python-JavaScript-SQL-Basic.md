# Programming Basics: Python, JavaScript & SQL (TryHackMe)

Notes from two TryHackMe rooms: a "guess the number" coding exercise built first in **Python**, then rebuilt in **JavaScript**, followed by a separate **SQL** fundamentals room ("Café SQL").

---

## Part 1 — Python: Number Guessing Game

### Core built-ins used

| Function | Purpose |
|---|---|
| `print()` | Displays text on screen |
| `input()` | Reads user input — **always returns a string**, even if the user types a number |
| `int()` | Converts a string to an integer |
| `random.randint(a, b)` | Returns a random integer between `a` and `b` (inclusive) |

### Control flow
- `if` / `elif` / `else` — checks conditions in order; only one branch runs.
- `while CONDITION:` — repeats the indented block **as long as** the condition is true. Used here because we don't know in advance how many guesses it will take (unlike a `for` loop, which is for a known number of repeats).

### Final program logic

```python
import random  # gives us tools for picking random numbers

secret = random.randint(1, 20)  # 1 <= secret <= 20
tries = 0
guess = 0  # start with a value that cannot be the secret (since secret is 1..20)

print("I'm thinking of a number between 1 and 20")

# Repeat until the user guesses the secret number.
while guess != secret:
    text = input("Take a guess: ")  # input() returns text (a string)
    guess = int(text)               # convert the text to a number

    tries = tries + 1  # add 1 try

    # Give a hint using if / elif / else.
    if guess < 1 or guess > 20:
        print("That number is out of range. Try again.")
    elif guess < secret:
        print("Too low, try again.")
    elif guess > secret:
        print("Too high, try again.")
    else:
        print("You got it in", tries, "tries!")
```

**Quick recall for exams:**
- Display output → `print`
- Text → number → `int`
- "else if" in Python → `elif`
- Unknown number of repeats → `while`
- A variable placed after a comma in `print()` is substituted with its value (e.g. `tries` → `3`)

---

## Part 2 — JavaScript: Same Game, Different Language

### Why `parseInt()` is needed
User input from `rl.question()` is **always a string**, even for numbers:

```js
const text = await rl.question("Take a guess: ");
// user types 15 → text = "15" (string, not number)
```

Without conversion, `"15" === 15` is `false` (string ≠ number), so comparisons and math silently fail. `parseInt(text, 10)` converts the string to a number — the second argument, `10`, tells JavaScript to read it as base‑10 (a normal decimal number).

### Final program logic

```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

const rl = readline.createInterface({ input, output });

try {
  const secret = Math.floor(Math.random() * 20) + 1; // 1 <= secret <= 20
  let tries = 0;
  let guess = 0; // start with a value that cannot be the secret (since secret is 1..20)

  console.log("I'm thinking of a number between 1 and 20");

  // Repeat until the user guesses the secret number.
  while (guess !== secret) {
    const text = await rl.question("Take a guess: "); // rl.question() returns text (a string)
    guess = parseInt(text, 10);                        // convert the text to a number

    tries = tries + 1; // add 1 try

    // Give a hint using if / else if / else.
    if (guess < 1 || guess > 20) {
      console.log("That number is out of range. Try again.");
    } else if (guess < secret) {
      console.log("Too low, try again.");
    } else if (guess > secret) {
      console.log("Too high, try again.");
    } else {
      console.log("You got it in", tries, "tries!");
    }
  }
} finally {
  rl.close();
}
```

### Python ↔ JavaScript quick mapping

| Concept | Python | JavaScript |
|---|---|---|
| Print output | `print()` | `console.log()` |
| Read input | `input()` | `await rl.question()` |
| String → number | `int(text)` | `parseInt(text, 10)` |
| "else if" | `elif` | `else if` |
| Logical OR | `or` | `\|\|` |
| Not equal | `!=` | `!==` |
| Random int 1–20 | `random.randint(1, 20)` | `Math.floor(Math.random() * 20) + 1` |

---

## Part 3 — SQL: Café SQL (Databases, Tables & Queries)

### Core concepts
- **Database** — an organised place where a computer stores information (like a notebook that never runs out of pages, and can be searched/sorted instantly).
- **Table** — resembles a spreadsheet; data is organised into rows and columns.
- **Column** — one type of information (a field), e.g. `price`.
- **Row** — one complete record, e.g. one café order.
- **Query** — a question asked of the database using SQL. Queries only *display* data; they don't change it.

Example schema used in the exercises:
```
Orders (id, drink, price, time)
Menu   (drink, price)
```

### SQL keywords learned

| Keyword | Purpose |
|---|---|
| `SELECT` | Choose which columns to display |
| `FROM` | Choose which table the data comes from |
| `WHERE` | Filter rows based on a condition |
| `ORDER BY` | Sort results (ascending by default; add `DESC` for descending) |

### Example queries

```sql
-- Show every column, every row
SELECT * FROM Orders;

-- Show only specific columns
SELECT drink, price FROM Orders;

-- Filter rows by a condition
SELECT * FROM Orders WHERE drink = 'Coffee';

-- Sort results (lowest price first, by default)
SELECT * FROM Orders ORDER BY price;

-- Sort in reverse order (highest first)
SELECT * FROM Orders ORDER BY price DESC;

-- Combine filtering + sorting
SELECT * FROM Orders WHERE drink = 'Coffee' ORDER BY price DESC;

-- Check what's on the menu
SELECT * FROM Menu;
```

### Skills learned
- Understanding what a database is
- Knowing what tables, rows, and columns are
- Asking simple questions using SQL
- Reading results returned by a database

