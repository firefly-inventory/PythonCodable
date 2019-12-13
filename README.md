# PythonCodable

Swift framework to efficiently bridge objects from Python to Swift.

## Requirements

`PythonKit` requires [**Swift 5**](https://swift.org/download/) or higher.

## Usage

`PythonCodable` builds on `PythonKit` to allows you to bridge arbitrary Python objects into typed Swift structs effciently using `PythonDecoder`:

```swift
import PythonKit
import PythonCodable

// 1. Get a valid Python object:

let urllibParse = Python.import("urllib.parse")
let pythonParsedURL = urllibParse.urlparse("http://www.cwi.nl:80/%7Eguido/Python.html")

print(pythonParsedURL)               // ParseResult(scheme='http', netloc='www.cwi.nl:80'...
print(Python.type(pythonParsedURL))  // <class 'urllib.parse.ParseResult'>

// 2. Define a compatible Swift struct conforming to `Codable`:

struct ParsedURL: Codable, Equatable {
    let scheme: String
    let netloc: String
    let path: String
    let params: String?
    let query: String?
    let fragment: String?
}

// 3. Decode the Python object to the Swift strcut using `PythonDecoder`:

let parsedURL = try PythonDecoder.decode(ParsedURL.self, from: pythonParsedURL)

XCTAssertEqual(parsedURL.scheme, "http")
XCTAssertEqual(parsedURL.netloc, "www.cwi.nl:80")
XCTAssertEqual(parsedURL.path, "/%7Eguido/Python.html")
```

`PythonDecoder` supports multiple Python to Swift type conversion:

- `int` to `Int`
- `float` to `Double`
- `bool` to `Bool`
- `None` to `nil`
- `list[t]` to `Array<T>` where `t` is one of the supported types.
- `dict[k, v]` to `Dictionary<K, T>` where `k` is a `str` and `v` is one of the supported types.

### Swift Package Manager

Add the following dependency to your `Package.swift` manifest:

```swift
.package(url: "https://github.com/pvieito/PythonCodable.git", .branch("master")),
```

## References

- [`PythonKit`](https://github.com/pvieito/PythonCodable)
