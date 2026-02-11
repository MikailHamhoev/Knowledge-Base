# Goroutines & The Go Runtime Scheduler

A goroutine is a lightweight thread managed by the Go runtime.

```
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

```
ch := make(chan int)
```

To send and get data we use channel operator <-
```
ic <- 42  // send to the channel
v := <-ic // get data from channel
```
## Zero-value of a channel
```
var ch chan byte
fmt.Println(ch)    // <nil>
```
zero-value of a channel is nil

## Example

```
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
```
chan<- is send-only channel
<-chan is receive-only channel
```

Also we can close a channel; to check if it is closed we use the ```val, ok := <-ch``` approach.

## Usage with Loops
```
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
```
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
```
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

