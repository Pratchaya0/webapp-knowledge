# GO Routine
- Asynchronous usage concept in golang (Concurrent programming concept)

## Basic Usage

### Channel
- Channel ใน Go คือ ช่องทางที่ใช้สำหรับการสื่อสารระหว่าง Goroutines ด้วยกัน โดย Channel นั้นสามารถส่งข้อมูลได้ทั้งแบบ synchronous (อันนี้ยังไม่รู้ว่าใช้ไปทำไมแต่ทำได้) และแบบ asynchronous

```bash
ch := make(chan int)
```

- Buffer channel ใน Go คือ Channel ที่มี Buffer อยู่ภายใน โดย Buffer ทำหน้าที่เก็บข้อมูลไว้ชั่วคราว ช่วยให้การส่งข้อมูลระหว่าง    Goroutines เป็นไปอย่างราบรื่นยิ่งขึ้น **Buffer channel สามารถสร้างได้โดยใช้ฟังก์ชัน make() โดยให้ argument ที่สองเป็นจำนวน Buffer ที่ต้องการ**

```bash
ch := make(chan int, 10)
```

```bash
package main

import (
  "fmt"
  "time"
)

var ch = make(chan int, 3) // booking memory

func main() {
  start1 := time.Now()
  go testI() 
  start2 := time.Now()
  go testII()
  start3 := time.Now()
  go testIII()
  
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

func testI() {
	time.Sleep(3 * time.Second)
	tmp := 10
	tmp += 1
	
	fmt.Println("func 1 res: ", tmp)
	
	ch <- tmp
}

func testII() {
	tmp := 20
	tmp += 2
	
	fmt.Println("func 2 res: ", tmp)
	
	ch <- tmp
}

func testIII() {
	time.Sleep(2 * time.Second)
	tmp := 30
	tmp += 3
	
	fmt.Println("func 3 res: ", tmp)
	
	ch <- tmp
}
```

### ข้อควรระวัง
- Race condition คือ ปัญหาที่อาจเกิดขึ้นเมื่อหลาย goroutine พยายามเข้าถึงและแก้ไขทรัพยากรหรือข้อมูลร่วมกันในเวลาเดียวกัน ปัญหานี้อาจทำให้ข้อมูลหรือผลลัพธ์ของโปรแกรมไม่ถูกต้องหรือคาดเดาไม่ได้
- Dead Lock 

![Go routine Ref.](https://docs.mikelopster.dev/c/goapi-essential/chapter-6/sync)

## Example my use case 
- Basic
```bash
package main

import (
	"fmt"
	"sync"
	"time"
)

var (
	buffer_ch   = make(chan int, 3) // Buffered channel
	initial_value int = 0
	mu sync.Mutex
	wg sync.WaitGroup
)

func main() {
	start := time.Now()
	
	// Launch goroutines
	wg.Add(3)
	go testI() 
	go testII()
	go testIII()
	
	// Close the channel after all goroutines have finished
	go func() {
		wg.Wait()
		close(buffer_ch)
	}()
	
	// Read from the channel
	for ch := range buffer_ch {
		fmt.Println("Channel:", ch)
		tmp_value := ch
		duration := time.Since(start).Seconds()
		fmt.Print(" Sum result:", tmp_value)
		fmt.Println(" Runtime:", duration)
	}
	
	// Print the final initial_value
	fmt.Println("Test:", initial_value)
}

func testI() {
	defer wg.Done()
	local := 1
	
	time.Sleep(3 * time.Second)
	mu.Lock()
	initial_value += local
	tmp := initial_value
	mu.Unlock()
	fmt.Printf("Func [1] add: %d\n", local)
	
	buffer_ch <- tmp
}

func testII() {
	defer wg.Done()
	local := 2
	
	mu.Lock()
	initial_value += local
	tmp := initial_value
	mu.Unlock()
	fmt.Printf("Func [2] add: %d\n", local)
	
	buffer_ch <- tmp
}

func testIII() {
	defer wg.Done()
	local := 3
	
	time.Sleep(2 * time.Second)
	mu.Lock()
	initial_value += local
	tmp := initial_value
	mu.Unlock()
	fmt.Printf("Func [3] add: %d\n", local)
	
	buffer_ch <- tmp
}

```
- Mutex and NewCond
```bash
package main

import (
	"fmt"
	"sync"
	"time"
)

var (
	// Condition variable and associated mutex
	mu        sync.Mutex
	cond      = sync.NewCond(&mu)
	workersDone = 0
	totalWorkers = 3 // Total number of worker goroutines
)

func main() {
	var wg sync.WaitGroup
	
	// Launch worker goroutines
	for i := 1; i <= totalWorkers; i++ {
		wg.Add(1)
		go worker(i, &wg)
	}

	// Launch coordinator goroutine
	go coordinator()

	// Wait for all workers to finish
	wg.Wait()
}

func worker(id int, wg *sync.WaitGroup) {
	defer wg.Done()
	
	// Simulate work with sleep
	fmt.Printf("Worker %d starting work\n", id)
	time.Sleep(time.Duration(id) * time.Second) // Different work times for each worker
	fmt.Printf("Worker %d finished work\n", id)
	
	// Notify that this worker is done
	mu.Lock()
	workersDone++
	if workersDone == totalWorkers {
		// If all workers are done, signal the coordinator
		cond.Broadcast()
	}
	mu.Unlock()
}

func coordinator() {
	mu.Lock()
	defer mu.Unlock()
	
	// Wait for all workers to finish
	for workersDone < totalWorkers {
		fmt.Println("Coordinator waiting for workers to finish...")
		cond.Wait()
	}
	
	// All workers are done
	fmt.Println("Coordinator: All workers are done!")
}
```


