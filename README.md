# jsonpath_lib

[![Build Status](https://travis-ci.org/freestrings/jsonpath.svg?branch=master)](https://travis-ci.org/freestrings/jsonpath)
![crates.io](https://img.shields.io/crates/v/jsonpath_lib.svg)
![Crates.io](https://img.shields.io/crates/d/jsonpath_lib.svg?label=%60jsonpath_lib%60%20crates.io%20downloads)
![npm](https://img.shields.io/npm/dt/jsonpath-rs.svg?label=%60jsonpath-rs%60%20npm%20downloads)

`Rust` 버전 [JsonPath](https://goessner.net/articles/JsonPath/) 구현이다. `Webassembly`와 `Javascript`에서도 유사한 API 인터페이스를 제공 한다.

It is an implementation for [JsonPath](https://goessner.net/articles/JsonPath/) written in `Rust`. it provide a similar API interface in `Webassembly` and` Javascript` also.

- [Webassembly Demo](https://freestrings.github.io/jsonpath/)
- [Rust documentation](https://docs.rs/jsonpath_lib)

## Why?

To enjoy Rust!

## API

[With Javascript](#with-javascript)

- [jsonpath-wasm library](#jsonpath-wasm-library)
- [jsonpath-rs library](#jsonpath-rs-library-only-nodejs)
- [Javascript - jsonpath.select(json: string|object, jsonpath: string)](#javascript---jsonpathselectjson-stringobject-jsonpath-string)
- [Javascript - jsonpath.compile(jsonpath: string)](#javascript---jsonpathcompilejsonpath-string)
- [Javascript - jsonpath.selector(json: string|object)](#javascript---jsonpathselectorjson-stringobject)
- [Javascript - alloc_json, dealloc_json](#javascript---alloc_json-dealloc_json)
- [Javascript-wasm - examples](https://github.com/freestrings/jsonpath/wiki/Javascript-examples)

[With Rust](#with-rust)

- [jsonpath_lib library](#jsonpath_lib-library)
- [Rust - jsonpath::Selector struct](#rust---jsonpathselector-struct)
- [Rust - jsonpath::select(json: &serde_json::value::Value, jsonpath: &str)](#rust---jsonpathselectjson-serde_jsonvaluevalue-jsonpath-str)
- [Rust - jsonpath::select_as_str(json_str: &str, jsonpath: &str)](#rust---jsonpathselect_as_strjson-str-jsonpath-str)
- [Rust - jsonpath::select_as\<T: `serde::de::DeserializeOwned`\>(json_str: &str, jsonpath: &str)](#rust---jsonpathselect_ast-serdededeserializeownedjson-str-jsonpath-str)
- [Rust - jsonpath::compile(jsonpath: &str)](#rust---jsonpathcompilejsonpath-str)
- [Rust - jsonpath::selector(json: &serde_json::value::Value)](#rust---jsonpathselectorjson-serde_jsonvaluevalue)
- [Rust - jsonpath::selector_as\<T: `serde::de::DeserializeOwned`\>(json: &serde_json::value::Value)](#rust---jsonpathselector_ast-serdededeserializeownedjson-serde_jsonvaluevalue)
- [Rust - examples](https://github.com/freestrings/jsonpath/wiki/rust-examples)

[Simple time check - webassembly](https://github.com/freestrings/jsonpath/wiki/Simple-timecheck---jsonpath-wasm)

[Simple time check - native addon for NodeJs](https://github.com/freestrings/jsonpath/wiki/Simple-timecheck-jsonpath-native)

## With Javascript

### jsonpath-wasm library

*(not yet published `jsonpath-wasm`)*
```javascript
// browser
import * as jsonpath from "jsonpath-wasm";
// NodeJs
const jsonpath = require('jsonpath-wasm');
```

### jsonpath-rs library (Only NodeJS)

`jsonpath-rs` is native addon for NodeJs

```javascript
const jsonpath = require('jsonpath-rs');
```

### Javascript - jsonpath.select(json: string|object, jsonpath: string)

```javascript
let jsonObj = {
    "school": {
        "friends": [
            {"name": "친구1", "age": 20},
            {"name": "친구2", "age": 20}
        ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
    ]
};

let ret = [
    {"name": "친구3", "age": 30},
    {"name": "친구1", "age": 20}
];


let selectAsString = jsonpath.select(JSON.stringify(jsonObj), '$..friends[0]');
let selectAsObj = jsonpath.select(jsonObj, '$..friends[0]');

console.log(
    JSON.stringify(ret) == JSON.stringify(selectAsString),
    JSON.stringify(ret) == JSON.stringify(selectAsObj)
);

// => true, true
```

### Javascript - jsonpath.compile(jsonpath: string)

```javascript
let template = jsonpath.compile('$..friends[0]');

let jsonObj = {
    "school": {
        "friends": [
            {"name": "친구1", "age": 20},
            {"name": "친구2", "age": 20}
        ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
    ]
};

let ret = [
    {"name": "친구3", "age": 30},
    {"name": "친구1", "age": 20}
];

let selectAsString = template(JSON.stringify(jsonObj));
let selectAsObj = template(jsonObj);

console.log(
    JSON.stringify(ret) == JSON.stringify(selectAsString),
    JSON.stringify(ret) == JSON.stringify(selectAsObj)
);

// => true, true

let jsonObj2 = {
    "school": {
        "friends": [
            {"name": "Millicent Norman"},
            {"name": "Vincent Cannon"}
        ]
    },
    "friends": [ {"age": 30}, {"age": 40} ]
};

let ret2 = [
    {"age": 30},
    {"name": "Millicent Norman"}
];

let selectAsString2 = template(JSON.stringify(jsonObj2));
let selectAsObj2 = template(jsonObj2);

console.log(
        JSON.stringify(ret2) == JSON.stringify(selectAsString2),
        JSON.stringify(ret2) == JSON.stringify(selectAsObj2)
);

// => true, true
```
    
### Javascript - jsonpath.selector(json: string|object)
    
```javascript
let jsonObj = {
    "school": {
        "friends": [
            {"name": "친구1", "age": 20},
            {"name": "친구2", "age": 20}
        ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
    ]
};

let ret1 = [
    {"name": "친구3", "age": 30},
    {"name": "친구1", "age": 20}
];

let ret2 = [
    {"name": "친구4"},
    {"name": "친구2", "age": 20}
];

let selector = jsonpath.selector(jsonObj);
// or as json string 
// let selector = jsonpath.selector(JSON.stringify(jsonObj));

let select1 = selector('$..friends[0]');
let select2 = selector('$..friends[1]');

console.log(
    JSON.stringify(ret1) == JSON.stringify(select1),
    JSON.stringify(ret2) == JSON.stringify(select2)
);

// => true, true
```

### Javascript - alloc_json, dealloc_json

*(not supported in `jsonpath-rs`)*

wasm-bindgen은 Javascript와 Webassembly 간 값을 주고받을 때 JSON 객체는 String으로 변환되기 때문에, 반복해서 사용되는 JSON 객체를 Webassembly 영역에 생성해 두면 성능에 도움이 된다.

Since wasm-bindgen converts JSON objects to String when exchanging values between Javascript and Webassembly, it is helpful to create repeated Json objects in Webassembly area.

```javascript
const jsonpath = require('@nodejs/jsonpath-wasm');

let jsonObj = {
    "school": {
        "friends": [
            {"name": "친구1", "age": 20},
            {"name": "친구2", "age": 20}
        ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
    ]
};

// allocate jsonObj in webassembly
let ptr = jsonpath.alloc_json(jsonObj);

// `0` is invalid pointer
if(ptr == 0) {
    console.error('invalid ptr'); 
}

let path = '$..friends[0]';
let template = jsonpath.compile(path);
let selector = jsonpath.selector(jsonObj);
// create selector as pointer
let ptrSelector = jsonpath.selector(ptr);

let ret1 = selector(path)
let ret2 = ptrSelector(path)
let ret3 = template(jsonObj);
// select as pointer
let ret4 = template(ptr);
let ret5 = jsonpath.select(jsonObj, path);
// select as pointer
let ret6 = jsonpath.select(ptr, path);

console.log(
    JSON.stringify(ret1) == JSON.stringify(ret2),
    JSON.stringify(ret1) == JSON.stringify(ret3),
    JSON.stringify(ret1) == JSON.stringify(ret4),
    JSON.stringify(ret1) == JSON.stringify(ret5),
    JSON.stringify(ret1) == JSON.stringify(ret6));

// => true true true true true

jsonpath.dealloc_json(ptr);
```

## With Rust

### jsonpath_lib library

```rust
extern crate jsonpath_lib as jsonpath;
#[macro_use]
extern crate serde_json;
```

### Rust - jsonpath::Selector struct

```rust
#[derive(Serialize, Deserialize, PartialEq, Debug)]
struct Friend {
    name: String,
    age: Option<u8>,
}

let json_obj = json!({
    "school": {
        "friends": [
            {"name": "친구1", "age": 20},
            {"name": "친구2", "age": 20}
        ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
]});

let mut selector = Selector::new();

let result = selector
    .path("$..[?(@.age >= 30)]").unwrap()
//    .value_from_str(&serde_json::to_string(&json_obj).unwrap() /*&str*/).unwrap()
//    .value_from(&json_obj /*&impl serde::ser::Serialize*/).unwrap()
    .value((&json_obj /*serde_json::value::Value*/ ).into()).unwrap()
    .select_to_value().unwrap();

assert_eq!(json!([{"name": "친구3", "age": 30}]), result);

let result = selector.select_to_str().unwrap();
assert_eq!(r#"[{"name":"친구3","age":30}]"#, result);

let result = selector.select_to::<Vec<Friend>>().unwrap();
assert_eq!(vec![Friend { name: "친구3".to_string(), age: Some(30) }], result);
```

### rust - jsonpath::select(json: &serde_json::value::Value, jsonpath: &str)

```rust
let json_obj = json!({
    "school": {
        "friends": [
            {"name": "친구1", "age": 20},
            {"name": "친구2", "age": 20}
        ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
]});

let json = jsonpath::select(&json_obj, "$..friends[0]").unwrap();

let ret = json!([
    {"name": "친구3", "age": 30},
    {"name": "친구1", "age": 20}
]);
assert_eq!(json, ret);
```

### Rust - jsonpath::select_as_str(json: &str, jsonpath: &str)

```rust
let ret = jsonpath::select_as_str(r#"
{
    "school": {
        "friends": [
                {"name": "친구1", "age": 20},
                {"name": "친구2", "age": 20}
            ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
    ]
}
"#, "$..friends[0]").unwrap();

assert_eq!(ret, r#"[{"name":"친구3","age":30},{"name":"친구1","age":20}]"#);
```

### Rust - jsonpath::select_as\<T: `serde::de::DeserializeOwned`\>(json: &str, jsonpath: &str)

```rust
#[derive(Deserialize, PartialEq, Debug)]
struct Person {
    name: String,
    age: u8,
    phones: Vec<String>,
}

let ret: Person = jsonpath::select_as(r#"
{
    "person":
        {
            "name": "Doe John",
            "age": 44,
            "phones": [
                "+44 1234567",
                "+44 2345678"
            ]
        }
}
"#, "$.person").unwrap();

let person = Person {
    name: "Doe John".to_string(),
    age: 44,
    phones: vec!["+44 1234567".to_string(), "+44 2345678".to_string()],
};

assert_eq!(person, ret);
```

### Rust - jsonpath::compile(jsonpath: &str)

```rust
let mut template = jsonpath::compile("$..friends[0]");

let json_obj = json!({
    "school": {
        "friends": [
            {"name": "친구1", "age": 20},
            {"name": "친구2", "age": 20}
        ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
]});

let json = template(&json_obj).unwrap();

let ret = json!([
    {"name": "친구3", "age": 30},
    {"name": "친구1", "age": 20}
]);

assert_eq!(json, ret);
```

### Rust - jsonpath::selector(json: &serde_json::value::Value)

```rust
let json_obj = json!({
    "school": {
        "friends": [
            {"name": "친구1", "age": 20},
            {"name": "친구2", "age": 20}
        ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
]});

let mut selector = jsonpath::selector(&json_obj);

let json = selector("$..friends[0]").unwrap();

let ret = json!([
    {"name": "친구3", "age": 30},
    {"name": "친구1", "age": 20}
]);

assert_eq!(json, ret);

let json = selector("$..friends[1]").unwrap();

let ret = json!([
    {"name": "친구4"},
    {"name": "친구2", "age": 20}
]);

assert_eq!(json, ret);
```

### Rust - jsonpath::selector_as\<T: `serde::de::DeserializeOwned`\>(json: &serde_json::value::Value)

```rust
let json_obj = json!({
    "school": {
       "friends": [
            {"name": "친구1", "age": 20},
            {"name": "친구2", "age": 20}
        ]
    },
    "friends": [
        {"name": "친구3", "age": 30},
        {"name": "친구4"}
]});

#[derive(Serialize, Deserialize, PartialEq, Debug)]
struct Friend {
    name: String,
    age: Option<u8>,
}

let mut selector = jsonpath::selector_as::<Vec<Friend>>(&json_obj);

let json = selector("$..friends[0]").unwrap();

let ret = vec!(
    Friend { name: "친구3".to_string(), age: Some(30) },
    Friend { name: "친구1".to_string(), age: Some(20) }
);
assert_eq!(json, ret);

let json = selector("$..friends[1]").unwrap();

let ret = vec!(
    Friend { name: "친구4".to_string(), age: None },
    Friend { name: "친구2".to_string(), age: Some(20) }
);

assert_eq!(json, ret);
```