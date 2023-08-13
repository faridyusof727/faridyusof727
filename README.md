```go
package main

import (
	"fmt"
)

var aboutMe = struct {
	Name                          string
	Overview                      string
	LinkedIn                      string
	FavouriteProgrammingLanguages []string
}{
	Name: "Farid Yusof",
	Overview: `Passionate, innovative, and results-driven leader who believes in continuous learning.
	A strong believer in technology as the key enabler for social change.`,
	LinkedIn: "linkedin.com/in/faridyusof727",
	FavouriteProgrammingLanguages: []string{"Go", "Typescript", "Javascript", "PHP"},

}

func main() {
	fmt.Println("Name:", aboutMe.Name)
	fmt.Println("Overview:", aboutMe.Overview)
	fmt.Println("LinkedIn:", aboutMe.LinkedIn)
	fmt.Println("Favorite Programming Languages:", aboutMe.FavouriteProgrammingLanguages)
}
```
