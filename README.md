# GoLang Tutorial <!-- omit in toc -->

This is a repository of me learning Go and jotting down what I am learning in a consise form

> **NOTE**: I used the publically available tutorial of Go by [freeCodeCamp.org on YouTube](https://www.youtube.com/channel/UC8butISFwT-Wl7EV0hUK0BQ) which can be found [here](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=8049s) and most of the points written here are available in the video itself but this document will be available for a quick reference

**Table of contents**

- [Variables](#variables)
- [Datatypes](#datatypes)
  - [Primitive Datatypes](#primitive-datatypes)
  - [Constants](#constants)
  - [Arrays](#arrays)
  - [Slices](#slices)
  - [Maps](#maps)
  - [Structs](#structs)
- [Control Flow](#control-flow)
  - [`if` statements](#if-statements)
  - [`switch` Statements](#switch-statements)
  - [`for` statements](#for-statements)

## Variables

- **Declaration**
  - `var foo int`
  - `var foo int = 42`
  - `var foo := 42`
- **Can't redeclare variables, but can shadow them**
- **All variables must be used**
- **Visibility**
  - Lower case first letter for package scope
  - Upper case first letter to Export
  - No private scope
  - Can scope variables to block
- **Naming Conventions**
  - Pascal or camelCase
    - _Capitalize acronyms (HTTP, URL)_
  - As short as reasonable
- **Type Conversions**
  - `destinationType(variable)`
  - Use strconv package for strings

## Datatypes

### Primitive Datatypes

- #### Boolean type
  - Values are true or false
  - Not an alias for other types (e.g. int)
  - Zero value is false
- #### Numeric types
  - ###### Integers
    - **Signed Integers**
      - `int` type has varying size but minimum is 32 bits
      - 8 bit (`int8`) through 64 bit (`int64`)
    - **Unsigned Integers**
      - 8 bit (`byte` and `uint8`) through 32 bit (`uint32`)
    - **Arithmetic operations**
      - Addition, subtraction, multiplication, division, remainder(or modulus)
    - **Bitwise operations**
      - AND, OR, XOR, AND-NOT
    - **Zero value is 0**
    - **Can't mix types in same family! (`uint16 + uint32` gives error)**
  - ###### Floating point numbers
    - **Follow IEEE-754 stndard**
    - **Zero value is 0**
    - **32 and 64 bit versions**
    - **Literal Styles**
      - Decimal (3.14)
      - Exponential (13e18 or 2E10)
      - Mixed (13.7e12)
    - **Arithmetic operations**
      - Addition, subtraction, multiplication, division
  - ###### Complex numbers
    - **Zero value is `0+0i`**
    - **64 and 128 bit versions** (real and imaginary parts can be either 32 bit and 64 bit each making it a total of 64 bit or 128 bit)
    - **Built-in functions**
      - `complex` - make complex number from two floats
      - `real` - get real part as float
      - `imag` - get imaginary part as float
    - **Arithmetic operations**
      - Addition, subtraction, multiplication, division
- #### Text types
  - ###### Strings
    - **UTF-8**
    - **Immutable**
    - **Can be concatenated with plus(`+`) operator**
    - **Can be converted to []byte (collection of bytes)**
  - ###### Rune
    - **UTF-32**
    - **Alias for `int32`**
    - **Special methods normally required to process**
      - e.g. strings.Reader#ReadRune

### Constants

- **Immutable but can be shadowed**
- **Replaced by the compiler at compile time**
  - Value must be calculabe at compile time
- **Named like variables**
  - PascalCase for exported constants
  - camelCase for intetrnal constants
- **Typed constants work like immutable variables**
  - Can interoperate only with same type
- **Untyped constants work like literals**
  - Can interoperate with similar types
- **Enumerated constants**
  - Special symbol `iota` allows related constants to be created easily
  - `iota` starts at `0` in each const block and increments by one
  - Watch out of constant values that match zero values for variables
  - `_` as the first assignment of const block can resovle the issue of zero value variables if zero is not desired or required, also doesn't assign memory to `_`
- **Enumerated expressions**
  - Operations that can be determined at compile time are allowed
    - _Arithmetic_
    - _Bitwise operations_
    - _Bitshifting_

### Arrays

- **Collection of items with same type**
- **Fixed size**
- **Declaration styles**
  - `a := [3]int{1, 2, 3}`
  - `a := [...]int{1, 2, 3}`
  - `var a [3]int`
- **Access via zero-based index**
  - `a := [3]int{1, 3, 5}` | `a[1] == 3`
- **`len(arrray)` function returns size of array**
- **Copies refer to different underlying data**
  - Assigning an array to another variable (e.g. `b := a`, where `a` is already defined) will make a new copy of the array in the memory
  - Any changes done to the new(or copied) array will not reflect on the original array
  - Can use pointers to assign a reference of the array to a new variable - `b := &a`

### Slices

- **Backed by an array**
- **Creation styles**
  - Slice existing array or slice
  - Literal style (`a := []int{1, 2, 3}`)
  - Via make function
    - _`a := make([]int, 10)` creates a slice with capacity and length 10_
    - _`a := make([]int, 10, 100)` creates a slice with length 10 and capacity 100_
- **`len(slice)` function returns length of slice**
- **`cap(slice)` function return length of underlying array**
- **`append()` function to add elements to slice**
  - May cause expensive copy operation if underlying array is too small
  - Generally works like `append(s, 1)` where s is a slice
  - Can also be used like `append(s, 2, 3, 4, 5)` to append multiple items at once
  - Can also append a slice if used with spread operator but not otherwise like `append(s, []int{6, 7, 8}...)`
- **Copies refer to same underlying array unlike the default array behaviour**

### Maps

- **Collection of value types that are accessed via keys**
- **Created via literals or via `make()` function**
- **Members accessed via `[key]` syntax**
  - `myMap["key"] = "value"`
- **Check for presence with `variable, ok` form of result**
  - e.g. `t, ok := myMap["key"]` will give `t` as the value of `myMap["key"]` and `ok` as true if key exists or it will give `t` as the value of 0 and `ok` as false if key does not exist
- **Multiple assignments of a map refer to the same underlying data**

### Structs

- **Collections of disparate datatypes that describe a single concept**
- **Keyed by named fields**
- **Follows the same convention of naming as simple variables**
- **Normally created as types but anonymous structs are allowed**
  - ```go
    type Person struct {
      name string
      age int
      address []string
    }
    ```
  - `person := struct{name string}{name: "John Doe"}`
- **Structs are value types**
- **No inheritance but can use composition via embedding**

  - ```go
    type Person struct {
      name string
      age int
      address []string
    }

    type Student struct {
      Person
      institution string
      grade int
    }

    s := Student{}
    s.name = "John Doe"
    s.age = 10
    s.address = []string{"221B", "Baker Street"}
    s.institution = "School of Funk"
    s.grade = 5
    ```

- **Tags can be added to struct fields to described field**

  - ```go
    import "reflect"

    type Person struct {
      name string `required max:"100"`
      age int
      address []string
    }

    p := reflect.TypeOf(Animal{})
    field, _ := t.FieldByName("name") // field == required max:"100"
    ```

## Control Flow

### `if` statements

- **Initializer**
  - ```go
    a := 1
    if a == 1 {
      fmt.Println("The number is 1")
    }
    ```
- **Comparison operators**
- **Logical operators**
- **Short circuiting**
  - When using OR operator if the first condition is true then the other condition will not be checked
  - When using AND operator if the first condition is false then the other condition will not be checked
- **`if-else` statements**
- **`if-else if` statements**
- **Equality with floats not always produce desired results**

  - ```go
    myNum := 0.123
    if myNum == math.Pow(math.Sqrt(myNum), 2) {
      fmt.Println("These are the same")
    } else {
      fmt.Println("These are different")
    }

    // OUTPUT: These are different
    ```

- **To get around this issue we can an error value**

  - ```go
    myNum := 0.123
    if math.Abs(myNum / myNum == math.Pow(math.Sqrt(myNum), 2) - 1) < 0.001 { // Dividing the 2 numbers and checking if they are within 10% of each other
      fmt.Println("These are the same")
    } else {
      fmt.Println("These are different")
    }

    // OUTPUT: These are the same
    ```

### `switch` Statements

- **Switching on a tag(variable)**
- **Cases with multiple tests**
  - Removes the necessity of fallthroughs
- **Initializers**

  - ```go
    t := 2

    switch t {
      case 1:
        fmt.Println("The number is 1")
      case 2:
        fmt.Println("The number is 2")
      default:
        fmt.Println("The number is something else")
    }
    ```

  - Does not allow overlapping of conditions

- **Switches with no tags**

  - ```go
    t := 2

    switch {
      case t == 1:
        fmt.Println("The number is 1")
      case t == 2:
        fmt.Println("The number is 2")
      default:
        fmt.Println("The number is something else")
    }
    ```

  - Allows overlapping of conditions

- **`fallthrough` keyword available**
  - This will not check for the condition in the next case and will execute the code for the same
- **`type` switches**

  - ```go
    var t interface := 2

    switch t.(type) {
      case int:
        fmt.Println("t is an int")
      case float64:
        fmt.Println("t is a float64")
      case float64:
        fmt.Println("t is a string")
      default:
        fmt.Println("t is another type")
    }
    ```

- **Breaking out early available by using `break` keyword**

### `for` statements

- **Only `for` statements available for looping, no while or do statements**
- **simple loops**
  - `for initializer; test; incrementer {}` // Normal for loops
  - `for test {}` // for loop working as a while loop; initialization and increment has to be handled separately
  - `for {}` // Infinite for loop for special cases; initialization, tests and increment has to be handled separately
- **exiting early**
  - `break` // will completely exit the loop in which it is called
  - `continue` // will exit the current iteration of the loop but will execute further tests
  - `labels` // will break completely out of the loop and go to the line after the label
    - ```go
      MYLoop:
      for i := 1; i < 5; i++ {
        for j := 1; j < 5; j++ {
          if i * j == 3 {
            break MyLoop
          }
        }
      }
      ```
- **looping over collections**
  - arrays, slices, maps, strings, channels
  - `for k, v := range collection {}`
