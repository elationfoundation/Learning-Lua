Functions
============

Functions are.....

function name()
    STUFF
end

name = function() STUFF end

TODO

Multi-artgument functions
-------------------------

the elipses value (...) can be used to allow a function to take an unknown ammount of arguments.

    function many(...)
        local args ={...}
		for i,x in pairs(args) do
		    print(i,x)
		end
	end

Anonymous Functions
--------------------

Functions can also be created without naming them

TODO


Higher-order functions: Passing functions to other functions 
------------------

A function that gets another function as an argument is what we call a higher-order function. Higher-order functions are a powerful programming mechanism and the use of anonymous functions to create their function arguments is a great source of flexibility.


    tower_size = 5

    function stars (f)
      for i=1, tower_size do
         local x = (f(tower_size, i)) --Here we use the function passed to slope
         print(string.rep("*", x))
      end
    end

Now lets pass it a function for creating a sloped set of start.
	
	stars(function(x, y) return y+x end)
	==> ******
    ==> *******
    ==> ********
    ==> *********
    ==> **********

Now, lets change the function we pass to it to make a different slope.
	
	stars(function(x, y) return x-y end)
	==> ****
    ==> ***
    ==> **
    ==> *

	
String.gsub (an example of an AWESOME string based Higher Order Functions)
-----------------------------------

gsub is a string manipulation function in the "string" library. It can be called like other string functions using

    string.gsub(s, pattern, repl [,n])
	x = "hello world"
	x:gsub(pattern, repl [,n])

The long form translation of this is:

    string.gsub(string, pattern, (function, table, or string), (change the first "n" number of matches [optional]))

gsub returns a copy of the string (s) in which all (or the first n, if given) occurrences of the pattern have been replaced by a replacement string specified by repl, which can be a string, a table, or a function. gsub also returns, as its second value, the total number of matches that occurred.

The simple version uses string matching in the repl field to replace a value with the [n]th captured substring. (%0 returns everything, %1 returns the first capture )

    x = string.gsub("hello", "(%w+)", "%0")
	==> x="hello"
	
    x = string.gsub("hello world", "(%w+)", "%1 %1")
    ==> x="hello hello world world"    
	
    x = string.gsub("one two three four", "(%w+)%s*(%w+)", "%2 %1")
    ==> x="two one four three"    

BUT, gsub can also be a higher-order function taking a function as a repl value. If you do this the function passed will be called every time the gsub pattern finds a match, with all captured substrings passed as arguments.

This longer example is a play off a 1984 library. Our librarians use a gsub on all book requests. By passing gsub the "find_corruption" function that takes the identified book title  string compares it to the banned book list and, if it identifies a banned book, not so silently alerts the authorities and returns a random acceptable replacement book.

    function find_corruption(string)
		banned_books = {"Green Eggs and Ham", "The Giver", "Goodnight Moon"} --Soul twisting horror contained within
		replacements = {"Twilight", "Fifty Shades of Grey", "The Da Vinci Code", "Atlas Shrugged"} --perfectly acceptable content :)
		for _,book in pairs(banned_books) do
		    if string == book then
		        alert_authorities()
				return "<book>"..replacements[math.random(#replacements)].."</book>" --Don't use the # operator on tables that might ever have nil values in them
			end
		end	
		return string
	end

	function alert_authorities() print("INSERT POLICE SIREN NOISE HERE") end

	--Here is a sample book request with insideous content hidden within.
	request = [[
        <library>
	    <book>Great Expectations</book>
		<book>1984</book>
		<book>The Giver</book>
		<book>Heart of Darkness</book>
		</library>
		]]

	--Now we run our actual gsub function on a request
		
	cleaned_request = string.gsub(request, "<book>(.-)</book>", find_corruption)
    ==> INSERT POLICE SIREN NOISE HERE

	print(cleaned_request)
	==> <library>
	    <book>Great Expectations</book>
		<book>1984</book>
		<book>Atlas Shrugged</book>
		<book>Heart of Darkness</book>
		</library>