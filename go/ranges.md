# Ranges

Ranges when used inside a `for` iterate over the elements of a compatible data structure returning the index and the value in each iteration.

````go
package main

import (
    "fmt"
)

func main() {
    fruits := [5]string{"banana", "apple", "pear", "lime", "orange"}
    for index, fruit := range fruits {
        fmt.Println(index, "=> ", fruit)
    }
}
````

````
$ go run range.go

0 =>  banana
1 =>  apple
2 =>  pear
3 =>  lime
4 =>  orange
````

Usually, index are not needed, since the range already returns the element of the iteration, so the variable can be omitted

````go
package main

import (
    "fmt"
)

func main() {
    fruits := [5]string{"banana", "apple", "pear", "lime", "orange"}
    for _, fruit := range fruits {
        fmt.Println(fruit)
    }
}

````

More about ranges in:

https://github.com/golang/go/wiki/Range
https://tour.golang.org/moretypes/16
https://www.tutorialspoint.com/go/go_range.htm
https://garbagecollected.org/2017/02/22/go-range-loop-internals/