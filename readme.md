# json

A JSON parser for the [Neut](https://vekatze.github.io/neut/) programming language.

## Installation

```sh
neut get json https://github.com/vekatze/json-nt/raw/main/archive/0.2.1.tar.zst
```

## Types

```neut
// Represents a JSON object.
alias object(a) {
  array(pair(string, a))
}

// Represents a JSON value.
data json {
| Null
| Bool(bool)
| Integer(int)
| Float(float)
| Text(string)
| Object(object(json))
| Array(array(json))
}

// Creates an empty JSON object.
define empty-object<a>() -> object(a)

// Creates a JSON object with an initial capacity.
define make-object<a>() -> object(a)

// Creates an empty JSON array.
define empty-json-array() -> array(json)

// Appends a value to a JSON array.
inline append-json-array(xs: array(json), x: json) -> array(json)

// Creates a JSON array from a list.
define json-array-from-list(xs: list(json)) -> array(json)

// Parses a string into a JSON value.
define parse-json(k: &zonk-kit) ->> either(parse-error, json)

// Converts a JSON value into a string.
define show-json(j: json) -> string

// Adds a key-value pair to an object.
define insert(kvs: object(json), key: string, value: json) -> object(json)

// Looks up a key in an object.
define lookup<a>(kvs: &object(a), key: &string) ->> ?&a

// Deletes a key from an object.
define delete(kvs: object(json), key: string) -> object(json)
```

## Example

```neut
import {
  core::string.io {print-line},
  zonk::parse {make-zonk-kit},
  this::parse {parse-json},
  this::show {show-json},
}

define zen() -> unit {
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
