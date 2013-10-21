Metatables
==========

Metatables allow you to change the behavior of a table.

    t = {}
	print(getmetatable(t))   --> nil

We can use setmetatable to set or change the metatable of any table:

    t1 = {}
	setmetatable(t, t1)
	assert(getmetatable(t) == t1)

Metatables can be useful!

Make an Object
---------------------

Metatables allow you to create objects using only the existing table/metatable structure.

    local obj = {}
    obj.__index = obj

    function obj.new(x)
        local new_object = {data=x}
	    return setmetatable(new_object, obj)
    end

	function obj.get_data(self)
	    return self.data
	end

	local new_guy = obj.new("Hey You GUYS")
	print(new_guy:get_data())



Make a read only table
----------------------

    local default_color = {'color' = "pink"}
	
	function read_only (t)
	    local proxy = {}
        local mt = {       -- create metatable
            __index = t,
			__newindex = function (t,k,v)
			    error("attempt to update a read-only table", 2)
			end
			}
		setmetatable(proxy, mt)
		return proxy
    end

	defaults = read_only(default_color)

    days = readOnly{"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}
    print(days[1])
    days[2] = "Noday"
					