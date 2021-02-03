# Regexs 
From regexone.com

## Intro
- Everything is a character 
- We write patterns to match a sequence of characters (a string) 
- ASCII: letters, digits, punctuation, keyboard symbols
- Unicode characters can be used to match other text

- Good practice to write specific regexs because the default behaviour finds a match anywhere in the text e.g. "success" in the string "unsuccessful"

## Metacharacters 
- blackslash indicates metacharacters 

- `\d` can be used in place of any digit from 0-9
- `\s` indicates white space 
- `\w` is equivalent to character range `[A-Za-z0-9_]` matches characters in english text

Equivalent opposites of these using capital letters:
- `\D` any non-digit
- `\S` any non-whitespace
- `\W` any non-alphanumeric character (such as punctuation)

- `\b` matches boundary between word and non-word (then we can capture the entire word with `\w+\b`)

Other:
- `.` is ANY single character. to also include the dot itself, use `\.`
- `?` denotes optionality. to use the string itself use `\?`
- `^` start of the line
- `$` end of the line 
- `( ) ` parentheses

## Matching specific characters 
- Define characters inside square brackets eg `[abc]` matches single a, b or c letter and nothing else 
- Do NOT match using `[^abc]` (i.e. EXCEPT a,b,c)
- Character ranges `[0-6]` matches single digit characters from 0-6 
- `[^n-p]` will match any character except letters n to p 
- Note character sensitivity eg `[A-Z]` different from `[a-z]`

## Matching number of characters 
- `\d\d\d` would match exactly 3 digits
- Curly braces e.g. `a{3}` will match a character exactly 3 times 
- Some regex engines will allow a range so that `a{1,3}` matches character between 1 and 3 times 
- Quantifier can be used with any character or special metacharacters eg `.{2,6}` matches any character 2-6 times 

## Kleene Star / Kleene Plus 
- Represents 0 or more or 1 or more of the character it follows 
- Kleene Star: We could use `\d*` to match any number of digits 
- Kleene Plus: or rather use `\d+` to ensure the string has at least one digit 

## Characters optional 
- `?` metacharacter marks either zero or one of the preceding character or group
- `ab?c` means the b is optional so will match `abc` or `ab`

## Whitespace
- space `␣`
- tab `\t`
- new line `\n`
- carriage return `\r`
- whitespace serial `\s` matches any whitespaces - useful for raw input text

## Start and end of the line 
- `^` start of the line
- `$` end of the line 
- `^success` to find a line that starts with success 
- combining both hat and dollar sign creates a pattern that matches whole line from beginning to end 
- Note this is different to the NOT inside a bracket `[^]` for excluding characters

## Match groups 
- parentheses `( )` captures a group of characters
- Might want to list all the image files in the cloud
- Could use pattern like `^(IMG\d+\.png)$` to capture and extract full filename, but could also store only the filename without extension as `^(IMG\d+)\.png$`
- To search for any files with the format `file_etc.pdf` can use `^(file.*)\.pdf$` where the `(file.*)` searches for file followed by anything (remember * is any number of digits, including 0), and `\.pdf$` enforces that the file ends with ".pdf"

- Nested group are in order of parentheses eg `^(IMG(\d+))\.png$` where the first capture group is the filename, and the second is the picture number
- For example to store first the dates in the form "Jan 1987, May 1969, Aug 2011" and then just the years "1987, 1969, 2011" we could use `(.*\s(\d+))` where the `\d+` matches digits of at least one length, `\s` matches whitespace, and `.*` matches everything before the whitespace

- Grouped characters can then be used together with quantifiers (`*,+,{m,n},?`)
- Eg checking for a 3 digit area code `(\d{3})?`

## Conditionals 
- Can use `|` logical OR aka the pipe to denote possible sets of characters
- Eg `Buy more (milk|bread|juice)` matches any expressions "Buy more milk/ Buy more bread/ Buy more juice"

## Back referencing 
- Can vary between systems 
- But often can refer to captured groups as `\0, \1, \2`, where `\0` is the full matched text, `\1` is group 1, `\2` is group 2, etc
- Can use this for example for search and replace : search for `(\d+)-(\d+)` and replace with `\2-\1`

## Other topics 
- Greedy vs non-greedy expressions 
- Posix notation 

## Problems 
1. Match any number in any form (including -/+, scientific notation, with commas) 
Soln : `-?\d+e?,?d+$`
Can start with - sign, then any non-zero number of digits, e is optional, comma is optional, then any number of digits, then finish the number

2. Find area codes in different formats out of 415-555-1234, (416)555-3456, 1 416 555 9292
Soln: `(\d{3})` finds first set of 4 digits 

3. Matching emails: match the name part of an email address before the @, and without any "+.." in the name 
Soln: `(.*)\+|(.*)@` separately matches the case with a + and the ones without, both preceding the @ 

4. Capturing html tags - given some html starting with `<a...` or `<div ...` just capture "a"/"div"
Soln: `<(\w{1,3})`

5. Capture image files of the form `*.jpg, *.gif, *.png`, with no double ending eg `.jpg.tmp`
Then group filename and extensions
Soln: `(.*).(gif)$|(.*).(png)$|(.*).(jpg)$`
forces the extension to be at the end of the file, and keeps string from before dot

6. Skip white space at the start and end of lines 
Soln: `^\s*(.*)\s*` matches white space at beginning and end of line, and keeps the rest 

7. Keep some names of functions / numbers in some error file 
Soln: `at widget.List.(.*)\((.*):(.*)\)`

8. Parsing/extracting data from URL 
- This one is hard 
- Had to look at their soln: `(\w+)://([\w\-\.]+)(:(\d+))?`
- The first part before the :// matches any alphanumeric characters 
- The next part looks at the host, which may contain non-alphanumeric characters like the dash or period, so we have to specify all of these `[\w\-\.]` then we want to keep it all using `+`
- Then the port is an optional part after the colon `(:(\d+))?` where the ? indicates it's optional


