# Building RESTful APIs

- REST APIs are one of the most common kinds of web interfaces available today
- They allow various clients including browser apps to communicate with services via the REST API
- Therefore, it's very important to design REST APIs properly so that we won't run into problems down the road
- We have to take into account security, performance, and ease of use for API consumers
- Otherwise, we create problems for clients that use our APIs, which isn’t pleasant and detracts people from using our API
- If we don’t follow commonly accepted conventions, then we confuse the maintainers of the API and the clients that use them since it’s different from what everyone expects
- We'll look at how to design REST APIs to be easy to understand for anyone consuming them, future-proof, and secure and fast since they serve data to clients that may be confidential

- Accept and respond with JSON
- Use nouns instead of verbs in endpoint paths
- Name collections with plural nouns
- Nesting resources for hierarchical objects
- Handle errors gracefully and return standard error codes
- Allow filtering, sorting, and pagination
- Maintain Good Security Practices
- Cache data to improve performance
- Versioning APIs

- HATEOAS
- CORS
- 

---

## RESTful API Semantics

- A REST API is an application programming interface architecture style that conforms to specific architectural constraints, like stateless communication and cacheable data
- It is not a protocol or standard
- While REST APIs can be accessed through a number of communication protocols, most commonly, they are called over HTTPS, so the guidelines below apply to REST API endpoints that will be called over the internet
- 

---

## Accept and respond with JSON

---

## Use nouns instead of verbs in endpoint paths

---

## Use logical nesting on endpoints

---

## Handle errors gracefully and return standard error codes

---

## Allow filtering, sorting, and pagination

---

## Maintain good security practices

---

## Cache data to improve performance

---

## Versioning APIs

---

## Credits

- https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/
