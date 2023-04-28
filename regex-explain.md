# Regex Analysis: Complex Password Validation

Regular expressions, or regex, define search patterns in a body of text. Here I will look over an example to explore the different kinds of syntax you can expect. They can appear cryptic at first but upon closer inspection they can be quite tame.

## Summary

Let's look at a regex that validates a complex password. While regex are often used to search for distinctly ordered complex arrangements of characters we can also use them to validate the properties of a string to check for the existence of specific characters or qualities.

In this example we want to validate whether a string contains at least one number, lowercase letter, uppercase letter, and a special character. We also want to make sure that the password is greater than 8 characters.

Our regex looks like this:

```
/(?=(.*[0-9]))(?=.*[\!@#$%^&*()\\[\]{}\-_+=~`|:;"'<>,.\/?])(?=.*[a-z])(?=(.*[A-Z]))(?=(.*)).{8,}/
```

Within it we have several existential validation subexpressions marked by backslashes on either end that define our regex:

- A number validation: `(?=(.*[0-9]))`
- A special character validation: `` (?=.*[\!@#$%^&*()\\[\]{}\-_+=~`|:;"'<>,.\/?]) ``
- A lowercase letter validation: `(?=.*[a-z])`
- An uppercase letter validation: `(?=(.*[A-Z])`
- An additional character validation: `(?=(.*))`
- A minimum length validation: `.{8,}`

## Table of Contents

- [Grouping and Capturing](#grouping-and-capturing)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)
- [Character Classes](#character-classes)
- [Bracket Expressions](#bracket-expressions)
- [Quantifiers](#quantifiers)
- [Anchors](#anchors)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [OR Operator](#or-operator)
- [Flags](#flags)
- [Boundaries](#boundaries)
- [Back-references](#back-references)

## Regex Components

### Grouping and Capturing

If you want your regex to check multiple parts of a string, each with different requirements, then you use grouping constructs. The primary way to group a section is using parentheses `()`. Each group within the parentheses are known as a subexpression.

Our example has several subexpressions. This is because we are not looking for a string of a specific characters in a specific order, but rather just checking for their existence according to set qualifiers.

The first sub expression is `(?=(.*[0-9]))`. Additionally this subexpression has a further subsubexpression within it `(.*[0-9])`. This searches for any characters that are numbers. The outer subexpression contains the look-ahead that allows us to search existentially rather than sequentially. So, our first subexpression postively matches the existence of any quantity of numbers.

The other subexpression within our regex example function in an identical way.

- Special character existential match: `(?=.*[\!@#$%^&*()\\[\]{}\-_+=~|:;"'<>,.\/?])`
- Lower case letter existential match: `(?=.*[a-z])`
- Upper case letter existential match: `(?=.*[A-Z])`
- Additional character of any kind existential match: `(?=(.*))`

### Look-ahead and Look-behind

Collectively called lookaround, lookahead and lookbehind match characters and then give up the match, only returning the resulting boolean: match or no-match. They do not use up the characters in the string but only check if a match is possible or not.

Our regex example employs positive lookaheads to existentially confirm, or check the presence of, specific ranges of characters.

- `a(?!b)` - Negative lookaheads are useful for matching something not followed by something else, i.e. This match a `a` not followed by a `b`.
- `a(?=b)` - Positive lookaheads work similarly, i.e. This will match a `a` followed by a `b`.

- `(?<!a)b` - Negative lookbehinds will tell the engine to temporartily step backwards in the string to check if the text inside the lookbehind can be matched there, i.e. This will match a `b` that is not preceded by an `a`.
- `(?=a)b` - Postiive lookbehinds work similarly, i.e. This will match a `b` that is preceded by an `a`.

### Character Classes

Character classes define a set of characters. One example are the bracket expressions. Other common character classes are:

- `.` - Matches any character except the newline character (\n). We use this character class several times within our regex example.

- `\d` - Matches any Arabic numeral digit. This class is equivalent to the bracket expression [0-9].

- `\w` - Matches any alphanumeric character from the basic Latin alphabet, including the underscore. This class is equivalent to the bracket expression [A-Za-z0-9_].

- `\s` - Matches a single whitespace character, including tabs and line breaks

### Bracket Expressions

Any characters within a set of square brackets `[]` represents a range of characters we hope to match. These are known as 'bracket expressions' or 'positive character group'. Hypens can determine a range of characters between some defined boundary. I.e `[1-5]` will search for the numbers `1, 2, 3, 4, 5`.

In our example we want to include several ranges of characters types. First we use the bracket notation to positively define the all numbers: `[0-9]`. The next use of bracket notation is not a range but a set of special characters: `[\!@#$%^&*()\\[\]{}\-_+=~|:;"'<>,.\/?]` (and the backtick which was used to define this code block example). We implement backslashes `\` to escape characters that have meaning within a regex context so that we can search for them as character literals. The last two positive character groups we want to define are the lowercase `[a-z]` and uppercase `[A-Z]` letters. Now we have a search pattern that is looking to postiively match all uppercase, lowercase, numbers, and special characters.

If you wanted to exclude a range of characters (to negatively match a range) you can do so with the `^` symbol inside the `[]`, i.e. `[^1-5]` will exclude numbers 1-5.

### Quantifiers

We also want to make sure that our password validation only positively matches passwords that are greater than 8 characters long. This is done using quantifiers. Quantifiers set the limits of the string that your regex matches. Typically they include the minimum and maximum number of characters that your pattern is looking for, but they can also include more nuanced determinations.

In our example we make sure that our password is greater than 8 characters long by adding `.{8,}`. This tells the pattern to positively match anything (`.`) with minimum 8 characters and no maximum.

### Anchors

The characters `^` and `$` are both anchors. The `^` anchor marks a string that begins with the characters that follow it. The `$` anchor marks a string that ends with the characters that preced it. They can both be preceded by an exact string or a range of possible matches.

In our example there are no anchors present. This is because we are not using our search pattern to find a defined characters in a set series, rather we are looking for the existence of specific characters independent of their order.

### Greedy and Lazy Match

Regex are inherently greedy, they match as many occurences of particular patterns as possible. For example:

`*` - Matches the pattern zero or more times

`+` - Matches the pattern one or more times

`?` - Matches the pattern zero or one time

`{}` - Curly brackets can provide three different ways to set limits for a match:

`{ n }` - Matches the pattern exactly n number of times

`{ n, }` - Matches the pattern at least n number of times

`{ n, x }` - Matches the pattern from a minimum of n number of times to a maximum of x number of times

Each of these quantifiers can be made lazy by adding the `?` symbol after it. This will cause it to look for as few occurrences as possible.

### OR Operator

The OR operator `|` can be used to apply `OR` logic within a subexpression, i.e. `(a|b|c)` will search for the presence of either of a, b, or c.

### Flags

Flags are placed at the end of a regex, outside of the second slash character. They define additional search functionality or limits to be set on the regex. The three most common flags are:

- `g` - Global search: the regex should be tested against all possible matches in a string.

- `i` - Case-insensitive search: case should be ignored while attempting a match in a string

- `m` - Multi-line search: a multi-line input string should be treated as multiple lines

### Boundaries

The metacharacter `\b` is an anchor, similar to the `^` or `$`. It allows you to perform whole word searches within the regex: `\bword\b`. This can also include digits.

### Back-references

Back references can be used to reduce the length of the regex and simplify the syntax while reusing portions of your expression. Each time you open a parenthesis and create a subexpression within the regex you are creating a capturing group. Each capturing group can be referenced using a backreference. For example, `([a-c])x` would match `ax`, `bx`, and `cx`. by adding the backreference `\1` to the end of it we can reuse the search: `([a-c])x\1` will find `axax`, `bxbx`, and `cxcx`. This can be repeated and extended; `\1\1` will return `axaxax`, `bxbxbx`, and `cxcxcx` and `\99` is a valid backreference as long as you have 99 capture groups.

## Author

Isaac was for some range of time a student at the University of Washington Full Stack Coding Bootcamp. He can be found on [GitHub](https://github.com/dingbat-weasel) or reached by email at [perk.isaac@gmail.com](perk.isaac@gmail.com)
