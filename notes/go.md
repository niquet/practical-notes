# Go

- [<a href="#kennedy-software-design">1</a>]
- [<a href="#kennedy-package-architecture">2</a>]
- [<a href=""></a>]
- [<a href=""></a>]

## Language mechanics

- 

### Variables

- 

## Software design in Go

- A line of code can only ever do one of three things: allocate memory, read from memory, write to memory [<a href="#kennedy-package-architecture">2</a>]
- Integrity at code level is making sure that every single read and write is accurate and efficient [<a href="#kennedy-package-architecture">2</a>]
- 

### Creating maintainable Go code

- Don't optimize code for feeling productive when you're writing code, but for being productive when you have to you pinpoint and resolve a bug or failure in production [<a href="#kennedy-software-design">1</a>]
- Design philosophies [<a href="#kennedy-software-design">1</a>]:
  - We don't make things easy to do, we make things easy to understand (e.g., instead of DRY, we may want to copy something or minimizing an abstraction)
  - Every piece of code we write must define a new semantic where one is absolutely precise (obvious things can be understood more easily)
- There are two different coding modes a Go programmer is in at all times
  - Programming mode: idioms, guidelines, styles, error handling, etc. matter less than the 20 lines of code that make something work [<a href="#kennedy-software-design">1</a>]
  - Once you find the code you need that works, you need to switch to engineering mode
  - Engineering mode: idioms, guidelines, styles, error handling, design philosophies matter and we have to orient our piece of code within the bigger picture [<a href="#kennedy-software-design">1</a>]
- Refactoring comes in stages [<a href="#kennedy-software-design">1</a>]
  - Stage in which we focus on readability
  - Stage in which we focus on precision
  - Stage in which we focus on testability
  - There cannot be one refactor for everything, but you decide on what to refactor today
 
### Go package architecture

- Clear boundaries between program components for better maintainability [<a href="#kennedy-package-architecture">2</a>]
- Packaging creates "firewalls" between the different parts of your program which the language and compiler are enforcing [<a href="#kennedy-package-architecture">2</a>]
- In Go, we are essentially building APIs instead of monolithic codebases (each API provides some sort of functionality that eventually come together to solve the business problem) [<a href="#kennedy-package-architecture">2</a>]
- Packages should provide, not contain: a package (or API) should provide something very specific to what you need [<a href="#kennedy-package-architecture">2</a>]
- Packages that contain are "utils", "common", "helpers", "models" (these packages create situations in which you cannot make changes without breaking code aka "flipping the [dependency] pyramid") [<a href="#kennedy-package-architecture">2</a>]
- Instead of having a common package of types, have a robust type system [<a href="#kennedy-package-architecture">2</a>]

## 

## Credits

<a id="kennedy-software-design"></a>[1] B. Kennedy, "Ep. 1: Mastering Software Design: Architectural Layers and Coding Modes," Ardan Labs, 15 May 2024. [Online]. Available: [https://www.ardanlabs.com/blog/2024/05/mastering-software-design-architectural-layers-and-coding-modes-ep-1.html](https://www.ardanlabs.com/blog/2024/05/mastering-software-design-architectural-layers-and-coding-modes-ep-1.html). [Accessed: 29 Jul. 2024].

<a id="kennedy-package-architecture"></a>[2] B. Kennedy, "Ep. 2: Elevating Software Design in Go: Package Architecture in Modern Development," Ardan Labs, 22 May 2024. [Online]. Available: [https://www.ardanlabs.com/blog/2024/05/elevating-software-design-in-go-package-architecture-in-modern-development-ep-2.html](https://www.ardanlabs.com/blog/2024/05/elevating-software-design-in-go-package-architecture-in-modern-development-ep-2.html). [Accessed: 29 Jul. 2024].

<a id=""></a>[3] ...
