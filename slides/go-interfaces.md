# Interfaces in Go

- Interfaces give programs structure and encourage design by composition
- They enable and enforce clean divisions between components
- The standardization of interfaces can set clear and consistent expectations
- Decoupling means reducing the dependencies between components and the types they use
- This leads to correctness, quality and maintainability

---

## Interface Semantics

- Interfaces allow you to group concrete data together by what the data can do
- It’s about focusing on what data can do and not what the data is
- Interfaces also help my code decouple itself from change by asking for concrete data based on what it can do
- It’s not limited to one type of data

- You must do my best to understand what data changes are coming and use interfaces to decouple my program from that change
- Interfaces should describe behavior and not state
- They should be verbs and not nouns

- Generalized interfaces that focus on behavior are best
- Interfaces with more than one method have more than one reason to change
- Interfaces that are based on nouns, tend to be less reusable, are more susceptible to change, and defeat the purpose of the interface
- Uncertainty about change is not a license to guess but a directive to STOP and learn more
- You must distinguish between code that defends against fraud vs protects against accidents

- Use an interface when:
  - Users of the API need to provide an implementation detail
  - API’s have multiple implementations they need to maintain internally
  - Parts of the API that can change have been identified and require decoupling
- Don't use an interface:
  - For the sake of using an interface
  - To generalize an algorithm
  - When users can declare their own interfaces
  - If it's not clear how the interface makes the code better

---

## Interfaces Are Valueless

---

## Implementing Interfaces

---

## Polymorphism

---

## Method Set Rules

---

## Slice of Interface

---

## Summary

---

## Examples

---

## Credits

- https://tour.ardanlabs.com/tour/eng/interfaces/1
