# Regular Expressions - Matching Hex Values

In this document, we will be examining the regular expression used to match hex values: 

`/^#?([a-f0-9]{6}|[a-f0-9]{3})$/` 

While it may look like gibberish to some folks, each component has a different function and we will be going through each of them and their functionality!

## Summary

We will be going through each component of the following regular expression: `/^#?([a-f0-9]{6}|[a-f0-9]{3})$/`
This is the regex used to match hex values, or hex color codes used in HTML and CSS.
Hex codes are 3-byte, 3 or 6-digit codes that render as a colour. When you define a hex code by only 3 digits, it is automatically rendered as a 6-digit code with each character repeated once. For example, hex codes `#a54` and `#aa5544` are equivalent and will be rendered the same.
Each pair of digits is used to define the intensity of red, green and blue within a colour, respectively. Each byte value ranges from 00 to FF, from lowest to highest intensity. For example, hex code `#ffffff` is the highest intensity of all three colours, which shows up as white on your screen. Hex code `#000000` is the lowest intensity for each colour, so it shows up as black. 

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Boundaries](#boundaries)

## Regex Components

Please note that regex are considered literals, and are therefore wrapped in slash characters. Javascript also has the option to use a RegExP constructor, which are wrapped in quotation marks.

### Anchors

#### The `^` Anchor

The `^` anchor found in the beginning of this expression is used to match a character at the beginning of a target string.

Code snippet of this expression: `^#?([a-f0-9]{6}|[a-f0-9]{3})`

In this case it's presenting an optional `#` hashtag to be at the beginning of the string, and if there is no hashtag, this anchor needs either a lower case letter between `a-f` or a digit between `0-9`, which are presented between a `{}` quantifier, which we'll talk about in a later section. 

##### Examples of The `^` Anchor:

Applying `/^a/` to a string like `abc` would match, as the first character is the lower case letter `a`. 
However, applying `/^a/` to a string like `bac` would not be a match, as the first letter is a `b`. 
In this case, the regex is looking for the lower case letter `a` as the first character.
Applying `/^a/` to a string like `Abc` would not return a match either, as regular expressions are case-sensitive.

Applying `/^[0-9]/` to a string like `9876` would match, as the anchor is looking for a first character in the number range of 0 to 9. A string like `1234` would also match.
Alternatively, applying `/^[0-9]/` to strings like `abc123` or `+098` would not be a match, as neither strings begin with a digit from 0 to 9.

#### The `$` Anchor

The `$` anchor found at the end of this expression is used to match a character at the end of a target string.

Code snippet: `([a-f0-9]{6}|[a-f0-9]{3})$`

This expression is looking to match the last character in the string to either a digit from `0-9` or a lower case letter from `a-f`.

##### Examples of The `$` Anchor

Applying `/[a-j]$/` to a string like `abc` would be a match, as the last character falls in the range of lower cletters from `a-j`.
However, applying this expression to a string like `xyz` would not return a match, as the last character `z` falls outside of the anchor's range. 
Applying `/[a-j]$/` to a string like `ABC` would not return a match, as the last character in the string is a capital letter, and the anchor is looking for a lower case letter. 

Applying `/[a-m1-9]$/` to a string like `banana` would match, as the anchor is looking either for the last character to be either a lower case letter from `a-m` or a digit from `1-9`.
Applying a string like `fox` to the same expression would not return a match, as the last character, `x`, is outside of the anchor's alphabetical range of `a-m`.
A string like `giraffe2000` would not return a match either, as the last character is `0`, which is just outside the anchor's numerical range of `1-9`.

### Quantifiers

Quantifiers are used to define the amount of times a pattern can match.

#### The `?` Quantifier

The `?`following the `#` in the expression is an optional quantifier, rendering that hashtag an an optional component. It allows the pattern to match 0 or 1 time.

Code snippet: `^#?`

Whether the first character in a string is a hashtag of not, it could potentially be a match, given it satisfies other criteria in the expression.

##### Examples of The `?` Quantifier

Applying `/colou?r/` to strings like `color` or `colour` would both return matches, as the `u` is now optional and the string will match to the expression with our without it. 

Applying `/Sep(tember)?/` to strings like `Sep`or `September` will also both return as matches.

#### The Curly Brackets Quantifier `{}`

Curly brackets are used to set a length limit on a target string. There are a few ways to set the length on a regex:

`{ y }` : The pattern must match exactly `y` amount of times.
`{ y, }` : The pattern must match a minimum of `x` amount of times.
`{ y, z }` : The pattern must match a minimum of `y` amount of times, or a maximum of `z` amount of times. 

Code Snippet: `[a-f0-9]{6}|[a-f0-9]{3}`

In the first case, to the left of the `|` symbol, this quantifier is allowing a fixed amount of 6 characters for this hex code. There must be 6 characters of either lower case letters from `a-f` or numbers from `0-9`, or a mixture of both.

In the second instance, to the right of the `|` symbol, these curly brackets are requiring a fixed amount of 3 characters within those alpabetical and numerical ranges mentioned above.

In this particular expression, the use of `|` is as an OR Operator, which we'll talk about in the next section, which means we need 6 OR 3 characters in a given string in order to return a match, not any number in between.

If there are no curly brackets to quantify the amount of times a character can repeat itself, a regex would return a match from a string of any length, given it matches up with other criteria that may be present.

##### Examples of the Curly Brackets Quantifier `{}`

If you apply `/{ 3-10 }/` to a string like `Dog`, it will return a match as it satisfies the minimum length of three characters. 
If you apply the same expression to a string like `Superfluous`, it will not return a match as the length exceeds the given range in the expression.

If you apply `/{ 5, }/` to a string like `Dog`, it will not return a match, as it does not meet the minimum length of 5 characters.
If you apply that expression to `Superfluous`, however, it will return a match because it is satisfies the minimum length and there is no maximum stated in the expression.

### OR Operator 

The `|` symbol within the hex code regex is used to make sure the given string is 3 characters OR 6 characters long.

Code snippet: `[a-f0-9]{6}|[a-f0-9]{3}`

Notice how we have both `[a-f0-9]{6}` and `[a-f0-9]{3}` separated by the given OR operator.
This signifies that the expression will return a match if a condition on either side is satisfied. In this case, it's looking for either a 6-character long string of lower case letters from `a-f` and numbers from `0-9`, OR a combination of characters from those alphabetical and numerical ranges that is only 3 characters long. Either a length of 3 or 6 will work, but a length of 4 or 5 characters will not be accepted as a match, as the 6 and 3 are each fixed values and not defining a range.

#### Another Example of The OR Operator

Take the following expression:

`/^I ( ran | walked )  to work, but I wish I had taken the ( bus | train ).$/`

Either of the following four strings will return a match:

- `I ran to work, but I wish I had taken the bus.`
- `I ran to work, but I wish I had taken the train.`
- `I walked to work, but I wish I had taken the bus.`
- `I walked to work, but I wish I had taken the train.`

However, a string like `I drove to work, but I wish I had taken the car.` would not return a match, as `car` is neither one of the options presented in the expression. 

### Character Classes

Character classes are what used to define the characters allowed in order to return a match.

Code snippet: `[a-f0-9]`

In our case, the characters allowed in the hex code are lowercase letters ranging from `a`to `f` and digits from `0` to `9`. This is the hexadecimal number system, composed of 16 characters.

#### More Examples of Character Classes

If we apply an expression like `/[asdfghjkl]/` to a string such as `poppy`, it will not return a match, since none of the letters in the given character class are present in the string. 
This expression would be looking for at least one of those letters to be present in the string, and since there is no quantifier next to it, the length of the string is not fixed to any number of characters, nor does it exclude other letters, numbers or special characters.
A string like `abc!123` would return a match, as it contains at least one of the characters within the brackets of the expression.
If we add a quantifier and different anchors and turn it into `/^[asdfghjkl]{3}$/`, then `abc!123` would not return a match, as the expression will only accept a 3-character-long string, for example `fad`, starting and ending with characters from the given set.

### Grouping and Capturing

Grouping is used in regular expressions by enclosing subpatterns in parentheses `()` in order to narrow the scope of matches, and are used with quantifers on their exterior.
Capturing is essentially referring to what's insides those parentheses, the characters denoted in the bracket expressions []. 

Code snippet: `^#?([a-f0-9]{6}|[a-f0-9]{3})$`

The hex value regex, we have what's called a capturing group. The parentheses are enclosing a specific set of characters that are allowed to be in the string. The `^` and `$` anchors are specifiying their position in the entire string, whereas the `{6}`and `{3}` quantifiers are saying how many instances those characters need to repeat themselves. We have an OR operator in the middle as well, saying that the characters in the bracket expressions can repeat themselves 3 OR 6 times, with an optional hashtag allowed at the beginning of a string.

### Bracket Expressions

Bracket expressions represent a single instance of a character included in a given character class.

Code snippet: `[a-f0-9]{3}`

Like we mentioned before, the hex code regex allowed lower case letters from `a` to `f` and numbers from `1` to `9`. That little code snippet represents a single instance of a character, and quantifiers like `{3}` decide how many times it will need to repeat itself.

#### Another Example of Bracket Expressions `[]`

Bracket expressions are known as a positive character group, as they want to include as many options as possible. For example, an expression like `/[abc]/` would need only one instance of one of those letters to qualify as a match. The presence of other characters does not matter, nor does the length of the string, since it has no quantifiers or anchors.
A string such as `fax` would match, as it contains the letter `a`. On the other hand, a string such as `superfluous` would not return a match, as it does not contain any of the letters `a`, `b` or `c`.

### Boundaries

Boundaries are used in regular expressions to assert the position of certain characters. 

Code snippets: `/^` and `$/`

The `^` anchor directly after the first slash `/` asserts that a certain character must be present at the beginning of the string to be considered a match. In our case, it can be either an optional hashtag or a hexadecimal digit.

The `$` anchor just before the closing slash `/` asserts that a certain character must be present at the end of the string in order to be a match. In the case of the hex value regex, we would need a hexidecimal digit.

#### Another Example of Boundaries

Let's say we have a regex such as `/\bhow\b/`. This expression uses `\b`, which is a word boundary assertion, at the beginning and end of the expression. It's looking to match the word `how` in its entirety, on its own as a separate word.
When applied to a string like `Hello, how are you?`, the expression would match up with the word `how` within that string, but a string like `Howdy, how are you?` would not return a match on the first `How`, as it is part of the larger word `Howdy`. It would only match up with the second instance of `how` as it is its own word.

## Author

I'm Julie, currently a student at the University of Toronto doing a bootcamp on full-stack web development. I hope to transition into a related career path when finished. It's been a great journey so far! You can check out my GitHub profile by clicking [here](https://github.com/julie-mac).



