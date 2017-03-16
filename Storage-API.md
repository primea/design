# Storage API
The Storage API is the only way that data is persistently saved between executions of a contract. Storage has several demands

1. should work well to implement Ethereum  key/value storage
2. Be merklizable and allow for lazy merklelization (performant)
3. effective and accurately decoupling of metering (in the same way Webassembly metering injections are decoupled)

Other noteworthy goals are 

- to make it easy to implement the [POSIX file system API](https://www.gnu.org/software/libc/manual/html_node/File-System-Interface.html#File-System-Interface)
- to be able to implement the [DAG interface](https://github.com/ewasm/prima-design/blob/master/dag_interface.md) (or replace it) 

Each storage location can roughly be thought of as corresponding to a file. Locations are open by using there name.

## instance
opens a storage location give its name, returning a Storage reference

**Parameters**
- `nameOffest` **i32ptr** 
- `namelength` **i32**

**Returns**
- `storageRef` **i32ref** 

## getLength

Returns the size of the storage location. It equals to -1 if it doesn't exists.

**Parameters**
- `storageRef` **i32ref** the storage location reference

**returns**
- **i32** returns -1 if the location is empty , otherwise returns the length

## setLength

Grows or shrinks a storage location

**Parameters**
- `storageRef` **i32ref** the storage location reference
- `length` **i32** the next length

**returns**
- **i32** returns 1 on success, otherwise -1

## delete
deletes a storage location given its name
**Parameters**
- `storageRef` **i32ref** the storage location reference

**returns**
- **i32** returns 1 if the location was deleted else returns -1

## Linear Array API
Each storage location is a linear byte array.

## read
reads data from a storage location into memory
**Parameters**
- `storageRef` **i32ref** the storage location reference
- `index` **i32** the index to start reading from
- `length` **i32** how much data to read in bytes
- `memoryOffset` **i32ptr** where in memory to start writing data to

**returns**
- **i32** returns the number of bytes read. If the location was not open -1 is returned

## write
**Parameters**
- `storageRef` **i32ref** the storage location reference
- `index` **i32** the index to start writing too
- `length` **i32** how much should be written
- `memoryOffset` **i32ptr** a pointer to the memory location to start writing from


**returns**
- **i32** returns the number of bytes written to storage. If the location was not open -1 is returned
