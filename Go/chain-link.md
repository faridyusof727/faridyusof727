# Chain Link

In Go, you can easily create chains throughout the Struct. For example, you have an asset that needs to be tracked.

Let us define the asset as **`Sugar`**. Yeah, it's sugar the one that you drop in a cup of tea.

```go
type Sugar struct {
    previous *Sugar
    Location string
}

func main() {
    sugar := Sugar{
        Location: "Cyberjaya",
    }

    sugar1 := Sugar{
        previous: &sugar,
        Location: "Kota Bharu",
    }

    sugar2 := Sugar{
        previous: &sugar1,
        Location: "Muar",
    }

    fmt.Println("Current location:", sugar2.Location)
    fmt.Println("Previous location:", sugar2.previous.Location)
    fmt.Println("Previous previous location:", sugar2.previous.previous.Location)
}
```
