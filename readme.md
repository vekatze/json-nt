# json-nt

A JSON parser for the [Neut](https://vekatze.github.io/neut/) programming language.

## Summary

```neut
// types

inline object(a): type {
  dict(text, a)
}

data json {
| Null
| Bool(bool)
| Integer(int)
| Float(float)
| Text(text)
| Object(object(json))
| Array(list(json))
}

// functions

define parse-json(t: text): either(json-error, json) {..}

define show-json(j: json): text {..}

// for property-based testing

constant jsons: gen(json) {..}
```

## Example

```neut
// see source/test.nt

import {..}

define main(): unit {
  noa::check(
    "∀ (j: json). show(parse(show(j))) == show(j)",
    jsons,
    function (j: json) {
      pin json-txt = show-json(j) in
      match parse-json(*json-txt) {
      | Right(j) =>
        pin new-json-txt = show-json(j) in
        eq-text(json-txt, new-json-txt)
      | Left(_) =>
        False
      }
    },
  )
}
```
