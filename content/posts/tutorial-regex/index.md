+++
title = "Regular Expression Tutorial"
date = 2019-03-01
[taxonomies]
tags = ["tutorial"]
+++

[https://www.lynda.com/Regular-Expressions-tutorials](https://www.lynda.com/Regular-Expressions-tutorials)

### 1

* There are different types of RegEx engines that will behave slightly different than others.

### 2

* Literal characters

* Regular expressions are eager - important to keep in mind. It wants to return a match as fast as possible. Meta characters

* Characters with special meaning:

	```
	\ . * + - {} [] ^ $ | ? () : ! =
	```

	* A wildcard match:

	```
	.
	```

	* Escape the next character:

	```
	\
	```

	Escaping characters like this is generally used inside programming languages. Take note of the slash period.

	```
	/\9.00/ matches "9.00" but not "9500" or "9-00"
	```

	```
	\/home\/user\/document\/text\.txt
	```	

* Other special characters:

	* Tabs:

	```
	\t
	```

	* Line returns:

	```
	\r \n \r\n
	```

	* Non-printable characters. Not commonly used. Bell, escape, form feed, vertical tab:

	```
	\a \e \f \v
	```

	* ASCII or ANSI Codes

### 3

* Defining a character set

* Match any one of several characters but only one character

	```
	[]
	```

* This does not match "great".

	```
	/gr[ea]t/
	```

* Character ranges

	* Match zero to nine:

	```
	[0-9]
	```

* Negative character sets

	* Do not match character:

	```
	^
	```

	* Matches any one non-vowel:

	```
	/[^aeiou]/
	```

	* Matches "seek" and "sees" but not "seem" or "seen" or "see". "See " is a match so be aware

	```
	/see[^mn]/ 
	```

* Metacharacters inside character sets

	* Metacharacters inside character sets are already escaped with the exception of `[ - ^`

* Shorthand character sets

	* Digit:
	
	```
	\d or [0-9]
	```

	* Word character:

	```
	\w or [a-zA-Z0-9_]
	```

	* Whitespace:

	```
	\s or [ \t\r\n]
	```

	* Not digit:

	```
	\D or [^0-9]
	```

	* Not word character:

	```
	\W or [^a-zA-Z0-9_]
	```

	* Not whitespace:

	```
	\S or [^ \t\r\n]
	```

### 4

* Repetition metacharacters

	* Preceding item zero or more times:

	```
	*
	```

	* Preceding item one or more times:

	```
	+
	```

	* Preceding item zero or more times:

	```
	?
	```

	* Repetition example 1:

	```
	/apples*/ matches "apple", "apples", and "applessssss"
	```

	* Repetition example 2:

	```
	/apples+/ matches "apples" and "applessssss"
	```

	* Repetition example 3:

	```
	/apples?/ matches "apple" and "apples"
	```

* Quantified repetition

	* `min` must be included, `max` is optional:

	```
	{min,max}
	```

	* Matches numbers with 4 to 8 digits:

	```
	\d{4,8}
	```

	* Matches numbers with exactly 4 digits:

	```
	\d{4}
	```

	* Matches numbers with 4 or more digits:

	```
	\d{4,}
	```

* Greedy expressions

	* Regular expressions are greedy - important to keep in mind.

	* Greedy strategy - match as much as possible before giving control to the next expression part

* Lazy expressions

	```
	*?, +?, {min,max}?, ??
	```

	* Lazy strategy - match as little as possible before giving control to the next expression part

	```
	/\w*?\d{3}/
	```

	```
	/[A-Za-z-]+?./
	```
	
	```
	/.{4-8}?_.{4,8}/
	```

* Using repetition efficiently

	* Efficient matching + less backtracking = speedy results

	* Define the quantity of repeated expressions

		* `/.+/` is faster than `/.*/`

		* `/.{5}/` and `/.{3,7}/` are even faster

* Narrow the scope of the repeated expression

	* `/.+/` can become `/[A-Za-z]+/`

* Provide clearer starting and ending points

	* `/<.+>/` can become `/<[^>]+>/`

### 5: Grouping and Alternation

* Grouping metacharacters

	* Grouping

	```
	( )
	```

	* Apply repetition operators to a group

	* Makes expressions easier to read

	* Captures group for use in matching and replacing
	
	* Cannot be used inside character set

	* `/(abc)+/` matches "abc" and "abcabcabc"

	* `/(in)?dependent/` matches "independent" and "dependent"

* Alternation metacharacter

	* OR operator:

	```
	| -
	```

	* Match expression on left or right side

	* Ordered, leftmost expression gets precedence. If first expression matches, it will stop searching.

	* Multiple choices can be daisy-chained

	* `/apple|orange/` matches "apple" and "orange"

	* `/abc|def|ghi|jkl/` matches "abc", "def", "ghi", and "jkl"

	* `/w(ei|ie)rd/` matches "weird" and "wierd"

* Writing local and efficient alternations

	* `peanut(butter)?` matches "peanutbutter" and not peanut first because (butter) is greedy.

	* `(\w+|FY\d{4}_report.xls)` matches "FY2003_report.xls" using the first expression `w+` because it's greedy. It doesn't understand the second expression even though it's more precise

	* `/xyz|abc|def|ghi/` matches "abc" first and not "xyz" because it doesn't scan the entire string. It will scan the first few characters first and see if it matches the first alternation

	* Put simplest (most efficient) expression first

		* `/\w+_\d{2,4}|\d{4}_\d{2}_\w+|export\d{2}/` (inefficent,backtrack) vs `/export\d{2}|\d{4}_\d{2}_\w+|\w+_\d{2,4}/` (efficient,simple)

* Repeating and nesting alternations

	* Nesting means when you have an expression that looks like `(apple(juice|sauce))` and not `(applejuice|applesauce)`

	* Repeating alternations: first matched alternation does not affect the next matches meaning.

		* `(AA|BB|CC){6}` matches "AABBCCAABBCC"

### 6: Anchored Expressions

* Start and end anchors

	* Reference a position, not an actual character
	
	* Start of string/line:

	```
	^
	```

	* End of string/line:

	```
	$
	``` 

	* Start of string, never end of line (not supported by some programming languages):

	```
	\A
	```

	* End of string, never end of line (not supported by some programming languages):

	```
	\Z
	```

	```
	/^apple/ or /\Aapple/

	/apple$/ or /apple\Z/

	/^apple$/ or /\Aapple\Z/
	```

	* If you have `^` and `$` in a single expression, it will have to match the whole string

* Line breaks and multiline mode

	* `/[a-z]+/` matches the first line on the list but does the next lines after it. This is due to being in single line mode

		* To match multiple lines, you need to add `/m` to end of line. Just like adding a global (/g) switch.

* Word boundaries

	* Word boundaries references a position, not a character.

	* Word boundary (start/end word boundary):

	```
	\b
	```

	* Not a word boundary:

	```
	\B
	```

	* Watch out, it doesn't match certain characters such as `.` or `-` or spaces unless specified

	* `/\b/w+s\b/` matches "apples" - This expression is efficient because you are exactly specifying the boundaries.

	* `/apples\b` `\band\b` `\boranges/` matches "apples and oranges"

### 7: Capturing groups and Backreferences

* Backreferences

	* Allow access to captured data

	* How it works: `/a(p{2}l)e/` matches "apple" and the regex engine will store "ppl" automatically.

	* `\1` through `\9`

	* `/(apple)` to `\1/` matches "apples to apples"

	* `/<(i|em)>.+?</\1>/` matches <i>Hello</i> and <em>Hello</em>

* Non-capturing group expressions

	* Turns off capture and backreferences to optimize for speed and preserve space for captures

	```
	?:
	```

### 8: Lookaround Assertions

* Positive lookahead assertions

	* Positive lookahead assertion

	```
	?=
	```

	* `/(?=seashore)sea/` matches "sea" in "seashore" but not "seaside"

	* `/\b[A-Za-z']+\b(?=,)` matches words with commas after it but does not capture the comma

	* This is a good way to sift through large documents for specific words with symbols before or after it

* Double-testing with lookahead assertions

	* `(?=^[0-5-]+$)(?=.*4321)\d{3}-\d{3}-\d{4}` - matches "555-302-4321" and not "555-245-1312". Pay attention how many times it goes through and check the line

* Negative lookahead assertions

	* Negative lookahead assertion:

	```
	?!
	```

	* Used to rule out cases you specific to not search for

	* `/(?!seashore)sea/` matches "sea" in "seaside" but not "seashore"

	* `/online (?!.*training)/` does not match "online video training"

* Lookbehind assertions

	* Lookbehind positive assertion:

	```
	?<=
	```

	* Lookbehind negative assertion:

	```
	?<!
	```

	* `/(?<=base)ball/` matches the "ball" in "baseball" but not "football"

	* `/(?!=base)ball/` matches the "ball" in "football" but not "baseball"

	* Alternation only with fixed-length items

		* Allowed: `(?<=cat|dog|rat)`

		* Not allowed: `(?<=apple|banana|plum)`

* The power of positions

	* Zero-width means zero position movement

	* `/(?<![$\d])(?=\d+\.\d\d)/` - doesn't match either "54.00" or "$54.00". It moves the cursor infront of the 5 instead, no insertion of characters or anything, just the cursor. This is an ideal situation where you don't need to do a text replacement but instead text insertion like commas between large numbers 123,123,123.00 rather than 123123123.00

### 9: Unicode and Multibyte characters

* Briefly glanced at it, not interested as it doesn't apply to me yet. Never had an experience with unicode issues yet.