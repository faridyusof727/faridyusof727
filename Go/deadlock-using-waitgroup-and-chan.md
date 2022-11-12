# Deadlock using WaitGroup and Chan

At first glance, this code looks ok to me. Here's the code:

```go
func main() {
    mychan := make(chan string)

    text := "My Text"
    mychan <- text // DON'T DO THIS!!! Passing chan values should be within routines

    fmt.Println(<-mychan)
}
```

But it will return deadlock errors. Something like this:

```bash
fatal error: all goroutines are asleep - deadlock!
```

Then, I do something like this to overcome this error:

```go
func main() {
    mychan := make(chan string)

    go func() {
        text := "My Text"
        mychan <- text
    }()

    fmt.Println(<-mychan)
}
```

But when I try to do multiple routines:

```go
func main() {
    mychan := make(chan string)

    go func() {
        text := "My Text"
        mychan <- text
    }()

    go func() {
        text := "My Text 2"
        mychan <- text
    }()

    fmt.Println(<-mychan)
    fmt.Println(<-mychan)
}
```

It will output the value of **`My Text`** and **`My Text 2`** without following any order. The reason behind this is that go-routine executes functions concurrently. We really don't know which comes first and which comes last.

To solve this either we need to declare another variable for **`chan string`** or use a WaitGroup.

Let's see what it looks like when we introduce a new variable.

```go
func main() {
    mychan := make(chan string)
    mychan2 := make(chan string) // INTRODUCE NEW VARIABLE

    go func() {
        text := "My Text"
        mychan <- text
    }()

    go func() {
        text := "My Text 2"
        mychan2 <- text
    }()

    fmt.Println(<-mychan)
    fmt.Println(<-mychan2)
}
```

And there's another way we can solve this by using WaitGroup. Here's an example:

```go
func main() {
    mychan := make(chan string)

    var wg sync.WaitGroup
    wg.Add(1)

    go func() {
        text := "My Text"
        wg.Done() // IMPORTANT: This must write before sending value to the channel
        mychan <- text
    }()

    wg.Wait() // We are going to wait until the first routine function to finish

    go func() {
        text := "My Text 2"
        mychan <- text
    }()

    fmt.Println(<-mychan)
    fmt.Println(<-mychan)
}
```
