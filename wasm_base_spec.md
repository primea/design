# WebAssembly Binary Base Specification
This is the base specification for a Primea WebAssembly module. All binaries must
conform to these rules. The base spec is not meant to be used as standalone.
Other interfaces should be used to add functionality.

This specification defines:
* [Entry Points](#entry-points)
* [Memory export](#memory-export)
* [Table of Callbacks export](#table-of-callbacks-export)

## Entry Points
The following functions with the speficied signatures MUST be exported by a 
valid wasm binary.

### onCreation(messageRef: i32) -> i32
This function runs once when the container is initially created. It receives a
message reference that is used to initailize the container. This message
MAY contain ports, but doesn't contain any data payload.

### onMessage(messageRef: i32) -> i32
This function runs on every message the container receives.

## Memory export
If any interface needs to read or write to memory then the memory MUST be
exported with the string "memory".

## Table of Callbacks export
A WebAssembly container should provide functionality for asynchronous functions
with callback functions. All functions that a WebAssembly binary intends to use
MUST be indexed in the callback table. The callback table MUST be exported with
the string "callbacks".
