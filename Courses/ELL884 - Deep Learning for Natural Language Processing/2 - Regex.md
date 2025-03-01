
## Basic Syntax

### Anchors
| Symbol | Description           | Example   |
|--------|-----------------------|-----------|
| `^`    | Start of string/line  | `^Hello`  |
| `$`    | End of string/line    | `World$`  |

### Quantifiers
| Symbol  | Description                     | Example  |
| ------- | ------------------------------- | -------- |
| `*`     | 0 or more repetitions           | `a*`     |
| `+`     | 1 or more repetitions           | `a+`     |
| `?`     | 0 or 1 repetition               | `a?`     |
| `{n}`   | Exactly `n` repetitions         | `a{3}`   |
| `{n,}`  | `n` or more repetitions         | `a{2,}`  |
| `{n,m}` | Between `n` and `m` repetitions | `a{2,4}` |
| `*?`    | Lazy (non-greedy) quantifier    | `a*?`    |

### Character Classes
| Symbol   | Description                    | Example   |
| -------- | ------------------------------ | --------- |
| `[abc]`  | Any of `a`, `b`, or `c`        | `[aeiou]` |
| `[^abc]` | Negation (not `a`, `b`, `c`)   | `[^0-9]`  |
| `\d`     | Digit: `[0-9]`                 | `\d{3}`   |
| `\D`     | Non-digit: `[^0-9]`            | `\D+`     |
| `\w`     | Word character: `[a-zA-Z0-9_]` | `\w+`     |
| `\W`     | Non-word character             | `\W+`     |
| `\s`     | Whitespace                     | `\s+`     |
| `\S`     | Non-whitespace                 | `\S+`     |
| `.`      | Any character (except newline) | `a.c`     |

### Special Characters
| Symbol | Description           | Escape with |
|--------|-----------------------|-------------|
| `.`    | Literal dot           | `\.`        |
| `+`    | Literal plus          | `\+`        |
| `*`    | Literal asterisk      | `\*`        |
| `?`    | Literal question mark | `\?`        |
| `()`   | Literal parentheses   | `\(\)`      |

---

## Groups & Lookarounds

### Capturing Groups
| Syntax      | Description                   | Example               |
|-------------|-------------------------------|-----------------------|
| `(pattern)` | Capture group                 | `(\d{3})-(\d{3})`    |
| `(?:pattern)`| Non-capturing group          | `(?:\d{3})`          |

### Lookarounds
| Syntax       | Description                   | Example               |
|--------------|-------------------------------|-----------------------|
| `(?=pattern)`| Positive lookahead            | `a(?=b)`              |
| `(?!pattern)`| Negative lookahead            | `a(?!b)`              |
| `(?<=pattern)`| Positive lookbehind           | `(?<=a)b`             |
| `(?<!pattern)`| Negative lookbehind           | `(?<!a)b`             |

---

## Flags
| Flag | Description                   | Example       |
|------|-------------------------------|---------------|
| `i`  | Case-insensitive              | `/hello/i`    |
| `g`  | Global search                 | `/a/g`        |
| `m`  | Multiline mode                | `/^start/m`   |
| `s`  | Dotall (`.` matches newline)  | `/a.b/s`      |

---

## Common Patterns

| Pattern      | Example Regex                          |
|--------------|----------------------------------------|
| Email        | `^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$` |
| URL          | `https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)` |
| Date (YYYY-MM-DD) | `^\d{4}-\d{2}-\d{2}$`          |
| Phone (US)   | `^\+1-\d{3}-\d{3}-\d{4}$`             |

---

## Example Questions & Solutions

### Example 1: Extracting Date Components
**Question**:  
Write a regex to capture the year, month, and day from a `YYYY-MM-DD` date string.

**Solution**:  
```regex
^(\d{4})-(\d{2})-(\d{2})$
```

**Explanation**:  
- `^` and `$` ensure the entire string is matched.
- `(\d{4})` captures 4 digits for the year.
- `(\d{2})` captures 2 digits for the month and day.

---

### Example 2: Parsing Log Entries
**Question**:  
Parse a log entry in the format `[2023-10-05 14:30:00] ERROR: File not found` into timestamp, log level, and message.

**Solution**:  
```regex
^\[(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2})\] (\w+): (.+)$
```

**Explanation**:  
- `\[...\]` matches the timestamp block. `\d{4}-\d{2}-\d{2}` captures the date, and `\d{2}:\d{2}:\d{2}` captures the time.
- `(\w+)` captures the log level (e.g., ERROR).
- `(.+)` captures the remaining text as the message.

---

### Example 3: Email Username & Domain
**Question**:  
Extract the username and domain from an email (e.g., `user@example.com`).

**Solution**:  
```regex
^([a-zA-Z0-9._%+-]+)@([a-zA-Z0-9.-]+\.[a-zA-Z]{2,})$
```

**Explanation**:  
- `([a-zA-Z0-9._%+-]+)` captures the username (letters, numbers, and special characters).
- `@` matches the literal `@`.
- `([a-zA-Z0-9.-]+\.[a-zA-Z]{2,})` captures the domain (e.g., `example.com`).