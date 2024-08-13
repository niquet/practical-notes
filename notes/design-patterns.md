# Design Patterns

- Design patterns are typically OOP-based
- Go is not strictly OOP
  - No inheritance
  - Weak encapsulation
- OOP-ish terminology
  - _Hierarchy_ implies a set of related types: shared interface, embedding
  - _Properties_ are a combination of getter/setter methods

## Solid

### Single responsibility principle (SRP)
- A type should have one primary responsibility
- As a result it should have only one reason to change (the reason being somehow related to its primary reponsibility)

```go
package main

import "fmt"

var entryCount = 0
type Journal struct {
  entries []string
}

func (j *Journal) AddEntry(text string) int {
  entryCount++
  entry := fmt
}

func main() {

}
```

### Open-closed principle (OCP)
### Liskov substitution principle (LSP)
### Interface segragation principle (ISP)
### Dependency inversion principle (DIP)

## Builder

- 

## Factories

- 

## Credits
