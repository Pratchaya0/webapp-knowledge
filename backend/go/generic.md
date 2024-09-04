# Generics 
- Flexibility type 

- Basic Usage
```go 
package main

import (
	"fmt"
)

func main() {
	numsInt := []int{1,2,3}
	numsFloat64 := []float64{1.1, 2.2, 3.3}
	
	sumInt := sum(numsInt)
	sumFloat64 := sum(numsFloat64)
	
	fmt.Println(sumInt)
	fmt.Println(sumFloat64)
}

type SumProps interface {
	int | float64
}

func sum[T SumProps](nums []T) T{
	var sum T
	
	for _, n := range nums {
		sum += n
	}
	
	return sum
}
```

### Generic Struct / Interface
```go
type (
    DamageProps interface {
        int | float64
    }

    Player[T DamageProps] struct {
        Name string
        Age int
        Damage T
    } 

    // แทบจะไม่ได้ใช้ เพราะ Interface หา case flexible ยาก แทบไม่มี
    Database[T DamageProps] interface {
        // ...
    }
)
```
