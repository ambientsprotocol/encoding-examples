# Ambients Encoding Examples

Examples of computation abstractions using the [Robust Ambient](https://pdfs.semanticscholar.org/c847/a9bb262c87bffcae9b5e2dca5dcf88551ea2.pdf) calculus.

## Encodings

- [Monoids](https://github.com/ambientsprotocol/roam-examples/tree/master/monoids)
- [Functor](https://github.com/ambientsprotocol/roam-examples/tree/master/functors)

## Simulating Ambients

The example Ambients encodings in this repository can be run with the [AmbIcobjs](https://www-sop.inria.fr/mimosa/ambicobjs/) tool to simulate the ambient reductions and program behavior.

<img width="200" alt="Example 1" src="example1.gif">

### Setup

1. Download [ambicobj.jar](https://www-sop.inria.fr/mimosa/ambicobjs/ambicobj.jar) from the [AmbIcobjs](https://www-sop.inria.fr/mimosa/ambicobjs/) website
2. Go to the download directory and run the tool with `java -jar ambicobj.jar`
3. Once open, click the *"Robust Ambient"* icon (picture of a hand/glove)
4. Copy one of the encoding examples and paste it into the input box
5. Give the program some name (this can be whatever)
6. Drag the created "program" somewhere on the grey area (just to make it more visible)
7. Click the program icon
8. The simulation is now running and you should eventually see the fully reduced value of the program

### Example

The JavaScript program:

```js
let val1 = "hello"
let val2 = "world"
let string_concat = () => (left, right) => left + right
let program = () => {
   return string_concat()(val1, val2)
}
program()
```

Is encoded as:

```
val1[
  in_ call.open call.(
    string[hello[]|open_]|
    open return.open_
  )
]|
val2[
  in_ call.open call.(
    string[world[]|open_]|
    open return.open_
  )
]|
string_concat[
  in_ call.open call.(
    func[
      left[in_ left.open left.open string.in string.in concat]|
      right[in_ right.open right.open string.in string.in concat]|
      string[
        concat[
          in_ left|
          in_ right
        ]|
        in_ left|in_ right
      ]|
      open_
    ]|
    open return.open_
  )
]|
eval_a[
  out_ call.in_ val1|
  out_ call.in_ val2|
  out_ call.in_ string_concat|
  open func.open_|

  call[
    out eval_a.in val1.open_.return[open_.in eval_a.in func.in left]
  ]|
  call[
    out eval_a.in val2.open_.return[open_.in eval_a.in func.in right]
  ]|
  call[
    out eval_a.in string_concat.open_.return[open_.in eval_a.in func]
  ]|

  func[in_ string_concat.open string_concat.(
    left[in_ val1.open val1.in left.open_]|
    right[in_ val2.open val2.in right.open_]|
    in_ val1.in_ val2.open func.open_)
  ]
]|
open eval_a
```

Follow the instructions in [setup](#setup) and once running, wonderful things will start happening and eventually it should result in:

<img width="200" alt="Screen Shot 2019-06-03 at 18 24 32" src="https://user-images.githubusercontent.com/7499694/58813642-e0d13200-862c-11e9-8db6-e81369d4df2c.png">
