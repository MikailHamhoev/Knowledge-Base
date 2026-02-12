# Goroutines & The Go Runtime Scheduler

A goroutine is a lightweight thread managed by the Go runtime.

```go
func hello() {
    fmt.Println("Hello!")
}

func main() {
    go hello()
    fmt.Println("main")
}
```

If the main goroutine terminates then the program will be terminated and no other Goroutine will run.

We could give time for the hello goroutine to finish using the ``` time.Sleep(1 * time.Second) ``` but it is not the best idea. 

A system can run thousands or even millions of goroutines concurrently. 

Runtime handles scheduling across CPU cores providing a simple API for programmers. 

Goroutines communicate through channels.


# Channels

## Definition
Channel is a type of data used for communicating between goroutines.

Channels are type specific: 

```go
ch := make(chan int)
```

To send and get data we use channel operator <-
```go
ic <- 42  // send to the channel
v := <-ic // get data from channel
```
## Zero-value of a channel
```go
var ch chan byte
fmt.Println(ch)    // <nil>
```
zero-value of a channel is nil

## Example

```go
type Person struct {
    Name string
    Age int
}

func SendPerson(ch chan Person, p Person) {
    ch <- p
}

func main() {
    p := Person{"Bob", 21}

    ch := make(chan Person)

    go SendPerson(ch, p)

    name := (<-ch).Name
    fmt.Println(name)
}
```

## Direction
Channels are biderectional by default. But we can create one-destination only channels.
```go
chan<- is send-only channel
<-chan is receive-only channel
```

Also we can close a channel; to check if it is closed we use the ```val, ok := <-ch``` approach.

## Usage with Loops
```go
func f(ch chan int) {
    ch <- 3
    ch <- 2
    ch <- 1
    close(ch)
}

func main() {
    ch := make(chan int)
    go f(ch)
    for v := range ch {
        fmt.Println(v)
    }
}
```

## Buffered and Unbuffered
### Unbuffered
- Send blocks until someone receives
- Receive blocks until someone sends
- They meet, pass the data, and both continue
```go
done := make(chan struct{})
go func() {
    // do some work
    done<- struct{}{}
}()
<-done // wait for the worker
```
When to use:
- When you need to guarantee that the data sending and receiving happens at the same moment.

Examples of use: 
- Synchronization, signaling, handshakes.

### Buffered
- Send blocks ONLY when buffer is FULL
- Receive blocks ONLY when buffer is EMPTY
- Otherwise, operations complete immediately
```go
func main() {
    // Buffer of 3 can hold 3 tasks without blocking
    tasks := make(chan string, 3)

    // Sender doesn't wait for receiver
    tasks <- "task 1"
    tasks <- "task 2"
    tasks <- "task 3" // buffer now full

    fmt.Println("All tasks queued, doing other work...")

    // Receiver processes when ready
    fmt.Println(<-tasks) // task 1
    fmt.Println(<-tasks) // task 2
    fmt.Println(<-tasks) // task 3
}
```
# Synchtonization Essentials: WaitGroups, Mutexes & Atomic Operations

## Waigroups

```go
Add() -> how many to wait for
Done() -> one finished
Wait() -> block until all Done
```

Example:
```go
func main() {
    var wg sync.WaitGroup

    for i := 1; i <= 3; i++ {
        wg.Add(1) // Tell: "I'm starting one more"

        go func(id int) {
            defer wg.Done() // Tell: "I'm done"
            work(id)
        }(i)
    }

    wg.Wait() // Wait for ALL 3 to finish
    fmt.Println("all done!")
}

func work(id int) {
    time.Sleep(time.Second)
    fmt.Println("worker", id, "finished")
}
```

- Use `Add` Before the goroutine, never inside it (race condition)

- Always use `defer wg.Done()`, it runs even if panics

## Mutexes
```go
Lock() -> "MINE, nobody else can touch it"
Unlock() -> "OK, your turn then"
```

Example:

```go
type Counter struct {
    mu sync.Mutex
    value int
}

func (c *Counter) Increment() {
    c.mu.Lock()
    defer c.mu.Unlock() // defer immediately after Lock()
    
    c.value++
}

func (c *Counter) Value() int {
    c.mu.Lock()
    defer c.mu.Unlock()

    return c.value
}
```

- Never copy a mutex, always use pointer receivers
- `defer Unlock()` immediately, can't forget, works even if panics

## Atomic Operations
Faster than mutex, but only works for ints, uints, pointers.

Atomic operations are CPU instructions, not Go logic: LOCK XADD = LOAD+ADD+STORE as one command, so it cannot be interrupted in the middle.

```go
var counter int64
var flag int32

atomic.AddInt64(&counter, 1)

current := atomic.LoadInt64(&counter)

atomic.StoreInt32(&flag, 1)

// if flag is 0, set it to 1, and return true; 
// else return false
swapped := atomic.CompareAndSwapInt32(&flag, 0, 1)
```

- Use only for counters, flags, and simple stats
- Use mutex fo everuthing else

## All three together

```go
type Server struct {
    mu        sync.Mutex
    requests  int64
    config    map[string]string
    wg        sync.WaitGroup
}

func (s *Server) Handle() {
    atomic.AddInt64(&s.requests, 1)

    s.wg.Add(1)
    go func() {
        defer s.wg.Done()
        s.doWork() 
    }()
}

func (s *Server) doWork() {
    s.mu.Lock()
    defer s.mu.Unlock()
 
    s.config["key"] = "value"
}

func (s *Server) Shutdown() {
    s.wg.Wait()
}
```

# Graceful Cancellation

`ctx.Done() // Returns a channel that closes when cancellation happens`