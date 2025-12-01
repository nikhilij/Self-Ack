# Deep Dive into C++ STL `std::string`

`std::string` is a standard library class for handling strings in C++, providing dynamic arrays of characters with various operations. It is defined in the `<string>` header and is essential for string manipulation in competitive programming and DSA.

This guide covers usage, operations, algorithms, and common competitive problems with heavily code examples and comments for explanation.

---

### Table of Contents
1.  [**Basics**](#basics)
    - [Header and Declaration](#1-header-and-declaration)
    - [Initialization and Assignment](#2-initialization-and-assignment)
    - [Access and Modification](#3-access-and-modification)
    - [Size and Capacity](#4-size-and-capacity)
2.  [**Intermediate**](#intermediate)
    - [String Operations](#1-string-operations)
    - [Searching and Finding](#2-searching-and-finding)
    - [Substrings and Splitting](#3-substrings-and-splitting)
3.  [**Advanced**](#advanced)
    - [String Algorithms](#1-string-algorithms)
    - [Performance Considerations](#2-performance-considerations)
    - [Common Pitfalls](#3-common-pitfalls)
4.  [**Examples**](#examples)
    - [Reverse String](#1-reverse-string)
    - [Check Palindrome](#2-check-palindrome)
    - [Anagram Check](#3-anagram-check)
    - [Longest Common Prefix](#4-longest-common-prefix)
    - [String to Integer (atoi)](#5-string-to-integer-atoi)
    - [Valid Parentheses](#6-valid-parentheses)
    - [Group Anagrams](#7-group-anagrams)
    - [Longest Substring Without Repeating Characters](#8-longest-substring-without-repeating-characters)
    - [Zigzag Conversion](#9-zigzag-conversion)
    - [Count and Say](#10-count-and-say)
5.  [**Summary**](#summary)
    - [Best Practices](#best-practices)
    - [Common Use Cases](#common-use-cases)

---

## Basics

### 1. Header and Declaration

```cpp
#include <string>  // Include the string header
#include <iostream> // For output

int main() {
    // Declare a string
    std::string s;

    // std::string is a typedef for std::basic_string<char>
    // It manages its own memory dynamically

    return 0;
}
```

### 2. Initialization and Assignment

```cpp
#include <string>
#include <iostream>

int main() {
    // Default constructor: empty string
    std::string s1;

    // Constructor with C-string
    std::string s2("Hello");

    // Constructor with char array and length
    std::string s3("Hello World", 5);  // "Hello"

    // Constructor with repeated character
    std::string s4(5, 'A');  // "AAAAA"

    // Copy constructor
    std::string s5 = s2;

    // Assignment
    s1 = "World";
    s1 = s2;  // Assign from another string

    // Move assignment (C++11)
    std::string s6 = std::move(s5);  // s5 is now empty

    std::cout << "s2: " << s2 << '\n';  // Hello
    std::cout << "s3: " << s3 << '\n';  // Hello
    std::cout << "s4: " << s4 << '\n';  // AAAAA

    return 0;
}
```

### 3. Access and Modification

```cpp
#include <string>
#include <iostream>

int main() {
    std::string s = "Hello";

    // Access with operator[]
    char ch = s[0];  // 'H'
    s[0] = 'h';      // "hello"

    // Access with at() - bounds checked
    try {
        ch = s.at(0);  // 'h'
        s.at(0) = 'H'; // "Hello"
    } catch (std::out_of_range& e) {
        std::cout << "Index out of range\n";
    }

    // Append
    s += " World";  // "Hello World"
    s.append("!");  // "Hello World!"
    s.push_back('!'); // "Hello World!!"

    // Insert
    s.insert(5, " C++");  // "Hello C++ World!!"

    // Erase
    s.erase(5, 4);  // Remove " C++", "Hello World!!"

    // Replace
    s.replace(6, 5, "Universe");  // "Hello Universe!"

    std::cout << s << '\n';

    return 0;
}
```

### 4. Size and Capacity

```cpp
#include <string>
#include <iostream>

int main() {
    std::string s = "Hello";

    // Size and length (same)
    std::cout << "Size: " << s.size() << '\n';     // 5
    std::cout << "Length: " << s.length() << '\n'; // 5

    // Capacity
    std::cout << "Capacity: " << s.capacity() << '\n';  // At least 5

    // Reserve capacity
    s.reserve(100);
    std::cout << "Capacity after reserve: " << s.capacity() << '\n';  // At least 100

    // Shrink to fit (C++11)
    s.shrink_to_fit();

    // Empty check
    std::cout << "Empty: " << s.empty() << '\n';  // false

    // Clear
    s.clear();
    std::cout << "Size after clear: " << s.size() << '\n';  // 0

    return 0;
}
```

## Intermediate

### 1. String Operations

```cpp
#include <string>
#include <iostream>
#include <algorithm>  // For std::reverse

int main() {
    std::string s1 = "Hello";
    std::string s2 = "World";

    // Concatenation
    std::string s3 = s1 + " " + s2;  // "Hello World"

    // Comparison
    if (s1 == "Hello") std::cout << "Equal\n";
    if (s1 < s2) std::cout << "s1 < s2\n";  // Lexicographical

    // Swap
    s1.swap(s2);  // s1 = "World", s2 = "Hello"

    // Reverse (using algorithm)
    std::reverse(s1.begin(), s1.end());  // "dlroW"

    // To upper/lower (manual)
    for (char& c : s1) c = std::toupper(c);  // "DLROW"

    std::cout << "s3: " << s3 << '\n';
    std::cout << "s1: " << s1 << '\n';

    return 0;
}
```

### 2. Searching and Finding

```cpp
#include <string>
#include <iostream>

int main() {
    std::string s = "Hello World Hello";

    // find: Find substring
    size_t pos = s.find("World");  // 6
    if (pos != std::string::npos) {
        std::cout << "Found at: " << pos << '\n';
    }

    // rfind: Find last occurrence
    pos = s.rfind("Hello");  // 12

    // find_first_of: Find first char from set
    pos = s.find_first_of("aeiou");  // 1 ('e')

    // find_last_of: Find last char from set
    pos = s.find_last_of("aeiou");  // 14 ('o')

    // find_first_not_of: Find first char not in set
    pos = s.find_first_not_of("Helo ");  // 6 ('W')

    // substr: Extract substring
    std::string sub = s.substr(6, 5);  // "World"

    std::cout << "Sub: " << sub << '\n';

    return 0;
}
```

### 3. Substrings and Splitting

```cpp
#include <string>
#include <iostream>
#include <vector>
#include <sstream>  // For stringstream

int main() {
    std::string s = "apple,banana,cherry";

    // substr
    std::string first = s.substr(0, 5);  // "apple"

    // Manual splitting by delimiter
    std::vector<std::string> tokens;
    size_t start = 0, end;
    while ((end = s.find(',', start)) != std::string::npos) {
        tokens.push_back(s.substr(start, end - start));
        start = end + 1;
    }
    tokens.push_back(s.substr(start));  // Last token

    // Using stringstream for splitting
    std::stringstream ss(s);
    std::string token;
    std::vector<std::string> tokens2;
    while (std::getline(ss, token, ',')) {
        tokens2.push_back(token);
    }

    for (const auto& t : tokens) {
        std::cout << t << ' ';
    }
    std::cout << '\n';

    return 0;
}
```

## Advanced

### 1. String Algorithms

```cpp
#include <string>
#include <iostream>
#include <algorithm>
#include <unordered_map>

// KMP Algorithm for pattern searching
std::vector<int> computeLPS(const std::string& pattern) {
    int m = pattern.size();
    std::vector<int> lps(m, 0);
    int len = 0;
    int i = 1;
    while (i < m) {
        if (pattern[i] == pattern[len]) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    return lps;
}

std::vector<int> KMPSearch(const std::string& text, const std::string& pattern) {
    std::vector<int> result;
    int n = text.size();
    int m = pattern.size();
    std::vector<int> lps = computeLPS(pattern);
    int i = 0, j = 0;
    while (i < n) {
        if (pattern[j] == text[i]) {
            i++;
            j++;
        }
        if (j == m) {
            result.push_back(i - j);
            j = lps[j - 1];
        } else if (i < n && pattern[j] != text[i]) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
    return result;
}

int main() {
    std::string text = "ABABDABACDABABCABAB";
    std::string pattern = "ABABCABAB";
    std::vector<int> positions = KMPSearch(text, pattern);
    for (int pos : positions) {
        std::cout << "Pattern found at: " << pos << '\n';
    }

    return 0;
}
```

### 2. Performance Considerations

```cpp
#include <string>
#include <iostream>
#include <chrono>

// std::string operations are generally O(n) for concatenation in loops
// Use reserve to avoid reallocations

int main() {
    std::string s;
    s.reserve(1000000);  // Preallocate

    auto start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < 100000; ++i) {
        s += "a";  // Efficient with reserve
    }
    auto end = std::chrono::high_resolution_clock::now();
    std::chrono::duration<double> time = end - start;
    std::cout << "Time: " << time.count() << "s\n";

    // For frequent modifications, consider std::stringstream or std::vector<char>

    return 0;
}
```

### 3. Common Pitfalls

```cpp
#include <string>
#include <iostream>

int main() {
    // Pitfall 1: Forgetting null terminator in C-strings
    const char* cstr = "Hello";
    std::string s(cstr);  // OK, but s.size() == 5, no null

    // Pitfall 2: operator[] no bounds check
    // s[100] = 'a';  // Undefined behavior if out of bounds

    // Pitfall 3: Concatenation in loops without reserve
    std::string bad;
    for (int i = 0; i < 10000; ++i) {
        bad += "x";  // Multiple reallocations
    }

    // Better: use reserve or std::stringstream

    // Pitfall 4: Comparing with C-strings
    if (s == "Hello") {}  // OK
    // if (s == cstr) {}  // May not work if cstr not null-terminated properly

    return 0;
}
```

## Examples

### 1. Reverse String

```cpp
#include <string>
#include <iostream>
#include <algorithm>

// LeetCode 344: Reverse String
void reverseString(std::vector<char>& s) {
    int left = 0, right = s.size() - 1;
    while (left < right) {
        std::swap(s[left], s[right]);
        left++;
        right--;
    }
}

int main() {
    std::vector<char> s = {'h', 'e', 'l', 'l', 'o'};
    reverseString(s);
    for (char c : s) std::cout << c;  // olleh
    std::cout << '\n';

    // Using std::reverse
    std::string str = "hello";
    std::reverse(str.begin(), str.end());
    std::cout << str << '\n';  // olleh

    return 0;
}
```

### 2. Check Palindrome

```cpp
#include <string>
#include <iostream>
#include <algorithm>

// LeetCode 125: Valid Palindrome
bool isPalindrome(std::string s) {
    // Remove non-alphanumeric and convert to lower
    std::string cleaned;
    for (char c : s) {
        if (std::isalnum(c)) {
            cleaned += std::tolower(c);
        }
    }

    // Check palindrome
    int left = 0, right = cleaned.size() - 1;
    while (left < right) {
        if (cleaned[left] != cleaned[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}

int main() {
    std::string s = "A man, a plan, a canal: Panama";
    std::cout << isPalindrome(s) << '\n';  // 1

    return 0;
}
```

### 3. Anagram Check

```cpp
#include <string>
#include <iostream>
#include <algorithm>

// LeetCode 242: Valid Anagram
bool isAnagram(std::string s, std::string t) {
    if (s.size() != t.size()) return false;

    std::sort(s.begin(), s.end());
    std::sort(t.begin(), t.end());

    return s == t;
}

// Using frequency count
bool isAnagramFreq(std::string s, std::string t) {
    if (s.size() != t.size()) return false;

    int count[26] = {0};
    for (char c : s) count[c - 'a']++;
    for (char c : t) count[c - 'a']--;

    for (int i : count) {
        if (i != 0) return false;
    }
    return true;
}

int main() {
    std::string s = "anagram", t = "nagaram";
    std::cout << isAnagram(s, t) << '\n';       // 1
    std::cout << isAnagramFreq(s, t) << '\n';   // 1

    return 0;
}
```

### 4. Longest Common Prefix

```cpp
#include <string>
#include <iostream>
#include <vector>

// LeetCode 14: Longest Common Prefix
std::string longestCommonPrefix(std::vector<std::string>& strs) {
    if (strs.empty()) return "";

    std::string prefix = strs[0];
    for (size_t i = 1; i < strs.size(); ++i) {
        while (strs[i].find(prefix) != 0) {
            prefix = prefix.substr(0, prefix.size() - 1);
            if (prefix.empty()) return "";
        }
    }
    return prefix;
}

int main() {
    std::vector<std::string> strs = {"flower", "flow", "flight"};
    std::cout << longestCommonPrefix(strs) << '\n';  // "fl"

    return 0;
}
```

### 5. String to Integer (atoi)

```cpp
#include <string>
#include <iostream>
#include <climits>

// LeetCode 8: String to Integer (atoi)
int myAtoi(std::string s) {
    int i = 0, n = s.size();
    long result = 0;
    int sign = 1;

    // Skip leading whitespaces
    while (i < n && s[i] == ' ') i++;

    // Check sign
    if (i < n && (s[i] == '+' || s[i] == '-')) {
        sign = (s[i] == '-') ? -1 : 1;
        i++;
    }

    // Convert digits
    while (i < n && std::isdigit(s[i])) {
        result = result * 10 + (s[i] - '0');
        if (result * sign > INT_MAX) return INT_MAX;
        if (result * sign < INT_MIN) return INT_MIN;
        i++;
    }

    return result * sign;
}

int main() {
    std::string s = "   -42";
    std::cout << myAtoi(s) << '\n';  // -42

    return 0;
}
```

### 6. Valid Parentheses

```cpp
#include <string>
#include <iostream>
#include <stack>

// LeetCode 20: Valid Parentheses
bool isValid(std::string s) {
    std::stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') {
            st.push(c);
        } else {
            if (st.empty()) return false;
            char top = st.top();
            st.pop();
            if ((c == ')' && top != '(') ||
                (c == '}' && top != '{') ||
                (c == ']' && top != '[')) {
                return false;
            }
        }
    }
    return st.empty();
}

int main() {
    std::string s = "({[]})";
    std::cout << isValid(s) << '\n';  // 1

    return 0;
}
```

### 7. Group Anagrams

```cpp
#include <string>
#include <iostream>
#include <vector>
#include <unordered_map>
#include <algorithm>

// LeetCode 49: Group Anagrams
std::vector<std::vector<std::string>> groupAnagrams(std::vector<std::string>& strs) {
    std::unordered_map<std::string, std::vector<std::string>> map;
    for (std::string s : strs) {
        std::string key = s;
        std::sort(key.begin(), key.end());
        map[key].push_back(s);
    }

    std::vector<std::vector<std::string>> result;
    for (auto& p : map) {
        result.push_back(p.second);
    }
    return result;
}

int main() {
    std::vector<std::string> strs = {"eat", "tea", "tan", "ate", "nat", "bat"};
    auto groups = groupAnagrams(strs);
    for (const auto& group : groups) {
        for (const std::string& s : group) {
            std::cout << s << ' ';
        }
        std::cout << '\n';
    }

    return 0;
}
```

### 8. Longest Substring Without Repeating Characters

```cpp
#include <string>
#include <iostream>
#include <unordered_set>

// LeetCode 3: Longest Substring Without Repeating Characters
int lengthOfLongestSubstring(std::string s) {
    std::unordered_set<char> set;
    int max_len = 0, left = 0;
    for (int right = 0; right < s.size(); ++right) {
        while (set.count(s[right])) {
            set.erase(s[left]);
            left++;
        }
        set.insert(s[right]);
        max_len = std::max(max_len, right - left + 1);
    }
    return max_len;
}

int main() {
    std::string s = "abcabcbb";
    std::cout << lengthOfLongestSubstring(s) << '\n';  // 3

    return 0;
}
```

### 9. Zigzag Conversion

```cpp
#include <string>
#include <iostream>
#include <vector>

// LeetCode 6: Zigzag Conversion
std::string convert(std::string s, int numRows) {
    if (numRows == 1) return s;

    std::vector<std::string> rows(numRows);
    int curRow = 0;
    bool goingDown = false;

    for (char c : s) {
        rows[curRow] += c;
        if (curRow == 0 || curRow == numRows - 1) {
            goingDown = !goingDown;
        }
        curRow += goingDown ? 1 : -1;
    }

    std::string result;
    for (const std::string& row : rows) {
        result += row;
    }
    return result;
}

int main() {
    std::string s = "PAYPALISHIRING";
    int numRows = 3;
    std::cout << convert(s, numRows) << '\n';  // PAHNAPLSIIGYIR

    return 0;
}
```

### 10. Count and Say

```cpp
#include <string>
#include <iostream>

// LeetCode 38: Count and Say
std::string countAndSay(int n) {
    if (n == 1) return "1";

    std::string prev = countAndSay(n - 1);
    std::string result;

    for (size_t i = 0; i < prev.size(); ) {
        char current = prev[i];
        int count = 0;
        while (i < prev.size() && prev[i] == current) {
            count++;
            i++;
        }
        result += std::to_string(count) + current;
    }

    return result;
}

int main() {
    int n = 4;
    std::cout << countAndSay(n) << '\n';  // 1211

    return 0;
}
```

## Summary

### Best Practices
- Use `reserve()` for large strings to avoid reallocations.
- Prefer `at()` over `[]` for bounds checking in debug.
- Use `std::string_view` (C++17) for read-only views to avoid copies.
- For complex parsing, consider `std::stringstream`.

### Common Use Cases
- Text processing and parsing.
- Competitive programming problems (palindromes, anagrams, etc.).
- Input/output handling.
- String matching and searching.