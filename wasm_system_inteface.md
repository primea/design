# Webassembly System Interface

The system interface defines which imports are avaible to webassembly programs
running inside Primea.

We define the following types:
- `i32`: same as `i32` in WebAssembly
- `i32ptr`: same as `i32` in WebAssembly, but treated as a pointer to a WebAssembly memory offset
- `i32ref`: same as `i32` in WebAssembly, but treated as a reference to a Primea object

### create_message
Messages can contain data which is read from memory starting at the `offset`
and going for `len` bytes.

**Parameters**

* `offset`  **i32ptr** - a pointer to the location in memory to start reading from
* `len` **i32** - the number of bytes to read starting from `offset`

**Returns**

* **i32ref** the refence to a the message 

### create_channel
Creates a channel and writes two port references to memory. Each port reference
is an `i32`

**Parameters**

* `locA` **i32ptr**
* `locB` **i32ptr** 

### bind_port
Assigns a byte array to a port and starts to listen for incoming messages on it.
The byte array is used as a name to retrieve the port in the future.

**Parameters**

* `offset`  **i32ptr** - a pointer to the location in memory to start reading from
* `len` **i32** - the number of bytes to read starting from `offset`
* `portRef` **i32ref** - the reference to the port being bounded

### unbind_port
Stops listening to a port and returns a reference to the port

**Parameters**

* `offset`  **i32ptr** - a pointer to the location in memory to start reading from
* `len` **i32** - the number of bytes to read starting from `offset`

**Returns**

* **i32ref** - the referance to the port being unbounded

### get_port
Given the port's name this returns a refernce to the port. 

**Parameters**

* `offset`  **i32ptr** - a pointer to the location in memory to start reading from
* `len` **i32** - the number of bytes to read starting from `offset`

**Returns**

* **i32ref** - the reference to the port being unbounded

### message_data_len
Gets the number of bytes contain in the message's data payload

**Parameters**
* `message` **i32ref** - the reference to the message

**Returns**
* **i32**

### load_message_data
Loads the message's data into memory

**Parameters**
* `message` **i32ref** - the reference to the message
* `writeOffset` **i32ptr**
* `readOffset` **i32ptr**
* `len` **i32**

### add_port_to_message
Add a port ref to a message

**Parameters**
* `message` **i32ref** - the reference to the message
* `port` **i32ref** - the reference to the port


### message_port_len
Gets the number of ports contained in the message

**Parameters**
* `message` **i32ref** - the reference to the message

**Returns**
* **i32**

### get_message_port_ref
Gets a port reference from a given message

**Parameters**
* `message` **i32ref** - the reference to the message
* `index` **i32** which port to in the message to load. The same port cannot be loaded twice

**Returns**
* **i32ref**

### send_message
sends a message on a given port

**Parameters**
* `message` **i32ref** - the reference to the message
* `port` **i32ref** - the reference to the port

### is_valid_ref
test is an i32 is a valid ref or not

**Parameters**
* `message` **i32ref**

**Returns**
* **i32**

### delete_ref
deletes a port or message refs 

**Parameters**
* `message` **i32ref**
