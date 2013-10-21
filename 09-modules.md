Modules
=============

Modules that you are using
----------------------------

To see the current modules you are using, as well as the lua path you can explore the "package" table.

    for k,v in pairs(package) do print(k,v) end



Making a module the old way (and the LuCI way)
----------------------------------------------

mymodule.lua:

    module("mymodule", package.seeall)

    function foo() -- create it as if it's a global function
        print("Hello World!")
    end

And it would be used like this:

    require "mymodule"
    mymodule.foo()

The way it works is it creates a new table for the module, stores it in the global named by the first argument to module, and sets it as the environment for the chunk, so if you create a global variable, it gets stored in the module table.

This would make it so the module can't see global variables (line print). One solution would be so store all needed standard functions in locals before calling module, but that can be tedious, so the solution was module's second parameter, which should be a function that's called with the module table as the parameter. package.seeall gives the module a metatable with an __index that points to the global table, so the module can now use global variables. The problem with this is that the user of the module can now see global variables through the module:

    require "mymodule"
    mymodule.foo()
    mymodule.print("example")



The correct and new way
-------------------------

Create an example file mymodule.lua with the following content:

    local mymodule = {}

    function mymodule.foo()
        print("Hello World!")
    end

    return mymodule

    mymodule = require "mymodule" --make this local in any code for speed
    mymodule.foo()

To actually reload the module, we need to remove it from the cache first:

	package.loaded.mymodule = nil

Another nice thing isthat since they're ordinary tables stored in a variable, modules can be named arbitrarily. Say we think that "mymodule" is too long to type every time we want to use a function from it:

    m = require "mymodule"
    m.foo()


