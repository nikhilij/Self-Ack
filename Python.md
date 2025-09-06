# Complete Python Standard Library Modules

A **comprehensive, categorized, and numbered** reference of Python's standard library modules.  
**Tip:** Use `import this` for The Zen of Python!

---

## 1. Core & Built-in Modules

1. **`builtins`** — Built-in functions and exceptions (always available)
2. **`sys`** — System-specific parameters and functions
3. **`os`** — Operating system interfaces (file, process, and environment handling)
4. **`io`** — Core tools for working with streams (text, binary I/O)
5. **`time`** — Time access and conversions (sleep, timestamps)
6. **`datetime`** — Basic date and time types (date, time, timedelta)
7. **`calendar`** — Calendar-related functions (e.g., month calendars)
8. **`collections`** — Specialized container datatypes (`deque`, `Counter`, etc.)
9. **`collections.abc`** — Abstract Base Classes for containers (e.g., `Iterable`)
10. **`types`** — Dynamic type creation, names for built-in types
11. **`copy`** — Shallow and deep copy operations (`copy.copy()`, `copy.deepcopy()`)
12. **`pprint`** — Pretty-print data structures (great for debugging)
13. **`reprlib`** — Alternate `repr()` implementation for large/recursive objects
14. **`enum`** *(Python 3.4+)* — Support for enumerations (`Enum`, `IntEnum`)
15. **`inspect`** — Inspect live objects (introspection, signature info)
16. **`traceback`** — Print or retrieve stack tracebacks (exception handling)

---

## 2. Data Types & Structures

1. **`array`** — Efficient arrays of numeric values (compact, C-style arrays)
2. **`queue`** — Synchronized, thread-safe queue classes (`Queue`, `LifoQueue`)
3. **`struct`** — Interpret bytes as packed binary data (for binary protocols)
4. **`weakref`** — Weak references (memory-efficient data structures)
5. **`functools`** — Higher-order functions, function tools (`lru_cache`, `partial`)
6. **`operator`** — Standard operators as functions (e.g., `operator.add`)
7. **`itertools`** — Iterator building blocks (efficient looping, combinatorics)
8. **`contextlib`** — Utilities for `with`-statement context managers
9. **`atexit`** — Register exit handlers for program cleanup
10. **`abc`** — Abstract Base Classes (for custom class hierarchies)

---

## 3. Mathematics & Numerics

1. **`math`** — Basic mathematical functions (`sqrt`, `sin`, constants)
2. **`cmath`** — Math functions for complex numbers
3. **`decimal`** — Decimal fixed point and floating point arithmetic
4. **`fractions`** — Rational numbers (`Fraction` class)
5. **`random`** — Generate pseudo-random numbers
6. **`statistics`** *(Python 3.4+)* — Math statistics functions (mean, median)
7. **`numbers`** — Numeric abstract base classes (for custom numeric types)

---

## 4. File & Directory Access

1. **`pathlib`** *(Python 3.4+)* — Object-oriented filesystem paths
2. **`os.path`** — Common pathname manipulations
3. **`fileinput`** — Iterate over lines from multiple input streams
4. **`stat`** — Interpret `stat()` results (file info)
5. **`filecmp`** — Compare files and directories
6. **`tempfile`** — Generate temporary files and directories
7. **`glob`** — Unix-style pathname pattern expansion
8. **`fnmatch`** — Unix filename pattern matching
9. **`linecache`** — Random access to lines in text files
10. **`shutil`** — High-level file operations (copy, move, remove)
11. **`pickle`** — Python object serialization (not secure for untrusted data)
12. **`shelve`** — Simple object persistence (dictionary-like)
13. **`marshal`** — Internal Python object serialization (not for general use)
14. **`dbm`** — Interfaces to Unix "databases" (`dbm`, `gdbm`)
15. **`sqlite3`** — DB-API 2.0 interface for SQLite databases

---

## 5. Data Persistence & Exchange

1. **`json`** — JSON encoder and decoder
2. **`plistlib`** — Generate/parse Mac OS X `.plist` files
3. **`csv`** — CSV file reading and writing
4. **`xml.etree.ElementTree`** — The ElementTree XML API (easy XML parsing)
5. **`xml.dom`** — The Document Object Model API (tree-based XML parsing)
6. **`xml.sax`** — Support for SAX2 XML parsers (streaming XML)
7. **`xml.parsers.expat`** — Fast XML parsing using Expat

---

## 6. Data Compression & Archiving

1. **`zlib`** — Compression compatible with gzip
2. **`gzip`** — Support for gzip files
3. **`bz2`** — bzip2 compression support
4. **`lzma`** *(Python 3.3+)* — Compression using LZMA algorithm
5. **`zipfile`** — Work with ZIP archives
6. **`tarfile`** — Read/write tar archive files

---

## 7. Cryptography & Security

1. **`hashlib`** — Secure hashes and message digests (`sha256`, `md5`, etc.)
2. **`hmac`** — Keyed-Hashing for message authentication
3. **`secrets`** *(Python 3.6+)* — Generate secure random numbers, tokens (for security)
4. **`ssl`** — TLS/SSL wrapper for socket objects

---

## 8. Networking & Internet

1. **`socket`** — Low-level networking interface
2. **`ssl`** — TLS/SSL wrapper for socket objects
3. **`asyncio`** *(Python 3.4+)* — Asynchronous I/O, event loop, coroutines
4. **`select`** — Wait for I/O completion (select, poll, epoll)
5. **`selectors`** *(Python 3.4+)* — High-level I/O multiplexing
6. **`email`** — Email and MIME handling
7. **`mailbox`** — Manipulate mailboxes (mbox, Maildir, etc.)
8. **`mimetypes`** — Map filenames to MIME types
9. **`base64`** — Base16, Base32, Base64, Base85 data encodings
10. **`binascii`** — Convert between binary and ASCII
11. **`quopri`** — Encode/decode MIME quoted-printable data
12. **`uu`** — Encode/decode uuencoded files
13. **`html`** — HyperText Markup Language support
14. **`html.parser`** — Simple HTML/XHTML parser
15. **`http`** — HTTP modules (server, client, cookies, etc.)
16. **`http.client`** — HTTP protocol client
17. **`ftplib`** — FTP protocol client
18. **`poplib`** — POP3 protocol client
19. **`imaplib`** — IMAP4 protocol client
20. **`nntplib`** — NNTP protocol client
21. **`smtplib`** — SMTP protocol client
22. **`smtpd`** — SMTP server
23. **`telnetlib`** — Telnet client
24. **`urllib`** — URL handling modules  
    1. **`urllib.request`** — Open and fetch URLs  
    2. **`urllib.response`** — Response classes used by urllib  
    3. **`urllib.parse`** — Parse URLs into components  
    4. **`urllib.error`** — Exception classes raised by urllib.request  
    5. **`urllib.robotparser`** — Parser for `robots.txt`
25. **`xmlrpc`** — XML-RPC server and client modules
26. **`ipaddress`** *(Python 3.3+)* — IPv4/IPv6 manipulation library
27. **`webbrowser`** — Open URLs in a web browser
28. **`cgi`** — Common Gateway Interface support
29. **`cgitb`** — Traceback manager for CGI scripts
30. **`wsgiref`** — WSGI utilities and reference implementation
31. **`http.server`** — Basic HTTP servers (great for quick local testing)
32. **`http.cookies`** — HTTP state management (cookies)
33. **`http.cookiejar`** — Cookie handling for HTTP clients
34. **`socketserver`** — Framework for network servers

---

## 9. Concurrent Execution

1. **`threading`** — Thread-based parallelism
2. **`multiprocessing`** — Process-based parallelism
3. **`concurrent.futures`** *(Python 3.2+)* — High-level interface for launching parallel tasks
4. **`subprocess`** — Subprocess management (run other programs)
5. **`sched`** — Event scheduler
6. **`queue`** — Synchronized queue class (thread-safe)
7. **`_thread`** — Low-level threading API

---

## 10. Development & Debugging Tools

1. **`typing`** *(Python 3.5+)* — Type hints and static typing
2. **`pdb`** — Interactive debugger
3. **`profile`**, **`cProfile`** — Profilers (Python and C-based)
4. **`timeit`** — Measure execution time of small code snippets
5. **`trace`** — Trace or track Python statement execution
6. **`tracemalloc`** *(Python 3.4+)* — Trace memory allocations
7. **`doctest`** — Test interactive Python examples
8. **`unittest`** — Unit testing framework
9. **`unittest.mock`** *(Python 3.3+)* — Mock object library
10. **`test`** — Regression test package for Python
11. **`venv`** *(Python 3.3+)* — Virtual environment creation
12. **`ensurepip`** *(Python 3.4+)* — Bootstrapping the `pip` installer
13. **`zipapp`** *(Python 3.5+)* — Manage executable Python zip archives
14. **`sysconfig`** — Python’s configuration information
15. **`site`** — Site-specific configuration hook
16. **`distutils`**, **`setuptools`**, **`wheel`** — Building and installing Python packages

---

## 11. Runtime Services

1. **`atexit`** — Register exit handlers
2. **`gc`** — Garbage collector interface
3. **`sys`** — System-specific parameters and functions
4. **`warnings`** — Warning control
5. **`resource`** — Resource usage information
6. **`ctypes`** — Foreign function library for calling C code
7. **`mmap`** — Memory-mapped file support
8. **`readline`** — GNU readline interface (command history, completion)
9. **`rlcompleter`** — Completion function for GNU readline

---

## 12. Language Services

1. **`parser`** — Access Python parse trees
2. **`ast`** — Abstract Syntax Trees (for code analysis/transformation)
3. **`symtable`** — Access the compiler’s symbol tables
4. **`symbol`** — Constants used with Python parse trees
5. **`token`** — Constants used with Python parse trees
6. **`keyword`** — Test whether a string is a Python keyword
7. **`tokenize`** — Tokenizer for Python source
8. **`tabnanny`** — Detect ambiguous indentation
9. **`pyclbr`** — Python class browser support
10. **`py_compile`** — Compile Python source files
11. **`compileall`** — Byte-compile Python libraries
12. **`dis`** — Disassembler for Python bytecode
13. **`pickletools`** — Tools for pickle developers

---

## 13. Importing Modules

1. **`importlib`** — The implementation of import
2. **`imp`** — Access the import internals *(deprecated; use importlib)*
3. **`zipimport`** — Import modules from ZIP archives
4. **`pkgutil`** — Package extension utility
5. **`modulefinder`** — Find modules used by a script

---

## 14. Windows Specific

1. **`msilib`** — Read/write Microsoft Installer files
2. **`msvcrt`** — Useful routines from the MS VC++ runtime
3. **`winreg`** — Windows registry access
4. **`winsound`** — Sound-playing interface for Windows

---

## 15. Unix Specific

1. **`posix`** — POSIX system calls
2. **`pwd`** — The password database
3. **`spwd`** — The shadow password database
4. **`grp`** — The group database
5. **`crypt`** — Function to check Unix passwords
6. **`termios`** — POSIX style tty control
7. **`tty`** — Terminal control functions
8. **`pty`** — Pseudo-terminal utilities
9. **`fcntl`** — The `fcntl()` and `ioctl()` system calls
10. **`pipes`** — Interface to shell pipelines
11. **`resource`** — Resource usage information
12. **`nis`** — Interface to Sun’s NIS (Yellow Pages)
13. **`syslog`** — Unix syslog library routines

---

## 16. GUI & Multimedia

1. **`tkinter`** — Interface to Tcl/Tk for graphical user interfaces
2. **`turtle`** — Turtle graphics (great for teaching)
3. **`cmd`** — Line-oriented command interpreter support
4. **`shlex`** — Simple lexical analysis (split shell-like syntax)
5. **`wave`** — Read/write WAV files
6. **`colorsys`** — Color system conversions
7. **`audioop`** — Manipulate raw audio data
8. **`aifc`** — Read/write AIFF/AIFC files
9. **`sunau`** — Read/write Sun AU files
10. **`chunk`** — Read IFF chunked data

---

## 17. Internationalization

1. **`gettext`** — Multilingual internationalization services
2. **`locale`** — Internationalization services (number, date formats, etc.)

---

## 18. Miscellaneous

1. **`formatter`** — Generic output formatting
2. **`logging`** — Logging facility for Python
3. **`platform`** — Access to underlying platform’s identifying data
4. **`errno`** — Standard errno system symbols
5. **`ctypes`** — Foreign function library for Python
6. **`code`** — Interpreter base classes
7. **`codeop`** — Compile Python code
8. **`antigravity`** — Easter egg (import for The Zen of Python!)

---

## 19. Version-Specific Additions

### Python 3.8+
1. **`importlib.metadata`** — Access to package metadata
2. **`functools.cached_property`** — Cached property decorator

### Python 3.9+
3. **`zoneinfo`** — Time zone support
4. **`graphlib`** — Graph-like structure operations

### Python 3.10+
5. **`contextvars`** — Context variables (prominent since 3.10; introduced in 3.7)

---

**See also:**  
- [Python Standard Library Documentation](https://docs.python.org/3/library/)  
- [PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/)  
- Explore modules interactively with `help('modules')` in the Python REPL.

---
*This reference aims to be exhaustive yet approachable. Explore, experiment, and read the official docs for deeper understanding!*
