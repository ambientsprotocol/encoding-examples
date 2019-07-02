# Functors

This directory contains examples of _functors_ with various strategies for distributed evaluation.

[Locally evaluated identity functor](identity-functor.amb), roughly equivalent to following JS code:

```javascript
let identity = (value) => () => ({ value })
let map_identity = () => (id, fn) => identity(fn(id().value))
let string_length = (str) => str.length
let program = map_identity()(identity("hello"), string_length)
program()
```
