# Package

Every Go program is made up of packages.
Programs start running in package `main`.
By convention, the package name is the same as the last element of the import path

# Import

This code groups the imports into a parenthesized, "factored" import statement.
You can also write multiple import statements, like:

```go
import "fmt"
import "math"
```

But it is good style to use the factored import statement.
```go
import (  
    "fmt"  
    "math/rand")
```

# Exported names

In Go, a name is exported if it begins with a capital letter. For example, `Pizza` is an exported name, as is `Pi`, which is exported from the `math` package.

`pizza` and `pi` do not start with a capital letter, so they are not exported.

When importing a package, you can refer only to its exported names. Any "unexported" names are not accessible from outside the package.

```go

// instead of fmt.println
fmt.Println
```
# Function

A function can take zero or more arguments.

```go
func add(x int, y int) int {
	return x + y
}
```

When two or more consecutive named function parameters share a type, you can omit the type from all but the last.

In this example, we shortened

```go
x int, y int
```

to

```go
x, y int
```

```go
func add(x, y int) int {
	return x + y
}
```

[[Go's Declaration Syntax]]
# Multiple results

A function can return any number of results.

If you want to return multiple results, add (string, string) or any data type as a declaration

The `swap` function returns two strings.

```go
func swap(x, y string) (string, string) {
	return y, x
}
```

# Named return values

Go's return values may be named. If so, they are treated as variables defined at the top of the function.

These names should be used to document the meaning of the return values.

A `return` statement without arguments returns the named return values. This is known as a "naked" return.

Naked return statements should be used only in short functions, as with the example shown here. They can harm readability in longer functions.

```go
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}

// x=7 y=10
```

# Variables

The `var` statement declares a list of variables; as in function argument lists, the type is last.

A `var` statement can be at package or function level. We see both in this example.

```go
package main

import "fmt"

var c, python, java bool

func main() {
	var i int
	fmt.Println(i, c, python, java)
}
```

A var declaration can include initializers, one per variable.

If an initializer is present, the type can be omitted; the variable will take the type of the initializer.

```go
package main

import "fmt"

var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "no!"
	fmt.Println(i, j, c, python, java)
}
```

Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with implicit type.

**Outside a function, every statement begins with a keyword (`var`, `func`, and so on) and so the `:=` construct is not available.**

```go
package main

import "fmt"

func main() {
	var i, j int = 1, 2
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```

# Basic types

Go's basic types are

```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

The example shows variables of several types, and also that variable declarations may be "factored" into blocks, as with import statements.

The `int`, `uint`, and `uintptr` types are usually 32 bits wide on 32-bit systems and 64 bits wide on 64-bit systems. When you need an integer value you should use `int` unless you have a specific reason to use a sized or unsigned integer type.

# Zero values

Variables declared without an explicit initial value are given their _zero value_.

The zero value is:

- `0` for numeric types,
- `false` for the boolean type, and
- `""` (the empty string) for strings.

```go
package main

import "fmt"

func main() {
	var i int
	var f float64
	var b bool
	var s string
	fmt.Printf("%v %v %v %q\n", i, f, b, s)
}

```

# Type conversions

The expression `T(v)` converts the value `v` to the type `T`.

Some numeric conversions:

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

Or, put more simply:

```go
i := 42
f := float64(i)
u := uint(f)
```

Unlike in C, in Go assignment between items of different type requires an explicit conversion. Try removing the `float64` or `uint` conversions in the example and see what happens.

# Type inference

When declaring a variable without specifying an explicit type (either by using the `:=` syntax or `var =` expression syntax), the variable's type is inferred from the value on the right hand side.

When the right hand side of the declaration is typed, the new variable is of that same type:

```go
var i int
j := i // j is an int
```

But when the right hand side contains an untyped numeric constant, the new variable may be an `int`, `float64`, or `complex128` depending on the precision of the constant:

```go
i := 42           // int
f := 3.142        // float64
g := 0.867 + 0.5i // complex128
```

Try changing the initial value of `v` in the example code and observe how its type is affected.

# Constant

Constants are declared like variables, but with the `const` keyword.

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the `:=` syntax.

```go
const World = "世界"
```

Numeric constants are high-precision _values_.

An untyped constant takes the type needed by its context.

Try printing `needInt(Big)` too.

(An `int` can store at maximum a 64-bit integer, and sometimes less.)

