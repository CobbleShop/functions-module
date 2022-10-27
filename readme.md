# Functions module

## Usage

Function definition

```
function <name>([variable, ...]);endfunction
```
Variables can contain default values.
Example: `function default(&arg1="Arg1",#arg2=2,arg3=true,&array[]=["One","Two"])`



Return a value inside of an function

```
return(<value>)
```


Call a defined function

```
<name>([value, ...])
```


Alternative way of calling functions (`<name>` can contain variables)

```
CALL(<name>,[value, ...])
```


`<value>` can either be an array specifier (`&array[]`) or a normal string (`"Hello World"`, `%TIME%`), which gets expanded.

## Examples

```
function greet(&name)
	&message = "Hello %&name%!"
	log(%&message%)
	return(%&message%)
endfunction

greet("you")
```

```
function logarray(&array[])
	foreach(&array[],&content,#index)
		log(%#index% => %&content%)
	next
endfunction

unset(&test[])
&test[] = "Hello"
&test[] = "World"

logarray(&test[])
```

```
function fill(#amount,&filler="---")
	for(#i,1,%#amount%)
		&array[] = %&filler%
	next
	return(&array[])
endfunction

&filled[] = fill(5)
```

```
function data(#scores[]=[5,2,4])
	return("Alex %#scores[0]%","Sam %#scores[1]%","Kim %#scores[2]%")
endfunction

&names[] = data()
```

```
function max(...#numbers[])
	#max = 0
	
	foreach(#numbers[],#number)
		if(#number > #max)
			#max = #number
		endif
	next
	
	return(%#max%)
endfunction

#max = max(1,56,43,-10,55)
```

## Notes

Actions defined by the mod or other modules will always have the priority over functions.
That means that if you e.g. define a function named log, you cannot call the function as an action.
For those cases you can still call the functions with the `CALL` action.

## Changelog

### v0.1

 * Initial Version

### v0.2

 * Fixed arguments not getting expanded. (Issue #4)
 * Fixed passed arguments not getting parsed according to their type. (Issue #5)
 * Function names are now case-insensitive. (Issue #1)

### v0.3
 * Added `functions` iterator, which has the `FUNCTIONNAME` variable. (Issue #3)
 * Function names can now contain numbers.
 * Fixed a bug with functions which don't contain arguments.

### v0.4
 * Function arguments and return values can now be arrays. (Issue #6)
 * The `RETURN` syntax without brackets will be removed in v1.0, please use it like a normal action with brackets.
 * Functions can be called dynamically based on variables with the `CALL` action. (Issue #8)
 * `RETURN` also now allows multiple parameters, which will each get expanded and turned into an array.

### v0.5
 * Arguments can now contain default values, in case no value was provided for them by the caller.
 	* Example: `function default(&arg1="Arg1",#arg2=2,arg3=true,&array[]=["One","Two"])`
 * The last argument can be an catch-all array.
 	* Example: `function catch(...&array[])` `catch("One","Two","Three")`

### v0.5.1
 * Functions will now always return a result. If he function itself returned nothing, it will be an empty string.
 * Fixed a bug related to finding a function in higher scopes.
