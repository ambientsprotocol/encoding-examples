# Monoids

This directory contains examples of _monoids_ with various strategies for distributed evaluation.

[Simple locally executed monoid](simplest-monoid-construction.amb) for string concatenation, equivalent to following JS code:

```javascript
let string_concat = () => (left, right) => left + right
let program = () => string_concat()("hello", "world")
program()
```