# go-event
[![Go Report Card](https://goreportcard.com/badge/github.com/AlexanderGrom/go-event)](https://goreportcard.com/report/github.com/AlexanderGrom/go-event) [![GoDoc](https://godoc.org/github.com/AlexanderGrom/go-event?status.svg)](https://godoc.org/github.com/AlexanderGrom/go-event)

Go-event is a simple event system.

## Get the package
```bash
$ go get -u github.com/AlexanderGrom/go-event
```

## Examples
```go
e := New()
e.On("my.event.name.1", func() error {
    fmt.Println("Fire event")
    return nil
})

e.On("my.event.name.2", func(text string) error {
    fmt.Println("Fire", text)
    return nil
})

e.On("my.event.name.3", func(i, j int) error {
    fmt.Println("Fire", i+j)
    return nil
})

e.On("my.event.name.4", func(name string, params ...string) error {
    fmt.Println(name, params)
    return nil
})

e.Go("my.event.name.1") // Print: Fire event
e.Go("my.event.name.2", "some event") // Print: Fire some event
e.Go("my.event.name.3", 1, 2) // Print: Fire 3
e.Go("my.event.name.4", "params:", "a", "b", "c") // Print: params: [a b c]
```

A couple more examples
```go
package main

import (
	"fmt"

	"github.com/AlexanderGrom/go-event"
)

func EventFunc(text string) error {
	fmt.Println("Fire:", text, "1")
	return nil
}

type EventStruct struct{}

func (e *EventStruct) EventFunc(text string) error {
	fmt.Println("Fire:", text, "2")
	return nil
}

func main() {
	event.On("my.event.name.1", EventFunc)
	event.On("my.event.name.1", (&EventStruct{}).EventFunc)

	event.Go("my.event.name.1", "event")
	// Print: Fire event 1
	// Print: Fire event 2
}
```