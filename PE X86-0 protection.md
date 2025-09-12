![1757668440521](images/PEX86-0protection/1757668440521.png)


The provided function `sub_401726` checks if the input string matches the specific sequence of ASCII values when the length is exactly 7. The expected password is derived from the following ASCII values:

- 83 → 'S'
- 80 → 'P'
- 97 → 'a'
- 67 → 'C'
- 73 → 'I'
- 111 → 'o'
- 83 → 'S'

Thus, the correct password is the string **"SPaCIoS"**.

**Explanation:**
The function verifies each character of the input string against the hardcoded values. If all conditions are met, it prints "Gratz man :)" and exits. Otherwise, it outputs "Wrong password". The password combines uppercase and lowercase letters as specified by the ASCII values.
