Tables
======

Tables are a special data type that can contain any number of values of any type. They can even contain 
tables within tables. They are very useful for organizing data by grouping values together.

    t = {}
    return t
    => table: <memory address>
    t = {1,2,3}
    return table.concat(t, ', ')
    => 1, 2, 3
    
Table as an Array
-----------------

Tables can behave like an array with numeric indexes. Unlike other programming languages the 
starting index is 1 instead of 0. You can get the max index value via the `#` prefix.

    t = {"apple", "orange", "pear"}
    return t[1]
    => apple
    return #t
    => 3

To append a new value to the array:

    t[#t+1] = "grape"
    return table.concat(t, ', ')
    => apple, orange, pear, grape
    table.insert(t, "peach")
    return table.concat(t, ', ')
    => apple, orange, pear, grape, peach
    
You can also insert a value anywhere within the array and indexes will be adjusted.

    table.insert(t, 2, "plum")
    return table.concat(t, ', ')
    => apple, plum, orange, pear, grape, peach
    
You can remove values as well.

    table.remove(t, 4)
    return table.concat(4, ', ')
    => apple, plum, orange, grape, peach
    
To sort an array by its values:

    table.sort(t)
    return table.concat(t, ', ')
    => apple, grape, orange, peach, plum

Table as an Associative Array
-----------------------------

An associative array is like a record in a database. The values in the array are associated with a key which 
is like a field name. The function table.concat does not work with associative arrays.

    t = {name="David", age=21, hobby="Lua"}
    return t.name
    => "David"
    return t["name"]
    => "David"
    
Because associative arrays do not have numeric indexes getting the table size with `#t` is not
a great idea.

    return #t
    => 0
    table.insert(t, "Hello")
    return #t
    => 1
    return t[1]
    => Hello

The # operator doesn't count all the items in the table! Instead it finds the last integer (not-fractional number) key. Because of how it's implemented, its results are undefined if all the integer keys in the table aren't consecutive (that is, don't use it for tables used as sparse arrays).

    x = {'a','b','c','d'}
	print(#x)
	==> 4
    x = {'a','b','c', nil, 'd'}
	print(#x)
	==> 5
    x = {'a','b','c', nil 'd', nil}
    print(#x)
	==> 3
    
To add a new element to our array:

    t.gender = "Male"
    return t.name .. " is a " .. t.gender
    => David is a Male
    
To remove an element from our array:

    t.gender = nil
    return t.name .. " is a " .. t.gender
    => error because t.gender is undefined

You can add table indexes in a variety of ways:

	table1 = {}
    table1['one'] = 1
	table1.two = 1
	for i,x in pairs(table1) do print(i,x) end	
	==> one 1
	==> two 1
	
	table1.two == table1['two']
	==> true

	table1['one'] == table1.one
	==> true

Using the period table.item automatically adds quotes around the item. By using the bracket table['item'] you can pass a variable or function to define the item being selected.
	
You can create your own database using tables by storing tables in a table.

    t = {}
    table.insert(t, {name="David", age=21})
    table.insert(t, {name="Richard", age=30})
    table.insert(t, {name="Jaime", age=25})
    return t[1].name
    => "David"
    return t[2].name
    => Richard
    return t[3].name
    => Jaime
	
Anything can be a key, and anything can be a value!

    fun = function () print "I am a function" end
	st = {1, 2, 3, last=4}
	WTF = {subtable=st, check_fun = fun, times_two=function(x) return x+x end}
	
	print(WTF['times_two'](4))
	==> 8
	print(WTF.times_two(4))
	==> 8

	for i,x in pairs(WTF) do print(i,x) end
	==>subtable		table: 0x8109ff0
    ==>check_fun	function: 0x8109048
    ==>times_two	function: 0x810a5b8

This can lead to some issues when using brackets and lambda functions to add a value.

    a = {pineapple="mmmm", durian="yuck"}
	b = function () return "durian" end
	a[b] = "Ok I will eat it... just this time."
	for i,x in pairs(a) do print(i,x) end

	==> pineapple		mmmm
	==> durian			yuck
    ==> function: 0x80f8270		Ok I will eat it... just this time.

The ability to use a function as a value or key in Lua tables means that you have to ensure you call any functions in an attempt to access a field in a table.

	a = {pineapple="mmmm", durian="yuck"}
	b = function () return "durian" end
	a[b()] = "lightbulb"
	for i,x in pairs(a) do print(i,x) end

	==> pineapple		mmmm
	==> durian			lightbulb

	a[function () return "new" end] = "anon func fail"
	for i,x in pairs(a) do print(i,x) end

	==> pineapple		mmmm
	==> durian			lightbulb
	==> function: 0x805f3g0		anon func fail

	a[function () return "new" end()] = "error-ville" --Seriously, this will error out.

	a[(function () return "new" end)()] = "ahhhh, finally"
	for i,x in pairs(a) do print(i,x) end

	==> pineapple		mmmm
	==> durian			lightbulb
	==> function: 0x805f3g0		anon func fail
	==> new		  ahhhh, finally
