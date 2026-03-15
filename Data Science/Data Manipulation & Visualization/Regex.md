\\ escape character
^ start of a string
$ end of a string
# Characters
| Character | Legend                          | Example    | Sample Match |
| --------- | ------------------------------- | ---------- | ------------ |
| \d        | one digit from 0 to 9           | file_\d\d  | file_25      |
| \w        | word character                  | \w-\w\w\w  | A-b_1        |
| \s        | whitespace character            | a\sb\sc    | a b  <br>c   |
| \D        | non-digit                       | \D\D\D     | ABC          |
| \W        | non-word character              | \W\W\W\W\W | *-+=)        |
| \S        | non whitespace character        | \S\S\S\S   | Yoyo         |
| .         | any character except line break | a.c        | abc          |
## Character Classes
| Character | Legend                                                 | Example   | Sample Match                                                            |
| --------- | ------------------------------------------------------ | --------- | ----------------------------------------------------------------------- |
| [xyz]     | One of the characters in the brackets                  | [AEIOU]   | Match one uppercase vowel                                               |
| [x-y]     | One of the characters in the range from x to y         | [A-Z]+    | GREAT                                                                   |
| \[^x]     | One character that is not x                            | [^a-z]{3} | A1!                                                                     |
| \[^x-y]   | One of the characters **not** in the range from x to y | \[^ -~]+  | Characters that are **not** in the printable section of the ASCII table |
# Quantifiers
| Quantifier | Legend              | Example        | Sample Match             |
| ---------- | ------------------- | -------------- | ------------------------ |
| +          | One or more         | Version \w-\w+ | Version A-b1_1           |
| *          | Zero or more times  | A*B*C*         | AAACC                    |
| ?          | Once or none        | plurals?       | plural                   |
| +?, \*?    | Non greedy capture  | m.+?s          | match m123s in m123s123s |
| {3}        | Exactly three times | \D{3}          | ABC                      |
| {3,}       | Three or more times | \w{3,}         | regex_tutorial           |
| {2,4}      | Two to four times   | \d{2,4}        | 156                      |
# Logic
| Logic              | Legend                                                                                     | Example               | Sample Match |
| ------------------ | ------------------------------------------------------------------------------------------ | --------------------- | ------------ |
| \|                 | Alternation / OR operand                                                                   | 22\|33                | 33           |
| ( … )              | Capturing group                                                                            | A(nt\|pple)           | Apple        |
| \1                 | Contents of Group 1                                                                        | r(\w)g\1x             | regex        |
| \2                 | Contents of Group 2                                                                        | (\d\d)\+(\d\d)=\2\+\1 | 12+65=65+12  |
| (?: …)             | Non-capturing group (Can't use \\1 to reference)                                           | A(?:nt\|pple)         | Apple        |
| target(?!pattern)  | negative look ahead: assert no such *pattern* immediately on the **right** of the *target* |                       |              |
| target(?<!pattern) | negative look behind                                                                       |                       |              |
| target(?=pattern)  | positive look ahead                                                                        |                       |              |
| target(?<=pattern) | positive look behind                                                                       |                       |              |
# Examples
```python
# check if n is a prime number
not re.match(r'^.?$|^(..+?)\1+$', '1'*n)
# match a Chinese character
[\u4e00-\u9fff]
```