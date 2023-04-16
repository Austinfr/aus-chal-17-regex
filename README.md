# Austin Challenge 17 Regex

Have you ever wanted to have look for a specific word or maybe you only know the start of a word. Maybe you want to check if a password contains certain characters and numbers, or if an email is formatted correctly. You can do all of this an more with regex!

## Summary

In short, regex is a pattern in the form of a string finding to look for a specific text. The word Regex is short for regular expression. It helps speed up searching for specific patterns in text.

This is also formatted for use in JavaScript so every pattern will be enclosed in forward slashes

To create a new regular expression all you need to do is use a constructor `new RegExp('regular expression', 'flags')` or match a string with the function `.match(regex)` or `regex.exec('text')`
> When creating a regex in javascript it doesn't need to be a regex object it can be a regex literal

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components

The two most basic components to Regex are Literal Characters and Special Characters

##### Literal Characters

Literal Characters are as you'd expect the alphabet

- If you create a regex of 'a' -> `/a/`, this will just match any instance of the letter a
- You can also look for specific words `'dog' -> /dog/`, `'cat' -> /cat/`, `'parrot' -> /parrot/`
- The literal characters are case sensitive

##### Special Characters

There are only 12 special characters within regex which will give a wider variety of searching power depending on how they are used
- The twelve are, the backslash `\`, the caret `^`, the dollar sign `$`, the dot `.`, the vertical bar `|`, the question mark `?`, the asterisk `*`, the plus sign `+`, the parenthesis `(` `)`, and the square and curly brackets `[` `]` `{` `}`
- They are refered to as metacharacters
- If you want to search for any of the above you will use a backslash which functions as the escape character when used before one of these metacharacters
    - ex: finding every instance of $1.00 would be `/\$1\.00/`

### Anchors

Anchors match position as indicated by the name.

The caret `^` matches the position as the first character in the string
- For the text 'city', the regex `/^c/` will match to c while `/^i/` won't match to anything

The dollar sign `$` matches the position as the last character in the string 
- For the text 'city', the regex `/$y/` will match to y while `/$t/` won't match to anything

### Quantifiers

The quantifiers indicate the quantity of characters of a pattern to match
The curly brackets will be commonly used `{ }` to specify a range for how many times the character might appear
- The regex `/a{3}/` would match any pattern of aaa, aaah would match but aah wouldn't
- You can specify an upper bound `{3,5}` or any amount after `{3,}` 
    - both `/a{3,5}/` and `/a{3,}/` would match 'aaaaaaaa' but `/a{3,5}/` would only match the first 5 "a"'s while `/a{3,}/` would match all of it

We can further simplify the `{ }` with some of the special characters

##### Shorthand Quantifiers

With the asterisk `*` we can find a range of 0 or more times, `{0,}`
- `/ca*t/` will match any pattern that has 'ct' with any amount of the letter a inbetween
    - This will match 'cat' or 'caaaaat' and the 'ct' in 'act'

With the plus sign `+` we will match anything that occurs at least once, `{1,}`
- `/bo+k*/` will match 'bo' in 'bot', 'book', 'boo' in 'boot' and 'booooo'

With the question mark `?` we will match if it doesn't appear or appears only once, `{0,1}`
- `/e?le?/` will match 'el' in 'angel' and 'le' in 'angle'
- `/ye+?/`  will match 'ye' in 'yeet' and 'ye' in 'yes'

We also get some extra functionality if used after any of the previous quantifiers. It will make them non-greedy which matches to the lower bound
- `/\[.*?\]/` will match anything in square brackets if you have a list like use in markdown for links

We use the `?` to make it non-greedy so that it won't match with more than is what's needed
- `/\[.*\]/` will match all of `[google](http:google.com) [yahoo]` but using the above `/\[.*?\]/` will match `[google]` and `[yahoo]` seperately

### Grouping Constructs

When we do a regex search any match will be captured as a singular group and anytime we use the parenthesis `( )` it will capture whatever is in the parenthesis as another group. This is useful in storing what we match so that we can work with and manipulate any captured groups
Group number 0 will always refer to everything that the expression matches

##### Non-capturing Group

Using a question mark and a colon together in a group `(?:)` will match but not remember what it matches which is useful if you have multiple groups but you only need to look for one part
- If you're matching multiple urls and you only need to look for the host and ingore the protocol and path, `/(?:https?)\/\/(.*)/`

##### named capture groups

With the use of a question mark and a given name the regex `/(?<name>x)/` will match x and store it in the `match.group.name` to be referenced
- whatever is inbetween the `< >` will be stored as the name, so `/,\s(?<firstName>\w)/` will match the word after a comma and space if we have a list of names formatted as lastName, firstName and store it in `match.group.firstName`

##### Back References

You can use the backslash `\` to refer to the group within the regular expression. Using `\1` will refer to the first group
- `/\b(\w+)\s\1\b/` will match any words that are used twice in a row because the `\1` will refer back to the word that is captured in our first group `(\w+)`

### Bracket Expressions

Depending on the type of brackets used they will specify different things and have additional funtionality based on what is put inbetween them as described more by other sections

The curly brackets will specify quantity `{ }`

The round brackets or parenthesis will specify groups `( )`

The square brackets will specify a character set `[ ]`

### Character Classes

##### Character Set
A character class or a character set will be used to match a single character between a few given so that if there is a similar spelling of a word or a misspelling you will be able to find it
- Any American vs British English word that uses either a S or a Z
    - realize vs realise can be found by `/reali[sz]e/`
    - gray vs grey can be found by `/gr[ae]y/`

This is very useful in any multilingual or in any commonly misspelled word case but it will only match one character so in the case of `/reali[sz]e/` will not match 'realisse' if someone happens to misspell it that way

You can also give a range using a hyphen `-` between letters or numbers
- `/[0-9]/` will match any number single digit 
- `/[A-z]/` will match every singe letter while `/[a-z]/` will only match a single lower case letter
    - you might need to use `/[a-zA-Z]/` instead of `/[A-z]/` if you're not using javascript

You can use multiple ranges at once. The hyphen will only take the range of the characters before and after it
- `/[0-9a-fA-F]/` will match any single hexadecimal charater

As long as there is one character within the brackets it will match it

##### Negation

If you put a special character into the brackets it will automatically assume you you are searching for an instance of it so you won't need a backslash

To find everything that isn't what's in the brackets you can use the caret within the brackets
- `/[^aeiou]/` will match all non-vowel single letter characters
- `/[^0-9]/` will match all non-digits

##### Dot metacharacter

The dot `.` matches any single character except for line breaks, it's off by default but can be turned on with flags
- `/./` will match every single character, digits and letters both
- `/c.t/` will match anything character pattern that has a character between a c and a t

### The OR Operator

The OR operator or the pipe `|` will match whichever it finds which is useful in determining email addresses that might end in a specific format `/com|org|edu/`
- Can also be used when trying to match lingo that might be different for different people
    - `chips|crisps`, `wedges|jojos`, `soda pop|pop|soda|cola|juice` can be refered to as similar foods
    - `special characters|metacharacters` as mentioned previously

### Flags

There are 7 usable flags in javascript but they are added to the end of the regex in order to add more functionality 
- `/\w/g` has the global flag and selects every word

`//d` flag indicates the use of indices which will store the positions in the string that contain any match
- `/watch/dg` run on `watch this watch` will return indices of `[0,5]` for the first match and `[11,16]` for the second match

The global flag `//g` indicates that the regular expression will be tested against all possible matches for the given text. If not included then the regex will only match the first instance of the pattern

The case-insensitive search flag `//i` will ignore any case when testing against a string for a match
- `/Wallace/gi` will match `'wallace'`, `'Wallace'` and `'WaLlACe'`

The multiline flag `//m` will allow the anchors `^` and `$` to match newline characters
- It's very helpful for matching anything at the start or end of a paragraph

The `//s` flag allows the character class dot metacharacter `.` to match newline characters

The unicode flag `//u` allows for the use of Unicode related features such as unicode point escapes which are showed later
- `/\u{65}\u{64}\u{69}\u{74}/u` would match `edit` but without the unicode flag `/\u{65}\u{64}\u{69}\u{74}/` would match u 65 times then u 64 times then u 69 times then u 74 times for a total of 272 "u"'s in a row or more likely return null
- It's very useful for when you use any unicode characters such as `/\u{00f1}/u` which will match `ñ`

The 'sticky' flag `//y`. This flag used with a regex will only attempt to match the target string from the last index that was matched and doesn't attempt to match more than once

### Character Escapes

The backslash before a character will be used as an escape if you want to match any of the special characters
- `/\$/` will match any instance of $

There are special escapes which use a backslash and literal character to provide extra functionality as listed below

##### Reserved Escapes

`\w` will match words
- anything that is a latin character or digits will be considered a word
- can be replaced by `[a-zA-Z0-9_]`
`\W` will match not words
- will match spaces, non-latin characters, dots and other unicode characters
- can be replaced by `[^a-zA-Z0-9_]`
`\d` will match digits
- equivalent to `[0-9]`
`\D` will match not digits
- equivalent to `[^0-9]`
`\b` will match a word boundary
- will check if it is a begining of a string and can match `/\w/`
- will check between two characters where one is `/\w/` and the other is `/\W/`
- `/\b/` will not work for non-latin alphabets
`\B` will match not a word boundary
`\v` will match a vertical tab character
`\f` will match a form feed character
`\0` will match a null character
`\r` will match a carriage return or line skip
`\s` will match a single whitespace character
- equivalent to `[ \n\r\t\v]`
`\S` will match a single non-whitespace character
`\x00 - \xFF` will match a hexidecimal character
`\u{0000} - \u{FFFF}` will match unicode code point only when the unicode flag is included
`\p{property}` will match unicode property escapes specified by property when the unicode flag is included
- There are many many properties some of which can be viewed on the [Unicode Website](https://unicode.org/reports/tr18/#General_Category_Property) or on this [ECMAScript website](https://tc39.es/ecma262/multipage/text-processing.html#table-binary-unicode-properties)
`\P{property}` will match anything that isn't the unicode property escape given when the unicode flag is included

## Author

This was written by Austin, I can be found at [GitHub](https://github.com/Austinfr)
