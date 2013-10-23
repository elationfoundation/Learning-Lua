Patterns
===================

Patterns are used by string functions to capture sets of charicters for parsing. Lua, being small enough to fit on a floppy disk, cannot implement regular expressions because a full implementation would more than double its size.

Character Class:
----------------

A character class is used to represent a set of characters. The following combinations are allowed in describing a character class:

    .: (a dot) represents all characters.
    %a: represents all letters.
    %c: represents all control characters.
    %d: represents all digits.
    %l: represents all lowercase letters.
    %p: represents all punctuation characters.
    %s: represents all space characters.
    %u: represents all uppercase letters.
    %w: represents all alphanumeric characters.
    %x: represents all hexadecimal digits.
    %z: represents the character with representation 0. In Lua 5.1, string patterns (as used by string.gsub, string.match, etc.) couldn't contain embedded "\0" characters, and the "%z" pattern had to be used instead. As of 5.2-work3, embedded NUL characters are now handled correctly, and "%z" is now deprecated.

The next two examples use x as a variable, not as the letter x.

	x: (where x is not one of the magic characters ^$()%.[]*+-?) represents the character x itself.
    %x: (where x is any non-alphanumeric character) represents the character x. This is the standard way to escape the magic characters. Any punctuation character (even the non magic) can be preceded by a '%' when used to represent itself in a pattern.

    [set]: represents the class which is the union of all characters in set. A range of characters can be specified by separating the end characters of the range with a '-'. All classes %x described above can also be used as components in set. All other characters in set represent themselves. For example, [%w_] (or [_%w]) represents all alphanumeric characters plus the underscore, [0-7] represents the octal digits, and [0-7%l%-] represents the octal digits plus the lowercase letters plus the '-' character.

The interaction between ranges and classes is not defined. Therefore, patterns like [%a-z] or [a-%%] have no meaning.

    [^set]: represents the complement of set, where set is interpreted as above.

For all classes represented by single letters (%a, %c, etc.), the corresponding uppercase letter represents the complement of the class. For instance, %S represents all non-space characters.

The definitions of letter, space, and other character groups depend on the current locale. In particular, the class [a-z] may not be equivalent to %l.

Simple Character Class Examples
------------------------
    -- any chars
    print(string.match("#", "."))
	==> #

	-- control chars are (tabs \t, new-lines \n, etc.)
	print(string.match("*\t*", ".%c."))
	==> *	*

	-- hexadecimal chars
    print(string.match("F", "%h"))
	==> F
	
	-- sets and compliments
    print(string.match("A2A", "[%u%d]")) --remember match returns the first match and a set is a collection to pick a match from
	==> A
    print(string.match("A2A", "[^%u%d]"))
	==> nil
	
	--plain strings
	print(string.match("bob jones", "bob")) --remember match returns the first match and a set is a collection to pick a match from
	==> bob

Pattern Item:
-------------

A pattern item can be

    a single character class, which matches any single character in the class;
    a single character class followed by '*', which matches 0 or more repetitions of characters in the class. These repetition items will always match the longest possible sequence;
    a single character class followed by '+', which matches 1 or more repetitions of characters in the class. These repetition items will always match the longest possible sequence;
    a single character class followed by '-', which also matches 0 or more repetitions of characters in the class. Unlike '*', these repetition items will always match the shortest possible sequence;
    a single character class followed by '?', which matches 0 or 1 occurrence of a character in the class;
    %n, for n between 1 and 9; such item matches a substring equal to the n-th captured string (see below);
    %bxy, where x and y are two distinct characters; such item matches strings that start with x, end with y, and where the x and y are balanced. This means that, if one reads the string from left to right, counting +1 for an x and -1 for a y, the ending y is the first y where the count reaches 0. For instance, the item %b() matches expressions with balanced parentheses.

Pattern:
---------

A pattern is a sequence of pattern items. A '^' at the beginning of a pattern anchors the match at the beginning of the subject string. A '$' at the end of a pattern anchors the match at the end of the subject string. At other positions, '^' and '$' have no special meaning and represent themselves.
Captures:

A pattern can contain sub-patterns enclosed in parentheses; they describe captures. When a match succeeds, the substrings of the subject string that match captures are stored (captured) for future use. Captures are numbered according to their left parentheses. For instance, in the pattern "(a*(.)%w(%s*))", the part of the string matching "a*(.)%w(%s*)" is stored as the first capture (and therefore has number 1); the character matching "." is captured with number 2, and the part matching "%s*" has number 3.

As a special case, the empty capture () captures the current string position (a number). For instance, if we apply the pattern "()aa()" on the string "flaaap", there will be two captures: 3 and 5.

A pattern cannot contain embedded zeros. Use %z instead.


String Pattern Examples
-----------------------

	--Capturing a full word
	a,b = string.match("Sally Jones", "(%w+)%s(%w+)")
	print(a.."'s last name is: "..b)
	==> Sally's last name is Jones

	--Why shortest capture length?
	first_name = string.match("fname lname ", "(.*)%s") --WOOPS where did that accidental space at the end of the string come from
	print(first_name)
    ==> fname lname
	first_name = string.match("fname lname ", "(.-)%s")
	print(first_name)
	==> fname
	
	--Multiple Captures
	a,b = string.match("A2A", "(%w)%d(%w)")
	print(a..b)
	==> AA

	--Anchoring captures
	first, last = string.match("This is a long string with lots of words", "^(%w+)%s.-%s(%w+)$")
	print(first.." : "..last)
	==> This : words	
	
	--matching balanced chars
	print(string.match("function(value1, value2)", "%b()"))
	==> (value1, value2)
	print(string.match("<name>Gurbanguly Berdimuhamedow</name>", "%b<>(.-)%b<>"))
	==> Gurbanguly Berdimuhamedow

	--substrings of captures (using gsub instead of match)
    str, times_found = string.gsub("<name>Gurbanguly Berdimuhamedow</name>", "<(%a+)>(.-)</%a+>", "\\%1{%2}" ) --gsub returns a second variable with the number of times a string was substituted
	print(str)
	==> \name{Gurbanguly Berdimuhamedow}