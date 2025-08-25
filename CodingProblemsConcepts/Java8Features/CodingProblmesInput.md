# Strings, Numbers, and Math - Input and Output Examples

## Chapter 1: Strings, Numbers, and Math Problems

### String Manipulation Problems

**1. Counting duplicate characters**
```
Input: "programming"
Output: {r=2, g=2, m=2}

Input: "hello world"
Output: {l=3, o=2}

Input: "abcdef"
Output: {} (no duplicates)
```

**2. Finding the first non-repeated character**
```
Input: "programming"
Output: 'p'

Input: "hello"
Output: 'h'

Input: "aabbcc"
Output: null (no non-repeated character)
```

**3. Reversing letters and words**
```
Input: "hello world"
Output (reverse letters): "dlrow olleh"
Output (reverse words): "world hello"

Input: "Java Programming"
Output (reverse letters): "gnimmargorP avaJ"
Output (reverse words): "Programming Java"
```

**4. Checking whether a string contains only digits**
```
Input: "12345"
Output: true

Input: "123a45"
Output: false

Input: "0987654321"
Output: true

Input: ""
Output: false
```

**5. Counting vowels and consonants**
```
Input: "hello world"
Output: Vowels: 3, Consonants: 7

Input: "Programming"
Output: Vowels: 3, Consonants: 8

Input: "aeiou"
Output: Vowels: 5, Consonants: 0
```

**6. Counting the occurrences of a certain character**
```
Input: "programming", character: 'r'
Output: 2

Input: "hello world", character: 'l'
Output: 3

Input: "java", character: 'x'
Output: 0
```

**7. Converting a string into an int, long, float, or double**
```
Input: "123"
Output: int: 123, long: 123L, float: 123.0f, double: 123.0

Input: "45.67"
Output: float: 45.67f, double: 45.67

Input: "9876543210"
Output: long: 9876543210L

Input: "invalid"
Output: NumberFormatException
```

**8. Removing white spaces from a string**
```
Input: " hello world "
Output: "helloworld"

Input: "  Java   Programming  "
Output: "JavaProgramming"

Input: "a b c d"
Output: "abcd"
```

**9. Joining multiple strings with a delimiter**
```
Input: ["apple", "banana", "cherry"], delimiter: ", "
Output: "apple, banana, cherry"

Input: ["Java", "Python", "JavaScript"], delimiter: " | "
Output: "Java | Python | JavaScript"

Input: ["hello"], delimiter: "-"
Output: "hello"
```

**10. Generating all permutations**
```
Input: "abc"
Output: ["abc", "acb", "bac", "bca", "cab", "cba"]

Input: "ab"
Output: ["ab", "ba"]

Input: "a"
Output: ["a"]

Input: "xyz"
Output: ["xyz", "xzy", "yxz", "yzx", "zxy", "zyx"]
```

**11. Checking whether a string is a palindrome**
```
Input: "racecar"
Output: true

Input: "hello"
Output: false

Input: "A man a plan a canal Panama" (ignoring spaces and case)
Output: true

Input: "race a car" (ignoring spaces)
Output: false
```

**12. Removing duplicate characters**
```
Input: "programming"
Output: "progamin"

Input: "hello"
Output: "helo"

Input: "aabbccdd"
Output: "abcd"
```

**13. Removing a given character**
```
Input: "hello world", character: 'l'
Output: "heo word"

Input: "programming", character: 'r'
Output: "pogamming"

Input: "java", character: 'x'
Output: "java"
```

**14. Finding the character with the most appearances**
```
Input: "programming"
Output: 'r' (appears 2 times), 'g' (appears 2 times), 'm' (appears 2 times)

Input: "hello world"
Output: 'l' (appears 3 times)

Input: "abcdef"
Output: Any character (all appear once)
```

**15. Sorting an array of strings by length**
```
Input: ["apple", "hi", "banana", "a", "programming"]
Output: ["a", "hi", "apple", "banana", "programming"]

Input: ["java", "python", "c", "javascript"]
Output: ["c", "java", "python", "javascript"]
```

**16. Checking that a string contains a substring**
```
Input: "hello world", substring: "world"
Output: true

Input: "Java Programming", substring: "Script"
Output: false

Input: "programming", substring: "gram"
Output: true
```

**17. Counting substring occurrences in a string**
```
Input: "ababab", substring: "ab"
Output: 3

Input: "hello hello hello", substring: "hello"
Output: 3

Input: "programming", substring: "mm"
Output: 1

Input: "abcdef", substring: "xyz"
Output: 0
```

**18. Checking whether two strings are anagrams**
```
Input: "listen", "silent"
Output: true

Input: "hello", "world"
Output: false

Input: "anagram", "nagaram"
Output: true

Input: "rat", "car"
Output: false
```

**19. Declaring multiline strings (text blocks)**
```
Input: Text block declaration
Output: 
"""
This is a 
multiline string
with proper formatting
"""

Input: JSON text block
Output:
"""
{
  "name": "John",
  "age": 30,
  "city": "New York"
}
"""
```

**20. Concatenating the same string n times**
```
Input: "hello", n: 3
Output: "hellohellohello"

Input: "Java", n: 2
Output: "JavaJava"

Input: "abc", n: 0
Output: ""

Input: "x", n: 5
Output: "xxxxx"
```

**21. Removing leading and trailing spaces**
```
Input: "  hello world  "
Output: "hello world"

Input: "\t\n  Java Programming  \n\t"
Output: "Java Programming"

Input: "   "
Output: ""
```

**22. Finding the longest common prefix**
```
Input: ["flower", "flow", "flight"]
Output: "fl"

Input: ["dog", "racecar", "car"]
Output: ""

Input: ["interspecies", "interstellar", "interstate"]
Output: "inters"

Input: ["apple", "apple", "apple"]
Output: "apple"
```

**23. Applying indentation**
```
Input: "hello\nworld\njava", indentation: 4
Output: 
"    hello
    world
    java"

Input: "line1\nline2", indentation: 2
Output:
"  line1
  line2"
```

**24. Transforming strings**
```
Input: "hello world", transformation: toUpperCase
Output: "HELLO WORLD"

Input: "JAVA PROGRAMMING", transformation: toLowerCase
Output: "java programming"

Input: "hello world", transformation: reverse
Output: "dlrow olleh"

Input: "java", transformation: capitalize
Output: "Java"
```

---

### Number and Math Problems

**25. Computing the minimum and maximum of two numbers**
```
Input: 5, 10
Output: min: 5, max: 10

Input: -3, 7
Output: min: -3, max: 7

Input: 15, 15
Output: min: 15, max: 15

Input: 0, -5
Output: min: -5, max: 0
```

**26. Summing two large int/long values and operation overflow**
```
Input: Integer.MAX_VALUE, 1
Output: ArithmeticException (overflow)

Input: Long.MAX_VALUE, 1L
Output: ArithmeticException (overflow)

Input: 1000000, 2000000
Output: 3000000

Input: -1000000, 500000
Output: -500000
```

**27. String as an unsigned number in the radix**
```
Input: "FF", radix: 16
Output: 255

Input: "1010", radix: 2
Output: 10

Input: "777", radix: 8
Output: 511

Input: "123", radix: 10
Output: 123
```

**28. Converting into a number by an unsigned conversion**
```
Input: -1 (as unsigned int)
Output: 4294967295

Input: -1 (as unsigned long)
Output: 18446744073709551615

Input: 100 (as unsigned)
Output: 100
```

**29. Comparing two unsigned numbers**
```
Input: -1, 1 (as unsigned)
Output: -1 > 1 (true, because -1 as unsigned is very large)

Input: 100, 200 (as unsigned)
Output: 100 < 200 (true)

Input: Integer.MAX_VALUE, -1 (as unsigned)
Output: Integer.MAX_VALUE < -1 (true)
```

**30. Division and modulo of unsigned values**
```
Input: -1, 2 (as unsigned division)
Output: division: 2147483647, modulo: 1

Input: 100, 3 (as unsigned)
Output: division: 33, modulo: 1

Input: -4, 3 (as unsigned)
Output: division: 1431655764, modulo: 0
```

**31. Checking if double/float is a finite floating-point value**
```
Input: 3.14
Output: true

Input: Double.POSITIVE_INFINITY
Output: false

Input: Double.NaN
Output: false

Input: Float.NEGATIVE_INFINITY
Output: false

Input: 0.0
Output: true
```

**32. Applying logical AND/OR/XOR to two boolean expressions**
```
Input: true, false
Output: AND: false, OR: true, XOR: true

Input: true, true
Output: AND: true, OR: true, XOR: false

Input: false, false
Output: AND: false, OR: false, XOR: false
```

**33. Converting BigInteger into a primitive type**
```
Input: BigInteger("123456789012345")
Output: 
int: ArithmeticException (too large)
long: 123456789012345L
float: 1.2345679E14f
double: 1.23456789012345E14

Input: BigInteger("100")
Output:
int: 100
long: 100L
float: 100.0f
double: 100.0
```

**34. Converting long into int**
```
Input: 123L
Output: 123

Input: 2147483648L (larger than Integer.MAX_VALUE)
Output: ArithmeticException (overflow)

Input: -2147483649L (smaller than Integer.MIN_VALUE)
Output: ArithmeticException (overflow)

Input: 0L
Output: 0
```

**35. Computing the floor of a division and modulus**
```
Input: 7, 3
Output: floorDiv: 2, floorMod: 1

Input: -7, 3
Output: floorDiv: -3, floorMod: 2

Input: 7, -3
Output: floorDiv: -3, floorMod: -2

Input: -7, -3
Output: floorDiv: 2, floorMod: -1
```

**36. Next floating-point value**
```
Input: 1.0 (double)
Output: 1.0000000000000002

Input: 1.0f (float)
Output: 1.0000001f

Input: 0.0
Output: 4.9E-324 (smallest positive double)

Input: Double.MAX_VALUE
Output: Double.POSITIVE_INFINITY
```

**37. Multiplying two large int/long values and operation overflow**
```
Input: Integer.MAX_VALUE, 2
Output: ArithmeticException (overflow)

Input: 1000000, 2000
Output: 2000000000

Input: Long.MAX_VALUE, 2L
Output: ArithmeticException (overflow)

Input: -1000, 500
Output: -500000
```

**38. Fused Multiply Add**
```
Input: a=2.0, b=3.0, c=1.0 (a*b + c)
Output: 7.0

Input: a=0.1, b=0.2, c=0.3
Output: 0.32 (more precise than (0.1*0.2)+0.3)

Input: a=1.5, b=2.5, c=-1.0
Output: 2.75
```

**39. Compact number formatting**
```
Input: 1000
Output: "1K"

Input: 1500000
Output: "1.5M"

Input: 2300000000L
Output: "2.3B"

Input: 123
Output: "123"

Input: 1000000000000L
Output: "1T"
```

---

**Note:** These input/output examples demonstrate the expected behavior for each string, number, and math problem. When implementing these problems, consider:

1. **Edge Cases**: Empty strings, null values, boundary conditions
2. **Error Handling**: Invalid inputs, overflow conditions, format exceptions
3. **Performance**: Efficient algorithms for string manipulation and mathematical operations
4. **Java 8+ Features**: Use streams, lambdas, and modern APIs where applicable
5. **Internationalization**: Consider locale-specific formatting for numbers

Use these examples to test your implementations and ensure they handle all scenarios correctly!
