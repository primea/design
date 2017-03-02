# PRIMA INTERFACE SPECIFICATION

# Data types

We define the following Ethereum data types:
- `bytes`: an array of bytes with unrestricted length
- `address`: a 160 bit number, represented as a 20 bytes long little endian unsigned integer in memory
- `u128`: a 128 bit number, represented as a 16 bytes long little endian unsigned integer in memory
- `u256`: a 256 bit number, represented as a 32 bytes long little endian unsigned integer in memory

We also define the following WebAssembly data types:
- `i32`: same as `i32` in WebAssembly
- `i32ptr`: same as `i32` in WebAssembly, but treated as a pointer to a WebAssembly memory offset
- `i64`: same as `i64` in WebAssembly

# API
## getAddress

Gets address of currently executing account and loads it into memory at
the given offset.

**Parameters**

-   `resultOffset` **i32ptr** the memory offset to load the address into (`address`)

**Returns**

*nothing*

## getGasLeft

Returns the current gasCounter

**Parameters**

*none*

**Returns**

`gasLeft` **i64**

## self
**Returns**
`vertexRefernce` **i32ref**

## createPort
**Parameters**
- `link` **i32ref**
- `nameDataOffset` **i32ptr** 
- `nameDatalength` **i32** 

**Returns**
`edgeReference` **i32ref**

## send_message
**Parameters**

- `portRef` **i32ref**  the port to send the message on. The `portRef` is an edge reference
on the current ports vertex
- `messageRef` **i32ref** a reference to a vertex that is being sent as a message

**Returns**

## get_current_message
**Returns**
- `messageRef` **i32ref** a reference to a vertex that is the current message being ran
