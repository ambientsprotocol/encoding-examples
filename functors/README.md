# Functors

This directory contains examples of _functors_ with various strategies for distributed evaluation.

## Identity functor

[Locally evaluated identity functor](identity-functor.amb). 

Equivalent source code (JavaScript):

```javascript
let identity = (value) => () => ({ value })
let map_identity = () => (id, fn) => identity(fn(id().value))
let string_length = (str) => str.length
let program = () => map_identity()(identity("hello"), string_length)
program()
```

Ambients encoding:

```
string_length[
  in_ call.open call.(
    func[
      str[in_ arg.open arg.in int.in length.open_]|
      int[
        length[in_ str.open str]|
        in_ str
      ]|
      open_
    ]|
    open return.open_
  )
]|
map_identity[
  in_ call.open call.(
    func[
      id[in_ arg.open arg.open identity.in arg.open_]|
      arg[in_ id.open id.in func.in arg.open_]|
      func[in_ func.in_ arg.open func.in identity.open_.open func]|
      identity[
        in_ func.open func
      ]|
      open_
    ]|
    open return.open_
  )
]|
program[
  out_ call.in_ map_identity|
  out_ call.in_ string_length|
  open func.open_|
  
  call[
    out program.in map_identity.open_.return[open_.in program.in func]
  ]|
  call[
    out program.in string_length.open_.return[open_.in program.in func.in func]
  ]|
  func[in_ map_identity.open map_identity.(
    arg[
      identity[
        string[hello[]]|open_
      ]|
      in id.open_
    ]|
    func[
      arg[in_ arg.open arg.in str.open_]|
      in_ string_length.open string_length.in func.open_
    ]|
    in_ string_length.open func.open_)
  ]
]|
open program
```

Reduces to a final value:

```
identity[
  int[
    length[
      string[
        hello[]
      ]
    ]
  ]
]
```