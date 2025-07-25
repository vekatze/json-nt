# json-nt

A JSON parser for the [Neut](https://vekatze.github.io/neut/) programming language.

## Installation

```sh
neut get json https://github.com/vekatze/json-nt/raw/main/archive/0-1-38.tar.zst
```

## Types

```neut
// Represents a JSON value
data json {
| Null
| Bool(bool)
| Integer(int)
| Float(float)
| Text(text)
| Object(dictionary(text, a))
| Array(list(json))
}

// Parses a string into a JSON value.
define parse-json(k: &zonk-kit): either(parse-error, json)

// Converts a JSON value into a text.
define show-json(j: json): text

// For property-based testing
inline jsons: gen(json) {..}
```

## Example

```neut
// see source/test.nt

define zen(): unit {
  pin k = make-zonk-kit(*" {\"key\" : 1234}");
  match parse-json(k) {
  | Right(j) =>
    pin j = show-json(j);
    print("ok: ");
    print-line(j); // => ok: {"key": 1234}
  | Left(_) =>
    print("unreachable")
  }
}
```
