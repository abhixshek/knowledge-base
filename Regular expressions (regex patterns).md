`>>>import re`
>[!CAUTION]
>Both patterns and strings to be searched can be Unicode strings ([`str`](https://docs.python.org/3/library/stdtypes.html#str "str")) as well as 8-bit strings ([`bytes`](https://docs.python.org/3/library/stdtypes.html#bytes "bytes")). However, Unicode strings and 8-bit strings cannot be mixed: that is, you cannot match a Unicode string with a byte pattern or vice-versa; similarly, when asking for a substitution, the replacement string must be of the same type as both the pattern and the search string.

>[!INFO]
>Regular expressions use the backslash character (`'\'`) to indicate special forms or to allow special characters to be used without invoking their special meaning. This collides with Python’s usage of the same character for the same purpose in string literals; for example, to match a literal backslash, one might have to write `'\\\\'` as the pattern string, because the regular expression must be `\\`, and each backslash must be expressed as `\\` inside a regular Python string literal. Also, please note that any invalid escape sequences in Python’s usage of the backslash in string literals now generate a [`DeprecationWarning`](https://docs.python.org/3/library/exceptions.html#DeprecationWarning "DeprecationWarning") and in the future this will become a [`SyntaxError`](https://docs.python.org/3/library/exceptions.html#SyntaxError "SyntaxError"). This behaviour will happen even if it is a valid escape sequence for a regular expression.
>
>**The solution is to use Python’s raw string notation for regular expression patterns**; backslashes are not handled in any special way in a string literal prefixed with `'r'`. So `r"\n"` is a two-character string containing `'\'` and `'n'`, while `"\n"` is a one-character string containing a newline. Usually patterns will be expressed in Python code using this raw string notation.
>
>It is important to note that most regular expression operations are available as module-level functions and methods on [compiled regular expressions](https://docs.python.org/3/library/re.html#re-objects). The functions are shortcuts that don’t require you to compile a regex object first, but miss some fine-tuning parameters.
>
>**See also:** The third-party [regex](https://pypi.org/project/regex/) module, which has an API compatible with the standard library [`re`](https://docs.python.org/3/library/re.html#module-re "re: Regular expression operations.") module, but offers additional functionality and a more thorough Unicode support.

A regular expression (or RE) specifies a set of strings that matches it; the functions in this module let you check if a particular string matches a given regular expression (or if a given regular expression matches a particular string, which comes down to the same thing).

Regular expressions can be concatenated to form new regular expressions; if _A_ and _B_ are both regular expressions, then _AB_ is also a regular expression. In general, if a string _p_ matches _A_ and another string _q_ matches _B_, the string _pq_ will match AB. This holds unless _A_ or _B_ contain low precedence operations; boundary conditions between _A_ and _B_; or have numbered group references. Thus, complex expressions can easily be constructed from simpler primitive expressions like the ones described here. For details of the theory and implementation of regular expressions, consult the Friedl book or almost any textbook about **compiler construction**.

**Regular expressions** (called **REs, or regexes, or regex patterns**) are essentially a tiny, highly specialized programming language embedded inside Python and made available through the [`re`](https://docs.python.org/3/library/re.html#module-re "re: Regular expression operations.") module. Using this little language, you specify the rules for the set of possible strings that you want to match; this set might contain English sentences, or e-mail addresses, or TeX commands, or anything you like. You can then ask questions such as “Does this string match the pattern?”, or “Is there a match for the pattern anywhere in this string?”. You can also use REs to modify a string or to split it apart in various ways.

```

Regular expression patterns are compiled into a series of bytecodes which are then executed by a matching engine written in C. For advanced use, it may be necessary to pay careful attention to how the engine will execute a given RE, and write the RE in a certain way in order to produce bytecode that runs faster. Optimization isn’t covered in this document, because it requires that you have a good understanding of the matching engine’s internals.
```

**NOTE:** The regular expression language is relatively small and restricted, so not all possible string processing tasks can be done using regular expressions. There are also tasks that _can_ be done with regular expressions, but the expressions turn out to be very complicated. In these cases, you may be better off writing Python code to do the processing; while Python code will be slower than an elaborate regular expression, it will also probably be more understandable.

Regular expressions can contain both **special** and **ordinary** characters. Most ordinary characters, like `'A'`, `'a'`, or `'0'`, are the simplest regular expressions; they simply match themselves. You can concatenate ordinary characters, so `last` matches the string `'last'`.
(In the rest of this section, we’ll write RE’s in `this special style`, usually without quotes, and strings to be matched `'in single quotes'`.)

Some characters, like `'|'` or `'('`, are special. **Special characters either stand for classes of ordinary characters, or affect how the regular expressions around them are interpreted.**
Special characters are also called **metacharacters** in RE. And don’t match themselves. Instead, they signal that some out-of-the-ordinary thing should be matched, or they affect other portions of the RE by repeating them or changing their meaning. 

>**Repetition operators** or **quantifiers** (`*`, `+`, `?`, `{m,n}`, etc) **cannot be directly nested.** This avoids ambiguity with the non-greedy modifier suffix `?`, and with other modifiers in other implementations. To apply a second repetition to an inner repetition, parentheses may be used. For example, the expression `(?:a{6})*` matches any multiple of six `'a'` characters.

Here’s a complete list of the metacharacters (11, considering pair of brackets as 1)
### ==. ^ $ * + ? { } [ ] \\ | ( ) ==

**^** is called a **caret** .

>[!Gentle explanation]
The first metacharacters we’ll look at are `[` and `]`. They’re used for specifying a **character class**, which is a set of characters that you wish to match. Characters can be listed individually, or a range of characters can be indicated by giving two characters and separating them by a `'-'`. For example, `[abc]` will match any of the characters `a`, `b`, or `c`; this is the same as `[a-c]`, which uses a range to express the same set of characters. If you wanted to match only lowercase letters, your RE would be `[a-z]`.
>
 **Metacharacters (except `\`) are not active inside classes.** For example, `[akm$]` will match any of the characters `'a'`, `'k'`, `'m'`, or `'$'`; `'$'` is usually a metacharacter, but inside a character class it’s stripped of its special nature.
 >
 >You can match the characters not listed within the class by _complementing_ the set. This is indicated by including a `'^'` as the first character of the class. For example, `[^5]` will match any character except `'5'`. If the caret appears elsewhere in a character class, it does not have special meaning. For example: `[5^]` will match either a `'5'` or a `'^'`.
 >
 >Perhaps the most important metacharacter is the backslash, `\`. As in Python string literals, the backslash can be followed by various characters to signal various special sequences. It’s also used to escape all the metacharacters so you can still match them in patterns; for example, if you need to match a `[` or `\`, you can precede them with a backslash to remove their special meaning: `\[` or `\\`.
 >
 >Some of the special sequences beginning with `'\'` represent predefined sets of characters that are often useful, such as the set of digits, the set of letters, or the set of anything that isn’t whitespace.

---
The special characters are:

`.`

(Dot.) In the default mode, this matches any character except a newline. If the [`DOTALL`](https://docs.python.org/3/library/re.html#re.DOTALL "re.DOTALL") flag has been specified, this matches any character including a newline.

>`^`
>
>(Caret.) Matches the start of the string, and in [`MULTILINE`](https://docs.python.org/3/library/re.html#re.MULTILINE "re.MULTILINE") mode also matches immediately after each newline.



>`$`
>Matches the end of the string or just before the newline at the end of the string, and in [`MULTILINE`](https://docs.python.org/3/library/re.html#re.MULTILINE "re.MULTILINE") mode also matches before a newline. `foo` matches both ‘foo’ and ‘foobar’, while the regular expression `foo$` matches only ‘foo’. More interestingly, searching for `foo.$` in `'foo1\nfoo2\n'` matches ‘foo2’ normally, but ‘foo1’ in [`MULTILINE`](https://docs.python.org/3/library/re.html#re.MULTILINE "re.MULTILINE") mode; searching for a single `$` in `'foo\n'` will find two (empty) matches: one just before the newline, and one at the end of the string.


`*`
Causes the resulting RE to match 0 or more repetitions of the preceding RE, as many repetitions as are possible. `ab*` will match ‘a’, ‘ab’, or ‘a’ followed by any number of ‘b’s.

`+`

Causes the resulting RE to match 1 or more repetitions of the preceding RE. `ab+` will match ‘a’ followed by any non-zero number of ‘b’s; it will not match just ‘a’.

`?`

Causes the resulting RE to match 0 or 1 repetitions of the preceding RE. `ab?` will match either ‘a’ or ‘ab’.

`*?`, `+?`, `??`

The `'*'`, `'+'`, and `'?'` quantifiers are all _greedy_; they match as much text as possible. Sometimes this behaviour isn’t desired; if the RE `<.*>` is matched against `'<a> b <c>'`, it will match the entire string, and not just `'<a>'`. Adding `?` after the quantifier makes it perform the match in **_non-greedy_ or _minimal_ fashion**; **as _few_ characters as possible will be matched.** Using the RE `<.*?>` will match only `'<a>'`.


>`*+`, `++`, `?+`
>Like the `'*'`, `'+'`, and `'?'` quantifiers, those where `'+'` is appended also match as many times as possible. However, unlike the true greedy quantifiers, these do not allow back-tracking when the expression following it fails to match. These are known as _possessive_ quantifiers. For example, `a*a` will match `'aaaa'` because the `a*` will match all 4 `'a'`s, but, when the final `'a'` is encountered, the expression is backtracked so that in the end the `a*` ends up matching 3 `'a'`s total, and the fourth `'a'` is matched by the final `'a'`. However, when `a*+a` is used to match `'aaaa'`, the `a*+` will match all 4 `'a'`, but when the final `'a'` fails to find any more characters to match, the expression cannot be backtracked and will thus fail to match. `x*+`, `x++` and `x?+` are equivalent to `(?>x*)`, `(?>x+)` and `(?>x?)` correspondingly.
>(getting multiple repeat error in above trys. CHECK AGAIN)


`{m}`

Specifies that exactly _m_ copies of the previous RE should be matched; fewer matches cause the entire RE not to match. For example, `a{6}` will match exactly six `'a'` characters, but not five.

`{m,n}`

Causes the resulting RE to match from _m_ to _n_ repetitions of the preceding RE, attempting to match as many repetitions as possible. For example, `a{3,5}` will match from 3 to 5 `'a'` characters. Omitting _m_ specifies a lower bound of zero, and omitting _n_ specifies an infinite upper bound. As an example, `a{4,}b` will match `'aaaab'` or a thousand `'a'` characters followed by a `'b'`, but not `'aaab'`. The comma may not be omitted or the modifier would be confused with the previously described form.

`{m,n}?`

Causes the resulting RE to match from _m_ to _n_ repetitions of the preceding RE, attempting to match as _few_ repetitions as possible. This is the non-greedy version of the previous quantifier. For example, on the 6-character string `'aaaaaa'`, `a{3,5}` will match 5 `'a'` characters, while `a{3,5}?` will only match 3 characters.

>`{m,n}+`
>Causes the resulting RE to match from _m_ to _n_ repetitions of the preceding RE, attempting to match as many repetitions as possible _without_ establishing any backtracking points. This is the possessive version of the quantifier above. For example, on the 6-character string `'aaaaaa'`, `a{3,5}+aa` attempt to match 5 `'a'` characters, then, requiring 2 more `'a'`s, will need more characters than available and thus fail, while `a{3,5}aa` will match with `a{3,5}` capturing 5, then 4 `'a'`s by backtracking and then the final 2 `'a'`s are matched by the final `aa` in the pattern. `x{m,n}+` is equivalent to `(?>x{m,n})`.
>New in version 3.11.


