# Goset

Goset is a basic and simple **thread safe** SET data structure implementation in
Go. Because it's thread safe, you can use it concurrently with your goroutines.

For usage see examples below or look at godoc: [Goset godoc](http://godoc.org/github.com/fatih/goset)

## Install

```bash
go get github.com/fatih/goset
```

## Examples

#### Initialization of a new Set

```go

// create a set with zero items
set := goset.New()

// ... or with some initial values 
set := goset.New("istanbul", "frankfurt", "san francisco", 1234)

```

#### Basic Operations

```go
// add items
set.Add("berlin")
set.Add("istanbul") // nothing happens if you add duplicate item

// remove item
set.Remove("frankfurt")
set.Remove("frankfurt") // nothing happes if you remove a nonexisting item

// create a new copy
other := set.Copy() 

// remove all items
set.Clear()

// number of items in the set
len := set.Size()

// return a list of items
items := set.List()

// string representation of set
fmt.Printf("set is %s", set.String())

```

#### Check Operations

```go
// check for set emptiness
if !set.IsEmpty() {

}

// check for a single item exist
if set.Has("istanbul") {

}

s := goset.New("1", "2", "3", "4", "5")
t := goset.New("1", "2", "3")


// check if they are them
if !s.IsEqual(t) {
    fmt.Println("s is not equal to t")
}

// if s contains all elements of t
if s.IsSubset(t) {
	fmt.Println("t is a subset of s")
}

// ... or if s is a superset of t
if t.IsSuperset(s) {
	fmt.Println("s is a superset of t")
}


```

#### Set Operations


```go
// let us initialize two sets with some values
a := goset.New("ankara", "berlin", "san francisco")
b := goset.New("frankfurt", "berlin")

// creates a new set with the items in a and b combined.
// [frankfurt, berlin, ankara, san francisco]
c := a.Union(b) 

// contains items which is in both a and b
// [berlin]
c := a.Intersection(b) 

// contains items which are in a but not in b
// [ankara, san francisco]
c := a.Difference(b) 

// contains items which are in one of either, but not in both.
// [frankfurt, ankara, san francisco]
c := a.SymmetricDifference(b) 

```

```go
// like Union but saves the result back into a.
a.Merge(b)

// removes the set items which are in b from a and saves the result back into a.
a.Separate(b)

```

#### Concurrent safe operations

```
package main

import (
	"fmt"
	"github.com/fatih/goset"
	"strconv"
	"sync"
)

func main() {
	var wg sync.WaitGroup // this is just for waiting until all goroutines finish

	// Initialize our Set
	s := goset.New()

	// Add items concurrently (item1, item2, and so on)
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go func(i int) {
			item := "item" + strconv.Itoa(i)
			fmt.Println("adding", item)
			s.Add(item)
			wg.Done()
		}(i)
	}

	// Wait until all concurrent calls finished
	wg.Wait()

	// You'll see that it's not ordered, because each goroutine has finished it differently
	fmt.Println(s.List())
}
```
	

