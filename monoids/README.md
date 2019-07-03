# Monoids

This directory contains examples of _monoids_ with various strategies for distributed evaluation.

## String Concatenation

[Simple locally executed monoid](https://github.com/ambientsprotocol/roam-examples/blob/master/monoids/locally-evaluated-string-concat.amb) for string concatenation.

Source code (JavaScript):

```javascript
let string_concat = () => (left, right) => left + right
let program = () => string_concat()("hello", "world")
program()
```

Ambients encoding:

```
string_concat[
  in_ call.open call.(
    func[
      left[in_ arg.open arg.in string.in concat]|
      right[in_ arg.open arg.in string.in concat]|
      string[
        concat[in_ left|in_ right]|
        in_ left|in_ right
      ]|
      open_
    ]|
    open return.open_
  )
]|
program[
  out_ call.in_ string_concat|
  open func.open_|

  call[
    out program.in string_concat.open_.return[open_.in program.in func]
  ]|
  func[in_ string_concat.open string_concat.(
    arg[string[hello[]]|in left.open_]|
    arg[string[world[]]|in right.open_]|
    open func.open_)
  ]
]|
open program
```

Reduces to a final value:

```
string[
  left[
    string[hello[]]
  ]|
  right[
    string[world[]]
  ]
]
```
