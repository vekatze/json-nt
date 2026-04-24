# json

A JSON parser for the [Neut](https://vekatze.github.io/neut/) programming language.

## Installation

```sh
neut get json https://github.com/vekatze/json-nt/raw/main/archive/0-1-50.tar.zst
```

## Types

```neut
// Represents a JSON object.
alias object(a) {
  dictionary(string, a)
}

// Represents a JSON value.
data json {
| Null
| Bool(bool)
| Integer(int)
| Float(float)
| Text(string)
| Object(object(json))
| Array(list(json))
}

// Parses a string into a JSON value.
define parse-json<c>(k: &zonk-kit(c)) -> either(parse-error, json)

// Converts a JSON value into a string.
define show-json(j: json) -> string

// Inserts or replaces a key in an object.
define insert(kvs: object(json), key: string, value: json) -> object(json)

// Looks up a key in an object.
define lookup(kvs: &object(json), key: &string) -> ?&json

// Deletes a key from an object.
define delete(k: &ord(string), kvs: object(json), key: string) -> object(json)
```

## Example

```neut
// see source/test.nt
define zen() -> unit {
  pin k = make-zonk-kit(*" {\"key\" : 1234}", Unit);
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
