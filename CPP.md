# Complete C++ STL Libraries List (Continuously Numbered)

A comprehensive, **continuously numbered** list of all the major components of the C++ Standard Template Library (STL), organized by category and including version notes.

---

## Containers Library

### Sequence Containers

1. **vector** — Dynamic array  
2. **deque** — Double-ended queue  
3. **list** — Doubly-linked list  
4. **forward_list** *(C++11)* — Singly-linked list  
5. **array** *(C++11)* — Fixed-size array  

### Associative Containers

6. **set** — Ordered unique keys  
7. **map** — Ordered key-value pairs  
8. **multiset** — Ordered multiple keys  
9. **multimap** — Ordered multiple key-value pairs  

### Unordered Associative Containers *(C++11)*

10. **unordered_set** — Hash-based set  
11. **unordered_map** — Hash-based map  
12. **unordered_multiset** — Hash-based multiset  
13. **unordered_multimap** — Hash-based multimap  

### Container Adaptors

14. **stack** — LIFO data structure  
15. **queue** — FIFO data structure  
16. **priority_queue** — Priority queue  

### Other

17. **bitset** — Fixed-size sequence of bits  
18. **valarray** — Numerical array with operations  
19. **pair** — Two-element tuple  
20. **tuple** *(C++11)* — Fixed-size collection of heterogeneous values  

---

## Algorithms Library

### Non-modifying Operations

21. **all_of / any_of / none_of** *(C++11)*  
22. **for_each**  
23. **count / count_if**  
24. **mismatch**  
25. **equal**  
26. **find / find_if / find_if_not** *(C++11)*  
27. **find_end**  
28. **find_first_of**  
29. **adjacent_find**  
30. **search**  
31. **search_n**  

### Modifying Operations

32. **copy / copy_if** *(C++11)*  
33. **copy_n** *(C++11)*  
34. **copy_backward**  
35. **move** *(C++11)*  
36. **move_backward** *(C++11)*  
37. **fill**  
38. **fill_n**  
39. **transform**  
40. **generate**  
41. **generate_n**  
42. **remove / remove_if**  
43. **remove_copy / remove_copy_if**  
44. **replace / replace_if**  
45. **replace_copy / replace_copy_if**  
46. **swap**  
47. **swap_ranges**  
48. **iter_swap**  
49. **reverse**  
50. **reverse_copy**  
51. **rotate**  
52. **rotate_copy**  
53. **random_shuffle / shuffle** *(C++11)*  
54. **unique**  
55. **unique_copy**  

### Partitioning Operations

56. **is_partitioned** *(C++11)*  
57. **partition**  
58. **partition_copy** *(C++11)*  
59. **stable_partition**  
60. **partition_point** *(C++11)*  

### Sorting Operations

61. **is_sorted** *(C++11)*  
62. **is_sorted_until** *(C++11)*  
63. **sort**  
64. **partial_sort**  
65. **partial_sort_copy**  
66. **stable_sort**  
67. **nth_element**  

### Binary Search Operations

68. **lower_bound**  
69. **upper_bound**  
70. **binary_search**  
71. **equal_range**  

### Set Operations

72. **merge**  
73. **inplace_merge**  
74. **includes**  
75. **set_difference**  
76. **set_intersection**  
77. **set_symmetric_difference**  
78. **set_union**  

### Heap Operations

79. **is_heap** *(C++11)*  
80. **is_heap_until** *(C++11)*  
81. **make_heap**  
82. **push_heap**  
83. **pop_heap**  
84. **sort_heap**  

### Minimum/Maximum Operations

85. **max**  
86. **max_element**  
87. **min**  
88. **min_element**  
89. **minmax** *(C++11)*  
90. **minmax_element** *(C++11)*  
91. **clamp** *(C++17)*  
92. **lexicographical_compare**  

### Comparison Operations

93. **equal**  
94. **lexicographical_compare**  

### Permutation Operations

95. **is_permutation** *(C++11)*  
96. **next_permutation**  
97. **prev_permutation**  

### Numeric Operations

98. **iota** *(C++11)*  
99. **accumulate**  
100. **inner_product**  
101. **adjacent_difference**  
102. **partial_sum**  

---

## Iterators Library

103. **iterator** — Base iterator  
104. **reverse_iterator** — Reverse iterator  
105. **move_iterator** *(C++11)* — Move iterator  
106. **back_insert_iterator** — Iterator for push_back  
107. **front_insert_iterator** — Iterator for push_front  
108. **insert_iterator** — Iterator for insert  
109. **istream_iterator** — Input stream iterator  
110. **ostream_iterator** — Output stream iterator  
111. **istreambuf_iterator** — Input stream buffer iterator  
112. **ostreambuf_iterator** — Output stream buffer iterator  

---

## Functional Library

113. **function** *(C++11)* — Polymorphic function wrapper  
114. **bind** *(C++11)* — Bind function arguments  
115. **reference_wrapper** *(C++11)* — Reference wrapper  
116. **mem_fn** *(C++11)* — Member function wrapper  
117. **not1 / not2** — Negators  
118. **unary_function / binary_function** *(deprecated)* — Base classes  
119. **pointer_to_unary_function / pointer_to_binary_function** *(deprecated)*  
120. **plus / minus / multiplies / divides / modulus / negate** — Arithmetic operations  
121. **equal_to / not_equal_to / greater / less / greater_equal / less_equal** — Comparisons  
122. **logical_and / logical_or / logical_not** — Logical operations  
123. **bit_and / bit_or / bit_xor** *(C++11)* — Bitwise operations  

---

## Numerics Library

124. **complex** — Complex numbers  
125. **valarray** — Numerical arrays  
126. **numeric_limits** — Information about numeric types  
127. **random** *(C++11)* — Random number generation  
128. **ratio** *(C++11)* — Compile-time rational arithmetic  
129. **cfenv** *(C++11)* — Floating-point environment  

---

## Utilities Library

130. **utility** — Utility components  
131. **tuple** *(C++11)* — Tuple library  
132. **optional** *(C++17)* — Optional objects  
133. **variant** *(C++17)* — Type-safe union  
134. **any** *(C++17)* — Type-safe container for single values  
135. **bitset** — Fixed-size sequence of bits  
136. **memory** — Memory management  
137. **scoped_allocator** *(C++11)*  
138. **type_traits** *(C++11)* — Type traits  
139. **chrono** *(C++11)* — Time utilities  
140. **typeindex** *(C++11)*  
141. **functional** — Function objects  
142. **initializer_list** *(C++11)*  

---

## String Library

143. **string** — Basic string class  
144. **wstring** — Wide string class  
145. **u16string** *(C++11)* — UTF-16 string  
146. **u32string** *(C++11)* — UTF-32 string  
147. **string_view** *(C++17)* — String view  

---

## Input/Output Library

148. **iostream** — Standard input/output streams  
149. **fstream** — File streams  
150. **sstream** — String streams  
151. **iomanip** — Input/output manipulators  
152. **iosfwd** — Forward declarations of I/O classes  

---

## Regular Expressions Library *(C++11)*

153. **regex** — Regular expressions  
154. **regex_match** — Match regular expression  
155. **regex_search** — Search for regular expression  
156. **regex_replace** — Replace regular expression  
157. **regex_iterator** — Regex iterator  
158. **regex_token_iterator** — Regex token iterator  

---

## Atomic Operations Library *(C++11)*

159. **atomic** — Atomic types  
160. **atomic_flag** — Atomic flag  
161. **memory_order** — Memory ordering  

---

## Thread Support Library *(C++11)*

162. **thread** — Threads  
163. **mutex** — Mutual exclusion  
164. **condition_variable** — Condition variables  
165. **future** — Asynchronous operations  
166. **async** — Asynchronous execution  
167. **promise** — Store value for future retrieval  
168. **packaged_task** — Package function for future execution  

---

## Filesystem Library *(C++17)*

169. **filesystem** — File system operations  
170. **path** — Represents a path  
171. **directory_iterator** — Iterates over directory contents  
172. **recursive_directory_iterator** — Recursively iterates over directory contents  

---

## Ranges Library *(C++20)*

173. **ranges** — Range-based algorithms and views  
174. **views** — Range views  
175. **range** — Range concepts  

---

## Coroutines Library *(C++20)*

176. **coroutine** — Coroutine support  
177. **coroutine_handle** — Coroutine handle  
178. **coroutine_traits** — Coroutine traits  

---

## Concepts Library *(C++20)*

179. **concepts** — Concepts library  

---

## Span Library *(C++20)*

180. **span** — Non-owning view of a contiguous sequence  

---

## Format Library *(C++20)*

181. **format** — Text formatting  

---

**See also:**  
- [C++ Standard Library Reference (cppreference.com)](https://en.cppreference.com/w/cpp/standard_library)  
- [ISO C++ STL documentation](https://isocpp.org/std/the-standard)  
- Explore further with your C++ toolchain’s documentation.

---
*This reference is exhaustive yet approachable. Consult your compiler and cppreference for the most up-to-date STL features!*
