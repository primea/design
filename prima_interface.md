# PRIMA INTERFACE SPECIFICATION

# Data types

- `i32`: same as `i32` in WebAssembly
- `i32ptr`: same as `i32` in WebAssembly, but treated as a pointer to a WebAssembly memory offset
- `i64`: same as `i64` in WebAssembly

# API
## getAddress

Gets address of currently executing account and loads it into memory at
the given offset.

**Parameters**

-   `resultOffset` **i32ptr** the memory offset to load the address into (`address`)

## getGasLeft

Returns the current gasCounter

**Parameters**

*none*

**Returns**

`gasLeft` **i64**

## self
**Returns**
`vertexRefernce` **i32ref**

**Returns**
`edgeReference` **i32ref**

## sendMessage
**Parameters**

- `portRef` **i32ref**  the port to send the message on. The `portRef` is an edge reference
on the current ports vertex
- `messageRef` **i32ref** a reference to a vertex that is being sent as a message

**Returns**

## getCurrentMessage
**Returns**
- `messageRef` **i32ref** a reference to a vertex that is the current message being ran
