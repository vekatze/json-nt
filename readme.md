# json-nt

A JSON parser for the [Neut](https://vekatze.github.io/neut/) programming language.

## Installation

```sh
neut get json https://github.com/vekatze/json-nt/raw/main/archive/0-1-27.tar.zst
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
define parse-json(t: text): either(json-error, json)

// Converts a JSON value into a text.
define show-json(j: json): text

// Converts a parse error into a human-readable error message.
define report(e: json-error): text

// For property-based testing
inline jsons: gen(json) {..}
```

## Example

```neut
// see source/test.nt

define zen(): unit {
  let input = " {\"key\" : 1234}" in
  match parse-json(*input) {
  | Right(j) =>
    printf("ok: {}\n", [show-json(j)]) // => ok: {"key": 1234}
  | Left(_) =>
    print("unreachable")
  }
}
```
