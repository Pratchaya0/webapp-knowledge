## Basic Usage

```bash
package main

import (
  "fmt"
  "time"
)

func main() {
  ch := make(chan int, 3) // booking memory

  start1 := time.Now()
  go testI(ch) 
  start2 := time.Now()
  go testII(ch)
  start3 := time.Now()
  go testIII(ch)
  
  v1 := <-ch 
  duration1 := time.Since(start1).Seconds()
  fmt.Println(v1)
  fmt.Println("Runtime:", duration1)
  
  v2 := <-ch
  duration2 := time.Since(start2).Seconds()
  fmt.Println(v2)
  fmt.Println("Runtime:", duration2)
  
  v3 := <-ch
  duration3 := time.Since(start3).Seconds()
  fmt.Println(v3)
  fmt.Println("Runtime:", duration3)
  
  close(ch)
}

func testI(ch chan int) {
	time.Sleep(3 * time.Second)
	tmp := 10
	tmp += 1
	
	fmt.Println("func 1 res: ", tmp)
	
	ch <- tmp
}

func testII(ch chan int) {
	tmp := 20
	tmp += 2
	
	fmt.Println("func 2 res: ", tmp)
	
	ch <- tmp
}

func testIII(ch chan int) {
	time.Sleep(2 * time.Second)
	tmp := 30
	tmp += 3
	
	fmt.Println("func 3 res: ", tmp)
	
	ch <- tmp
}
```