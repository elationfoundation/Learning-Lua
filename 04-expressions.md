Expressions
===========

Lua evaluates and calculates values via expressions. An expression may use one or more values to calculate 
another value. There are two types of expressions in Lua; arithmetic and relational.

Arithmetic Operators
----------------------

Arithmetic operators are used to perform a calculation between two or more values. For example, 
`1 + 2` uses the `+` operator to add two numbers together. The expression returns a value of `3`.

Lua supports the following arithmetic operators:

* `+` (addition)
* `-` (subtraction)
* `*` (multiplication)
* `/` (division)
* `%` (modulo)
* `^` (exponentiation)
* `-` (negation)

Here are some examples:

    return 1 + 1 -- 1 plus 1
    => 2
    return 2 - 1 -- 2 minus 1
    => 1
    return 2 * 3 -- 2 multiplied by 3
    => 6
    return 5 / 2 -- 5 divided by 2
    => 2.5
    return 5 % 2 -- remainder of 5 / 2
    => 1
    return -(3 * 2)
    => -6

Relational Operators
--------------------

Relational operators are used to compare two or more values and return a `true` or `false` value. For 
example, `1 < 2` returns `true` since 1 is less than 2. `1 > 2` returns `false` since the expression is 
false.

Lua supports the following relational operators:

* `<` (less than)
* `>` (greater than)
* `<=` (less than or equal to)
* `>=` (greater than or equal to)
* `==` (equal to)
* `~=` (not equal to)

Here are some examples:

    return 1 < 2
    => true
    return 2 > 1
    => true
    return string.len("David") <= 2
    => false
    return string.len("David") >= 9
    => false
    return 2.00 == 2
    => true
    return true ~= false
    => true

Logical Operators
-----------------

Logical operators are used to evaluate boolean values. Remember that anything other than `false` and `nil`
are considered to be `true`.

Lua supports the following logical operators:

* `and`
* `or`
* `not`

Here are some examples:

    return true and false
    => false
    return "Hello" and true
    => true
    return nil or true
    => true
    return 5 or false
    => true
    return not false
    => true
    return not "Hello"
    => false

Be careful though:
  x = 1
  if x ~= 5 then print "YES" end
  if not x == 5 then print "YES" end
  if not (x == 5) then print "YES" end
  if ~(x == 5) then print "YES" end
  
    
Logical Assignments
-------------------

Logical operators are most frequently used in conditional statements, but they can also be used in variable 
assignments as well.

    y = nil
    x = y or "Hello"
    return x
    => "Hello"
    
The example above is the equivalent to `if y then x = y else x = "Hello" end`. 

    x = 25
    y = 100
    min = (x < y) and x or y
    return min
    => 25

The example above shows how we can assign the smaller value between `x` and `y` to `min`. Since `x` is less than `y`
`min` is assigned the value of `x`. If `x` was not less than `y`, `min` would be assigned the value of `y`.

This makes terniary operators kind of ugly.

	error = true
	local error_checking = error and do return "OH NOOOO!" end or next_step()

What about this situation though?
	false_me = function () return false end
    what_am_I = true and false_me() or "WHY"
	what_am_I = true and function () return false end or "WHY"
	

	
