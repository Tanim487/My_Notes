# File I/O

Python can **read from** and **write to** files.

## File Types

| Type | Examples |
|------|---------|
| **Text files** | `.txt`, `.docx`, `.log` |
| **Binary files** | `.mp4`, `.mov`, `.png`, `.jpeg` |

---

## Opening, Reading & Closing

```python
f = open("file_name", "mode")
data = f.read()
f.close()
```

---

## File Modes

| Mode | Meaning |
|------|---------|
| `'r'` | Open for reading *(default)* |
| `'w'` | Open for writing — **truncates** (clears) the file first |
| `'x'` | Create new file and open for writing |
| `'a'` | Open for writing — **appends** to end if file exists |
| `'b'` | Binary mode |
| `'r+'` | Read and write — **no truncation** |
| `'w+'` | Read and write — **truncates** file |
| `'a+'` | Read and append — **no truncation** |

!!! note "Quick Rule"
    - `r+` → no truncate
    - `w+` → truncate
    - `a+` → no truncate

---

## Reading

```python
data = f.read()         # reads entire file
data = f.readline()     # reads one line at a time
```

---

## Writing

```python
# Overwrite entire file
f = open("demo.txt", "w")
f.write("this overwrites the entire file")

# Append to file
f = open("demo.txt", "a")
f.write("\nthis adds a new line to the file")
```

---

## `with` Syntax (Recommended)

Automatically closes the file — no need to call `f.close()`.

```python
with open("demo.txt", "a") as f:
    data = f.read()
```

---

## Deleting a File

Uses the `os` module — a built-in code library (module = file written by someone else that has functions you can use).

```python
import os
os.remove("filename.txt")
```
