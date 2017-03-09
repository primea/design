# Webassembly DAG interfaces
### related
https://github.com/ipld/specs/issues/35  
https://github.com/ipld/specs/issues/38  
https://github.com/ipld/ipld/issues/18  

## Overview
This note presents an initial Webassembly interface for generic Merkle DAGs. There are two interfaces proposed here. 
1) The [translator](https://www.gnu.org/software/hurd/hurd/translator.html) (resolver) API which allows the wasm VM to parse links from binary data. 
2) The Selector API allow for traversal of the graph and selecting of a subgraph 

These interfaces are **low level** and should allow for [upper layers](https://github.com/ipld/interface-ipld-format) to be implemented on top of them.

## glossary
- **Vertex**: a map of names to Edges
- **Edge**:  Contains data and/or a link
- **Link**: a merkle link

## Rational
It would be conducive to implement Selectors and Transforms in webassembly so that 
1) they can be loaded at runtime reducing the bootstrap binary size
2) enable universal implementations of Selectors and Transforms
3) reduces the trusted computing base
4) present an deterministic way to limit resource usage based on metering

### Challenges 
1) Its still the early days of Webassembly and Webassembly VM usage outside the browser is still largely uncharted territory
2) Language support is still limited to C/C++/rust although potential anything that llvm can compile can be used in a wasm VM

# API Overview
### Data types
We define the following Webassembly data types:
- `i32`: same as `i32` in Webassembly
- `i32ptr`: same as `i32` in Webassembly, but treated as a pointer to a Webassembly memory offset
- `i32ref`: same as `i32` in Webassembly, but treated as an opaque reference and should be replaced with an opaque reference after it has been [specified in Webassembly](https://github.com/WebAssembly/design/blob/master/GC.md#opaque-reference-types)
- `i64`: same as `i64` in WebAssembly


### Tables

A table named 'callback' must be exported if any callbacks are used. All callbacks functions have a parameter of a single i32 which will contain the error code of the orginal operation. If there where no errors then the return value will be 0 other wise it will be 1. All operation that involve state look ups are asynchronous 

# Translator API
A [translators](https://www.gnu.org/software/hurd/hurd/translator.html) maps some given data into method that can be consumed by the underlining IPFS implementations.

the Webassembly binary MUST export the following methods to be compatible DAG service. Like wise the host environment must also provided the following API to wasm binary that are intended to be translators. The translator's imports MUST use the namespace "translator".

## createVertex 
Creates a mutable vertex reference

**Returns**
`vertexRefence` **i32ref**

## createEdge
Given a link and some metadata this creates an edge.

**Parameters**
- `link`  **i32ref** - a vertex Refence
- `metaDataOffset` **i32ptr** 
- `metaDatalength` **i32** 

**Returns**
`edgeReference` **i32ref**

## addEdge
Adds an edge to a vertex given an edge reference and a label

**Parameters**
- edgeRef 
- labelOffset
- lableLength

## removeEdge
Removes an edge to a vertex given the edge's label

**Parameters**
- labelOffset
- lableLength

# Selector API
Selectors traverses the graph and selects some subset of vertices. The Selector's imports MUST use the namespace "selector".

## select
Adds a vertex reference to the array of reference to be returned

**parameters**
- `vertexReference` **i32ref**

## root
the root vertex of the DAG we are operating on

**Returns**

- `result` **i32ref**  an opaque reference to the root vertex

## resolve
Given an edge reference returns a vertex reference.

**Parameters**

- `link` **i32ref** a reference to a merkle link
- `callBackIndex` **i32** an index of the callback function

**Callback Signature**
- `error` **i32** reserved for error code
- `vertex` **i32ref** a reference to the resolved vertex


## getEdge
Gets an edge reference given the edge name

**Parameters**
- `namePtr` **i32prt**
- `length` **i32**

**return**
- `edgeReferance` **i32ref**

## getEdgeDataLength
Gets the metadata attached from to an edge

**Parameters**
- `edgeReference` **i32ref** an opaque reference to the edge

**return**
- `length` **i32** the length of the an edge

## getEdgeData
Gets the data from an edge and writes it to memory

**Parameters**
- `edgeReference` **i32ref** an opaque reference to the edge
- `writeOffset` **i32ptr** the memory location to write the data

## getLink
Unwrap a link from an edge

**returns**
- `link` **i32fer**

## isNullLink
checks if a link is null or not
**Parameters**
- `linkReference` **i32ref** an opaque reference to a link

**returns**
- `isNull` **i32**


# Iterator API
This enables iteration of a vertex's edges

## edges

**Return**

- `edgeIterator` **i32ref** a reference to an edge iterator

## next
Advances the iterator

**Parameters**
- `edgeItr` **i32ref**


## getEdgeName
**Parameters**
- `edgeItr` **i32ref**
- `writeOffset` **i32ptr**

## getEdgeNameLength
**Parameters**
- `edgeItr` **i32ref**

**Return**
- `length` **i32**
