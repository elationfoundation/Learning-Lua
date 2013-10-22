
Multi-artgument functions
-------------------------

the elipses value (...) can be used to allow a function to take an unknown ammount of arguments.

    function many(...)
        local args ={...}
		for i,x in pairs(args) do
		    print(i,x)
		end
	end
	