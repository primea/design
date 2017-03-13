# Storage API
The Storage API is the only way that data is persistently saved between executions of a contract. Storage has several demands

1. should work well to implement ethereum  key/value storage
2. Be merklizable and allow for lazy merklelization (perfomant)
3. effective and accurately decoupling of metering (in the same way wasm metering injection are decoupled)

Other noteworthy goals are 

- to make it easy to implement the [POSIX file system API](https://www.gnu.org/software/libc/manual/html_node/File-System-Interface.html#File-System-Interface)
- to be able to implement the [DAG interface](https://github.com/ewasm/prima-design/blob/master/dag_interface.md) (or replace it) 

Each storage location can roughly be thought of as corresponding to a file. Locations are open by using there name and locations are manipulated by the Stream API. 

## open
opens a storage location give its name, returning a stream reference

**Parameters**
- `nameOffest` **i32ptr** 
- `namelength` **i32**

**Returns**
- **i32ref** returns 1 if the location was opened. If the location is empty it returns -1


**Returns**
- **i32ref**

## delete
deletes a storage location given its name
**Parameters**
- `nameOffest` **i32ptr** 
- `namelength` **i32**
**returns**
- **i32** returns 1 if the location was deleted else returns -1

# Stream API
## close
closes a stream given its reference
**Parameters**
- `streamRef` **i32ref**

**Returns**
- **i32** returns 1 if the location was closed else returns -1

## read
reads data from a stream into memory
**Parameters**
- `storageRef` **i32ref** the storage location reference
- `dataOffset` **i32ptr** where in memory to start writing data to
- `dataLength` **i32** how much data should be read into memory

**returns**
- **i32** returns the number of bytes read. If the location was not open -1 is returned

## write
**Parameters**
- `storageRef` **i32ref** the storage location reference
- `memoryOffset` **i32ptr** a pointer to the memory location to start reading from
- `length` **i32** how much data should be read from memory

**returns**
- **i32** returns the number of bytes written to storage. If the location was not open -1 is returned

## getPosition
gets the current position in number of bytes of a given stream
**Parameters**
- `storageRef` **i32ref** the storage location reference

**returns**
- **i32** the current position or -1 if the location is not opened

## seek
change the position of the stream
**Parameters**
- `storageRef` **i32ref** the storage location reference
- `offset` **i32** offset to the current stream
- `whence`  **i32**  the position in storage to apply the offset to
    - 0 CURRENT
    - 1 BEGINNING
    - 2 END

**returns**
- **i32** the resulting position or -1 if the storage reference was not open


# Merkle Storage API
The merkle storage interface which is exposed directly contracts has the same api as above but with some extra methods. The idea is that merklization will be implementing using the base storage API and also expose the same storage API plus a few merkle storage specific method. The merklization contract is a "system level" contract. That is not any user space contract can become a merklization contract. The merklization contract is also metered.


## checkpoint
checkpoints the store

## revert
reverts the store back to the last checkpoint

## commit
removes the last checkpoint

**Returns**
- **i32** returns 1 if the storage was committed else return -1
