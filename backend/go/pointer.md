# Pointer 

- PoC
```go
package main

import (
	"fmt"
)

func main() {

	a := 1 // a
	b := &a // point to a (address) 
	c := &b // point to b (address)
	
	fmt.Println(*b) // a
	fmt.Println(*c) // (address)
	fmt.Println(**c) // a
	
}
```

# Parse param case
## basic
```go
package main

import (
	"fmt"
	"time"
)

func main() {

	start1 := time.Now()
	a := 1 // a
	
	// Way 1
	b := &a // point to a (address) 
	c := &b // point to b (address)
	
	// Way 2
	// b := new(int) // declare value
	// c := new(*int) // declare value
	// b = &a // point to a (address)
	// c = &b // point to b (address)
	
	parseParamI(b)
	parseParamI(*c)
	parseParamII(c)
	
	fmt.Println("initail value:", a)
	fmt.Println("address a:", &a)
	fmt.Println("address b:", &b)
	fmt.Println("address c:", &c)
	
	duration1 := time.Since(start1).Seconds()
  	fmt.Println("Runtime:", duration1)
}

func parseParamI(n *int) {
	*n += 1
	fmt.Println("Parse:", *n)
}

func parseParamII(n **int) {
	**n += 2
	fmt.Println("Parse:", **n)
}
```

# Advanced Pointer Use Cases in Go

## 1. **Pointer to Struct**
Pointers to structs are used to avoid copying large structs and to modify the struct's fields directly.

```go
type Person struct {
    Name string
    Age  int
}

func modifyPerson(p *Person) {
    p.Age += 1
}

func main() {
    person := Person{Name: "Alice", Age: 30}
    modifyPerson(&person)
    fmt.Println(person) // Person{Name: "Alice", Age: 31}
}
```

## 2. Pointer Arithmetic
Go does not support pointer arithmetic (like C or C++), but it's an important concept in other languages. In Go, pointer arithmetic is replaced by techniques like slices or using the `unsafe` package.

## 3. Nil Pointers and Pointer Checks
It's essential to handle `nil` pointers to avoid runtime panics. Checking for `nil` pointers is a safety measure.

```go
func safeDereference(p *int) {
    if p != nil {
        fmt.Println("Value:", *p)
    } else {
        fmt.Println("Pointer is nil")
    }
}

func main() {
    var a *int
    safeDereference(a)  // Pointer is nil
}
```

## 4. Pointer Slices
Pointers can be used with slices, especially for dynamic data structures like linked lists or trees.

```go
type Node struct {
    Value int
    Next  *Node
}

func main() {
    head := &Node{Value: 1}
    head.Next = &Node{Value: 2}
    fmt.Println(head.Next.Value) // 2
}
```

## 5. **Pointers in Concurrency**
Pointers allow sharing and modifying data between goroutines in concurrent programming.

```go
func modifySharedValue(p *int, done chan bool) {
    *p += 1
    done <- true
}

func main() {
    value := 0
    done := make(chan bool)
    
    go modifySharedValue(&value, done)
    go modifySharedValue(&value, done)
    
    <-done
    <-done
    fmt.Println(value) // Likely to be 2
}
```

## 6. **Pointer to Interface**
Interfaces are often implemented using pointers to allow methods to modify the underlying value.

```go
type Stringer interface {
    String() string
}

type MyType struct {
    value int
}

func (m *MyType) String() string {
    return fmt.Sprintf("Value: %d", m.value)
}

func main() {
    myVal := &MyType{value: 42}
    fmt.Println(myVal.String()) // Value: 42
}
```

## 7. Pointers and `unsafe` Package
The `unsafe` package allows more advanced pointer manipulation, similar to C. It can be used for tasks like casting pointers, manipulating memory directly, or converting between types unsafely.

```go
import (
    "fmt"
    "unsafe"
)

func main() {
    var i int = 42
    p := unsafe.Pointer(&i)
    p2 := (*float64)(p)
    fmt.Println(*p2) // This could lead to undefined behavior
}
```
